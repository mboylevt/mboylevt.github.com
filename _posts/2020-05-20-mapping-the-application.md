---
layout: post
title: "Mapping the Application"
description: "Ok, you've decided to migrate to microservices.  What's the next step?"
category: software
order: 3
tags: monolith-to-microservices architecture 
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the third in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>

The decision to move to microservices: made.  The strategic leadership role: filled (by me), Now, it’s time to start the fun stuff - coming up with a plan to make this transition.

In the case of Shapeways, we were starting with the software equivalent of a Warboy War Rig from Mad Max: Fury Road.  Over the years, the base application had pieces of functionality bolted on to serve one business need or another.  And, like a War Rig, while the additions didn’t necessarily belong on the application in the first place, it was hard to deny that the end result was both functional and kinda cool.
 
<figure>
  <center>
      <img src="/assets/img/mapping-the-app/madmax-guitar.gif" />
      <figcaption>Unwieldy and Cobbled Together?  Yes.  Cool?  Also yes.</figcaption>
  </center>
</figure>

Pulling this apart was a daunting task.  So, rather than blindly disassembling to find connection points, I took a product-focused approach and asked: What were the key user functions that this application provided?  Turns out, there were a few:

 * *Model Upload* - the ability for a user to upload a 3D model to shapeways.  We process the model to determine printability in all our materials, and price in each.

 * *Order Placement* - the ability for a user to place an order on our system.  This is basic ecommerce functionality, but also critical to the business.

 * *User Profiles* - a place for our users to track their models, orders, and manage their personal data.  This includes saved payment methods, shipping addresses, and tax exemption details if appropriate.

 * *Marketplace* - this is where our users place their own creations for sale.  This includes managing their digital shops, designer markup, and product detail pages.

 * *Inshape* - our [ERP](https://en.wikipedia.org/wiki/Enterprise_resource_planning) system responsible for receiving orders, tracking them through production, and fulfilling them to our customers.  Inshape is an entire application unto itself with layers of complexity, so we decided to consider the mapping of its key features separately.  This work will be covered in a separate post later in the series.

Armed with this mapping, we had a much clearer picture of potential future service functionalities.  The next step was to come up with an approach to getting started with services.  Since we were starting with an existing application that was designed to serve a very specific purpose (facilitating the production of 3D printed parts), we chose to separate functionalities of our system into two groups: core, and support.  

Core functions would be the ones specific to our industry and team - some examples are 
* determining 3d file printability and pricing, 
* preparing and optimizing 3d files for production, 
* maintaining an effectively infinite catalogue of Just-In-Time manufacturable parts.

Support functions would be systems required to operate this business, but not core to what we do - things like 
* tax calculation and exemption
* checkout and payment collection
* hosting a marketplace
* financial record keeping

Put more bluntly: core functions were things that made us special, whereas support functions were things we could buy or leverage from the OSS community.  

Once we had a first pass at functionality lists, and a mental model by which to sort them, we started to think about how they all tied together.  Our goal was to identify functions that had high reuse potential - if we built it once, we’d be able to plug it into multiple portions of our application to solve problems.  The next post in the series will focus on this process.
