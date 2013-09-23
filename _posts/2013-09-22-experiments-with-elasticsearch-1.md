---
layout: post
title: Experiments with Elasticsearch - part 1
categories: 
- search
- code
- tech
keywords: search, elasticsearch
intro:
 Part 1 of what's intended to be a longer series of posts exploring, by way of example, the features of Elasticsearch. All code below assumes that Elasticsearch is running locally using the default settings. 
---

### What's an Elasticsearch and why might I want one?

Ah, you may have come to the wrong place. [Elasticsearch](http://www.elasticsearch.org/overview/) is a relatively new (circa 2010) Java-based search engine built on top of the well-established [Lucene](http://lucene.apache.org/) library. For developers it's language agnostic as its primary means of interaction is by way of calls to a JSON API over HTTP with call wrappers/abstraction libraries available for many languages. In short, you can use it to create your very own search application. Cool huh?

### But I already have Solr

Elasticsearch is more flexible and approachable than Solr. If you already have a massive Solr index and are delighted with it, you have no reason to shut it down but if you're starting from scratch, and want to build on top of Lucene, this is probably the way to go.

### Getting started

As I said up in the intro, I'm assuming you've already downloaded Elasticsearch and got it running. If not, [the setup guide on the official site](http://www.elasticsearch.org/guide/reference/setup/) is your friend.

### Clean up - getting rid of the previous attempt

I should probably assume that you're starting from scratch. However, I'm going to go right ahead and assume that I'll be reading this myself at some point and that I'll have made a mess of my development box and will need to do cleanup before I make even more of a mess so I'll proceed on that basis. If, dear reader, that doesn't apply to you at all then I'll see you in [the next section](#create_a_shiny_new_index).

Jump into terminal and run the following command to delete a search index called 'test'.

    curl -XDELETE 'http://localhost:9200/test/'

(If you get an error at this point that isn't complaining that there's no server at localhost:9200, it probably means you don't already have an index called test, which is fine, carry on.)

### Create a shiny new index

Great, we threw our old work away, now we need to make a new index of the same name, using this command:

    curl -XPUT 'http://localhost:9200/test/'
    
### Generate the mapping configuration

_Wait, what? I want to play with indexing documents first!_

Yeah, well - it's better if you have some idea of structure first otherwise you'll have to junk the index (again) and start over, especially if you want to play with facets. Facets need to be excluded from the keyword analyzer and, left to its own devices, Elasticsearch will pass everything to the analyzer as it's the thing you're most likely to want it to do. This becomes particularly important if you want to set up facets with multiple words (people's names for example), otherwise each word will be treated a separate facet, e.g. Mr (290), John (25), Smith (64).

Let's go ahead and map out a document type called article:

    curl -XPOST 'http://localhost:9200/test/article/_mapping' -d '
    {
      "article" : {
        "properties" : {
          "title": {"type" : "string", "index" : "analyzed"},
          "text": {"type" : "string", "index" : "analyzed"},
          "contributors": {"type" : "string", "index" : "not_analyzed"}
        }
      }
    }'
    
Note that `contributors` has been excluded from the analyzer - despite being defined as a string, we're actually going to use it to store an array of strings to reflect that an article may have been written by more than one person.

### Create a test document (or 3)

Now we can generate some test data. We need something to search for and these should come in handy for the tests I have in mind:

    curl -XPUT 'http://localhost:9200/test/article/1' -d '{
        "title" : "test",
        "text" : "trying out my new search index",
        "contributors" : ["Liz Conlan"]
    }'
    
    curl -XPUT 'http://localhost:9200/test/article/2' -d '{
        "title" : "test too",
        "text" : "more grist for the search index mill",
        "contributors" : ["Liz Conlan", "Robert Brook"]
    }'
    
    curl -XPUT 'http://localhost:9200/test/article/3' -d '{
        "title" : "index baiting",
        "text" : "new doc to search on, must not mention the i-word in here"
    }'

### Simple search (with facets)  

Look for a word that appears in all of the documents:
    
    curl -X POST "http://localhost:9200/test/_search?pretty=true" -d '
      {
        "query" : { "query_string" : {"query" : "index"} },
        "facets" : {
          "contributors" : { "terms" : {"field" : "contributors"} }
        }
      }
    '
    
### Using a facet to refine the results

Same query as before but filtered to only retrieve the document co-created by Robert:

    curl -X POST "http://localhost:9200/test/_search?pretty=true" -d '
      {
        "query" : { "query_string" : {"query" : "index"},
        "filter" : {
          "query" : {
            "term" : { "contributors" : "Robert Brook" }
          }
        }
      }
    '

### Searching within a specific field

Let's look for the word `index` in the title field:

    curl -X POST "http://localhost:9200/test/_search?pretty=true" -d '
      {
        "query" : {
          "query_string" : {
            "query" : "index",
            "default_field" : "title"
          }
        }
      }
    '
    
### Enabling simple query operators

I can haz [dismax](http://searchhub.org/2010/05/23/whats-a-dismax/)!

First of all the fail condition - let's look for 'new search':

    curl -X POST 'http://localhost:9200/test/_search?pretty=true' -d '
      {
        "query" : {
          "query_string" : {
            "query": "new search"
          }
        }
      }
    '
Argh - it brought back all the things! Looks like it did an `OR` so I guess I meant `AND`. Let's be more specific and try that again:

    curl -X POST 'http://localhost:9200/test/_search?pretty=true' -d '
      {
        "query" : {
          "query_string" : {
            "query": "new AND search"
          }
        }
      }
    '
    
Wait, still not there - let's get dismax to help and turn this into a phrase search to fetch the single document I was after:
  
    curl -X POST 'http://localhost:9200/test/_search?pretty=true' -d '
      {
        "query" : {
          "query_string" : {
            "query": "\"new search\"",
            "use_dis_max" : true
          }
        }
      }
    '

### Building more complicated things

How about an old favourite - "`search` without `index` (in the text field)":

    curl -X POST 'http://localhost:9200/test/_search?pretty=true' -d '
      {
        "query" : {
          "query_string" : {
            "query": "search -index",
            "default_field" : "text",
            "use_dis_max" : true
          }
        }
      }
    '