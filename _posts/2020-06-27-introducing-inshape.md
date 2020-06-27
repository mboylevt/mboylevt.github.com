---
layout: post
title: "Introducing Inshape"
description: "Shapeways' Secret Sauce"
category: software
order: 8
tags: monolith-to-microservices architecture frontend
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the eigth in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>
<figure>
  <center>
      <img src="/assets/img/introducing-inshape/manufacturing.jpg" />
      <figcaption>Manufacturing has come a long way.</figcaption>
  </center>
</figure>
This series of posts thus far has focused on a common, well-understood space - e-commerce.  Sure, we’re selling 3d printing vs traditional goods, and sure, we’re manufacturing them in real time...but just as an API provides a defined interface to an underlying service, so too does e-commerce define a standard web practice: buying stuff on the internet.  You’ve got products, they’ve got prices, and you exchange your hard earned cash to get your hands on them.

However, as you can probably guess, Shapeways isn’t a standard e-commerce company.  We don’t keep inventory - we manufacture every single item that’s ordered from us, on demand (always fresh, never frozen).  What’s more, we have an infinite catalogue of products - every time someone uploads a new file to our site, our catalogue grows.  To support the variety of materials and the volume of orders we receive, we have a globally distributed supply chain performing just-in-time manufacturing of parts for us.  These parts have to be produced, processed, assembled, QA’d, packaged, and shipped all over the world.  There’s a purpose-built suite of software tools that make this possible - we call it Inshape.  

### Inshape

A customer journey with Shapeways starts with visiting our site, and ends with them receiving their 3d printed product(s).  Shapeways.com and the Powered by Shapeways applications mentioned in prior posts are responsible for everything that happens between a customer visiting our site and placing their order.  Inshape is responsible for everything else - what happens between the order being placed, and the customer receiving it.  Inshape’s functionality can be broken down into a few different silos

 * __Analysis__ - responsible for determining if files can be fabricated in the material in which they have been ordered.  If they cannot, we will work with a customer to ensure that it’s able to be produced.  This step also provides the opportunity for file optimizations, such as hollowing, orientation, and scaling

 * __Routing and Planning__ - responsible for determining where in our global supply chain the customer part will be produced.  Determining factors include: who can make the material, how large or small the part is, the customer’s shipping address (we try to produce as close the customer as possible to get them their parts quickly), required certifications (such as ISO:9001), and more.  

* __Manufacturing__ - responsible for guiding the manufacturing of our orders.  This includes job preparation (including packing and nesting, which will be a separate article), workflow management, contract management, part traceability, and quality assurance


* __Fulfillment__ - responsible for collating multi-part orders, final QC checks, assembly, kitting, packing, shipping, and delivery.  

### But...why would you build this?

Inshape is what’s known in the business world as an [Enterprise Resource Planning](https://en.wikipedia.org/wiki/Enterprise_resource_planning) system.  ERP systems power the backend of most businesses - chances are, if you’re dealing with customer management, inventory, and physical products, you’ve likely got an ERP system helping you out.  Like our e-commerce example above, this isn’t a unique problem to Shapeways - there are commercially available ERP systems from big companies you know.  Oracle, SAP, Siemens, and others all sell ERP systems.  So, why didn’t we use one of them?   

Two reasons:


* __Cost__
  * ERP systems from big companies come with big price tags.  At a few grand per seat, and a 20-30% annual recurring cost on those seats, we’d have spent a good portion of our company budget just on this software.  
  * These softwares are incredibly full-featured, doing everything from accounting to CRM to Sales to forecasting.  This was way more than we needed - we had neither the scale nor the staffing to leverage these features in way that made sense for Shapeways


* __Business__ 
  * Most ERPs rely on your business selling a finite catalogue of products.  Shapeways does not do this - as mentioned above, every time someone uploads and purchases a new model, that’s a new entry for us.  On average, this happens a few hundred to thousand times a day.  Software that would increase in cost as our model base grew didn't make sense for us.

  * At the time, no ERPs were available that specifically targeted the 3d printing space.  There were no machine connections, no file analysis or preparation tools, and no concept of visual sorting or other tools that are specific to the 3DP production cycle.  

### Inshape in a B2B World

Inshape itself is an application that’s built into our monolith - it’s in the same repo, uses the same database, and is deployed alongside shapeways.com.  This made it easy to work with at the start, but now it exhibits all the characteristics of our commercial applications, which have been discussed in previous posts.  When we made the business decision to pursue B2B customers, we knew that these customers would want access to Inshape.  While the commercial front-end drives volume into our business, Inshape is really the engine that drives Shapeways.  Without it, we’d never have been able to scale to producing thousands of parts per day.  

So, we have partners that want access to inshape...but usually not to all of it.  Some will have manufacturing capabilities, and only want our manufacturing component to help drive their workflows.  Others will have their own internal supply chains, and simply want to plan trays and assign volume to their production partners.  We want to have the ability to treat these modules like lego blocks, snapping them together on a per-partner basis to provide a tailor-made solution for our partners.  You can probably see where this is going...composible features implemented as microservices.

### Same Song, Different Dance

We approached splitting Inshape into services using the same approach described [earlier](/software/mapping-the-application/) - identify core vs support functionality, and begin breaking them up into potential services. With Inshape being the Secret Sauce that makes Shapeways go, almost all of its functionality was considered core. That settled, we started to identify user-facing front-end services, and from there figured out the backing services we’d need to drive them.  Using the silos above, we decided to start with our Manufacturing functionality.  Manufacturing is the tool that is used the most by our partners, and also has the most well-defined scope for its interactions. And, honestly - it was one of our oldest pieces of software at the company, and was badly in need of some re-working. The next post in this series will focus in on how we approached building a new Manufacturing Execution System at Shapeways - stay tuned!

