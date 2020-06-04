---
layout: post
title: "Microservices and Vendors - Managing Risk"
description: "Building our first microservice at Shapeways"
category: software
order: 9
tags: monolith-to-microservices architecture saas
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the sixth in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>
<figure>
  <center>
      <img src="/assets/img/microservices-for-vendors/vending-machines.png" />
      <figcaption>Would You Really Make a Snickers?</figcaption>
  </center>
</figure>
When discussing microservices, the common frame of reference is for internally-developed applications.  While core functionality is certainly the most important part of your business (and therefore the most critical to get right), I’d ask you to spare a thought for functionality that you’re outsourcing to others.  While things like tax collection, payment acceptance, and logistics services may not be the most critical portions of your stack, getting their implementation right the first time will pay dividends in the future.

In a [previous post](/software/first-services-peeling-the-onion/), I described two services that we were going to implement - tax, and equipment monitoring.  This post is ostensibly about the implementation of that service, but really it’s simply a convenient example to illustrate the process we used to decouple our vendor from our application and de-risk our choice to outsource this functionality.

### Choosing to Use a Vendor

There are a ton of good reasons to outsource functionality to a SaaS provider - they provide full functionality up-front, eliminate the need to spend development capacity (and cost!) implementing something that already exists, and permit your team to focus on delivering the core business functionality required to move your company forward.  

However, it’d be naive of us to ignore the drawbacks to leveraging a 3rd party provider.  For one, they cost money on an ongoing basis - you have to pay them every month, vs just paying up-front for the development time if you’d have built it yourself.  Vendors can also change the nature of the agreement every time a contractual period expires, either increasing costs or removing functionality from the current payment tier you’re using.  Finally, vendors come with risks of their own - both the technical (maintenance windows, unplanned outages, etc) and the existential (companies fail all the time).  If you’ve done the math, and determine that leveraging a 3rd party vendor for a portion of your business, then you’d be well-served to de-risk that vendor in your software architecture.

### Microservices for Vendors

De-risking the choice of using a vendor means striving towards a system that is resilient to the vendor going away.  The simplest way to achieve this is to decouple the usage of the vendor from the core functionality that they’re providing.  Microservices are a natural fit for this - you can create a service that offers an interface for the core functionality, and then plug the vendor into the back of the service to actually provide the functionality detailed in the service.  This natural decoupling allows for you to change vendors without impacting the other services in your mesh.  

### Define Core Functionality

We leveraged this pattern of decoupling to implement our Sales Tax service.  Our first step was to define the core functionalities that the Sales Tax service would provide.  We defined three pieces of functionality that the service would implement

 1. Provide Sales Tax rates based on customer shipping address.  This service needs to provide up-to-date rates for global taxes, including US Sales Tax, VAT, and GST.

 1. Allow for locale-based tax exemption, and provide a mechanism for users to submit their government documents for automated tax exemption on our platform.  This needs to work for at least US users, where exemption is more complex and based on individual states.

 1. Automatically submit monthly tax reports to state governments.  Our finance organization is not staffed to manually file monthly taxes in all 50 states, where we are now required to collect tax due to Wayfair vs South Dakota. 


With these core functionalities defined, we now have a requirements list we can use to evaluate potential partners.  We opted for [Avalara](https://www.avalara.com/us/en/index.html), though there are several other companies in the landscape that provide similar services.  

### The Provider Pattern

Functionality defined and vendor in place, it’s time to talk about the design of this service.  The internally facing API and persistent storage of a vendor-based microservice are no different than any other service in the system - you’ll face the same decisions around API format, storage choice, and data model.  Where things get a bit different is the provider interface.  
<figure>
  <center>
      <img src="/assets/img/microservices-for-vendors/provider-pattern.png" />
      <figcaption>Architecture of our Tax Service</figcaption>
  </center>
</figure>
The [Provider Pattern](https://www.codeproject.com/Articles/550495/Provider-Pattern-for-Beginners) calls for us to define a generic interface for a provider, which is implemented by providing service (in this case, our vendor).  The previously described core functionalities represent the interface - we simply had to build a mapping from that interface to Avalara’s APIs for retrieving tax rates, filing taxes, and handling user tax exemption.  With this in place, we now had a tax service which was capable of handing our own internal taxation logic (where we capture tax vs not, who is exempt vs not), with the actual retrieval of rates, filing of tax documents, and process for exemption handled by Avalara.  If we have to change tax providers some day, all we’ll need to do is implement their portion of the provider interface.  

This decoupling makes it so that a change in vendor has no knock-on effects to our service mesh beyond the individual service that is implementing the vendor API.  Compare this, say, to implementing the Avalara API everywhere that we require sales tax.  If we changed providers, we’d be forced to modify every single place we’d implemented the Avalara API, which would both take longer and require more teams to become involved.  De-risking behavior enables us to use our vendor with confidence that, if they go away, we’ve minimized our risk of impact to our platform.
