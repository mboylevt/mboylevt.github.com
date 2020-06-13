---
layout: post
title: "A Brave New World - ModelIQ"
description: "Bringing Services to our Customers"
category: software
order: 7
tags: monolith-to-microservices architecture frontend
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the seventh in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>
<figure>
  <center>
      <img src="/assets/img/model-iq/danger.jpeg" />
      <figcaption>Our Intrepid Hero Sallys Forth</figcaption>
  </center>
</figure>
At this point, we had implemented two services - [Equipment Monitoring](/software/the-guinea-pig/) and [Sales Tax](/software/microservices-and-vendors/), and were running them on production.  We’d made a few mistakes along the way, and learned plenty about the differences in running services vs monoliths in production. In short, we felt ready for prime-time - our first user-facing service for our new business-focused platform.

### Selecting a Service
We were fortunate to have some valuable user input into this process - an actual customer.  We had recently signed a commercial agreement with our very first customer on our Powered by Shapeways platform, [Loctite](https://loctite.shapeways.com/), who wanted to leverage our new Powered by Shapeways platform to sell their new 3d printing materials.  Based on our requirements conversations with Loctite, we identified Model Upload as a key piece of functionality required by their end customers.  While we had existing Model Upload and Checkout functionality on shapeways.com, it was determined that the existing Checkout feature was far closer to what Loctite envisioned than the existing Upload feature.  Therefore, it was determined that we’d work on Powered by Shapeways Model Upload first, and Checkout second. 

An aside - this type of direct, actionable feedback from a paying partner is absolute *gold* - there’s no stronger prioritization signal available.  It’s worth your time to have these types of honest conversations with your business partners, particularly early in a products development cycle.  Often, a 30 minute phone call can replace a week of research.

### Defining the Business Need
<figure>
  <center>
      <img src="/assets/img/model-iq/sw-loctite.jpg" />
      <figcaption>What a Team!</figcaption>
  </center>
</figure>
Model Upload isn’t just something that Loctite wanted - it’s also one of the core pieces of our business, and what differentiates Shapeways’ from many of our competitors.  Our model processing system (MP) enables us to provide near real-time printability analysis, image rendering, and pricing to our end users in a wide variety of materials.  This is the first true high-intent step in our conversion funnel - a user that has a model file ready to go is extremely likely to make a purchase decision provided that the experience meets their expectations.  

Now, our existing upload flow worked, but it was built to support our old business, and suffered from performance issues and a tightly coupled design.  Some issues included:
 * No ability to filter materials.  Our existing flow was designed to sell _every_ material that  Shapeways has to offer.  Most partners want to center the user experience around their materials, not to promote other materials that they don’t offer.
* Tightly coupled with our model processing system.  The original Model Upload would read data inserted in the ActiveMQ queues of our MP system, and leveraged their content to drive the front end.  This type of tight coupling required that both systems exist on the same network, which would be impossible if a partner ever wanted an on-site or private installation of Powered by Shapeways.
* Bloated data objects.  Over the years, we continued to add more and more data to our model objects.  This included categorization, ability to sell to others, commercial versioning information, and comments left by consumers who had purchased.  These data are of no use to our new customer base, and required significant database cycles to compute. 

### Building the Service
It was time to design our new Model Upload service, or ModelIQ (short for Model Instant Quote).  After some initial discovery, we realized that ModelIQ was, largely, a frontend application built on top of our existing APIs.  ModelIQ has three prime pieces of functionality: 

1. Upload a model to Shapeways
1. Retrieve pricing information about that model in materials defined by the partner
1. Add a model in a material to a user cart for purchase

The public facing Shapeways API provides all the required functionality for uploading a model and retrieving its pricing information.  Further, the existing Shapeways e-commerce experience, while by no means perfect, was more than capable of managing the cart and checkout experience for the end customer. Since we were using the existing API to upload the model to Shapeways, our cart was aware of the model and able to execute the purchase.  

Choosing to use our existing APIs was not only a matter of convenience: they also act as an interface between our existing Shapeways.com infrastructure and our new Powered by Shapeways platform.  Part of the trick with building Powered by Shapeways was that we needed to leverage the existing functionality of Shapeways - we weren’t going to rebuild 12 years of software overnight.  These existing APIs gave us a portal to access this functionality without tightly coupling it into our new platform.  That way, once we’ve replaced the monolith itself, all we have to do is point our ModelIQ to the new service endpoints instead of the old monolith ones - no functional change required in ModelIQ.

### Green Field Frontend Development
Our monolithic approach to Shapeways.com, unfortunately, wasn’t constrained to our backend development.  We had giant CSS and JS files which defined features used willy-nilly across our experience.  While we had made efforts to componentize parts of our front-end, we never had time nor resources to devote to really chipping away at the monolith.

Presented with this new service offering, our front-end focused developers were excited to rectify the mistakes of the past.  They opted to use the [vue.js](https://vuejs.org/) framework for our new frontend services, and set about defining a reusable component library for common components.  In short order, they had components for model upload, model information, pricing, add to cart buttons, and dynamic headers and footers which would adjust to a partner’s brand details.  These components are the actual driving force behind ModelIQ - they pass user actions and data to the backend, which in turn orchestrates API calls to the monolith and returns the data back to the front end.  

You can try ModelIQ out for yourself right [here](https://powered-by.shapeways.com/loctite/model-iq)!  I encourage you to compare it to our older upload flow, which can be found [here.](https://www.shapeways.com/model/material-configurator/upload) 

### Velocity
I want to end with a note on velocity, specifically development velocity.  While microservices themselves don’t necessarily increase velocity as a matter of fact, they can definitely improve it in specific situations.  Take our situation for example- we had a ton of existing functionality wrapped up in a giant monolith.  The functionality is sound, but the architecture is weak - changes to the monolith itself take a ton of time, as code is tightly coupled, and changes are riddled with unintended consequences.  

In a situation like this, the cost of changing or implementing a new feature in the monolith is high.  However, if we can leave the monolith at rest, and simply leverage its existing functionality through APIs to drive our new features, the cost to implement drops dramatically.  In fact, this is the best of both worlds - you can leverage production-hardened code in a new, baggage-free application without fear of regression or unexpected behavior.

We re-worked our original Model Upload in 2017.  It took 4 developers close to a year to implement all of the various features.  ModelIQ is less feature-rich than our existing upload experience, but provides the exact same functionality that matters - users can upload, price, and purchase.  It was built from scratch by two developers in a single quarter.

That’s an improvement.
