---
layout: post
title: Amazon's latest Kindle backend tweaks
categories: 
- kindle
- tech
- money-saving
keywords: kindle, iOS, Amazon
intro:
  In the run-up to Christmas, Amazon appear to have made some pretty serious changes to the Kindle backend. Some of these immediately and obviously change things for the better, some are just different and others make things more... complicated for those of us who thought we understood how it worked previously.
  
  
  _(Disclaimer&#59; Most of the app features discussed here currently appear to be iOS only, but presumably will be rolled out to Android in due course. I have no insider information on this, just a borrowed Android phone, internet access and what seems like a reasonable guess.)_
---

<div markdown="1" class="intro">
  

  
</div>

### Personal Documents and Kindle apps

In addition to the other recent(ish) change of intercepting incoming emailed documents and storing them in a Personal Documents Archive (with associated storage limits) so that you can resend them straight from your Amazon account without needing to find the original file and wait for the email to arrive all over again, Amazon have now extended this document-sending ability to the Kindle app as well as the Kindle hardware devices. This is fantastic as it now means you can send a document to whichever device you happen to have to hand - if you already have your hands full of coffee and paperwork, you can send the latest meeting minutes to your iPhone rather than it printing out yet another stack of paper or trying to balance your actual Kindle on top of a slithering heap of stuff that you're trying not to spill coffee over while trying to remember which room you're supposed to be in. (Also stuff from indie publishers like [The Pragmatic Bookshelf](http://pragprog.com/) can be directed straight to your reading gadget of choice, or sent along a little later via the Personal Document Archive page.)

Of course if you have a wireless network handy, you can pull as well as push - once the document has arrived in the Personal Documents Archive, it is available in Archived Items on the Kindle hardware and the most recently updated apps.

### Revamped email addresses

To make it even easier to ping documents around, Amazon have implemented a system of a per-device email addresses. When you update the Kindle app and start it up for the first time, Amazon automatically generate a new (customisable) email address for your device; mine defaulted to adding some numbers on the end of my usual username. Now you can email documents directly to a specific device.

One oddity of this update is that the free.kindle.com addresses seem to have disappeared. It's no longer mentioned anywhere in Manage Your Devices or Personal Document Settings or on any of the latest documentation pages. At this point it seems unclear whether Amazon are planning to withdraw this altogether or are just not talking about it to stop the documentation turning into an overlong page of waffle (much like this one).

### Limiting Whispernet for Personal Documents

From the documentation, it appears that documents sent to a hardware Kindle using a standard @kindle.com address will be downloaded over wifi (with no charge incurred) rather than using Amazon's Whispernet 3G service if the device has a working wifi connection. However if you prefer to be certain that you are not going to run up unexpected charges in the event of your wifi network suffering a badly-timed glitch, the damage-limitation mechanisms are still in play (if not particularly well sign-posted).

These days the only email addresses you have manually added to a whitelist - or the Approved Personal Document E-mail List as Amazon terms it - can send you stuff anyway so spam shouldn't be too much of a concern and the old Maximum Charge Limit which prevents a single document delivery from costing more than expected remains in place. To be absolutely certain there will be no unexpected bills, Amazon allow you set the Maximum Charge to 0.

Or if you only ever used the @free.kindle.com address and are worried it might disappear for good, you can go one better. When you click on Edit within the Whispernet Delivery Options section of the Personal Document Settings page, you are now offered a checkbox labelled "Enable delivery to my Kindle over Whispernet", uncheck it to disable document delivery over 3G altogether. In theory now your @kindle.com address will act as per your @free.kindle.com address but without the extra typing.

Happy Kindle tinkering!