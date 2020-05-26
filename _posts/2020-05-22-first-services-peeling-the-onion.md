---
layout: post
title: "Identifying our First Services"
description: "Identifying the first functional candidates for microservices"
category: software
order: 4
tags: monolith-to-microservices architecture 
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the fourth in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>
<figure>
  <center>
      <img src="/assets/img/peeling-the-onion/peeling-onion.jpg" />
      <figcaption>Progress sometimes comes with tears.</figcaption>
  </center>
</figure>
Application map in hand, we were ready to identify our first functional candidate for conversion to a service.  If you haven’t yet, now’s the time to reference the [previous article](/software/mapping-the-application) in this series for the definitions of core vs supporting functionalities.  I came up with two guiding principles for selecting service candidates in the early days.  The first service candidates should be either
* Support functionality that’s either poorly implemented, or has a demonstrably superior 3rd party provider.  
* Non-essential core functionality that we haven’t yet implemented

 As this was our first service, I wanted permission for us to make mistakes.  By choosing either a support functionality that already exists or a core service that we haven’t implemented, we’re giving ourselves permission to extend deadlines, change implementation strategies halfway, and generally experiment with building, deploying, and running microservices.  Basically, I wanted to develop our first service without the pressure of getting it right the first time.  

### Looking for candidates

In looking for candidates, we decided to take an outside-in approach: starting from the external scaffolding of our monolith to reimplement as services, much like peeling an onion.  The idea being that, as more and more functionality is peeled off the monolith and reimplemented as services, the monolith is slowly whittled away and eventually replaced at the core. This is a version of the [Strangler Pattern](https://martinfowler.com/bliki/StranglerFigApplication.html), where the new implementation move in like an invasive species moves into a new environment and takes over from the natives species.  

Rather than choosing between a core and a supporting service, I decided that I would seek out one of each to build.  We had the capacity on the development team to start with two small services, and I wanted to build as much momentum as I could in the organization towards this architectural shift.  To that end, I wanted to deploy services that would improve the work lives of folks at Shapeways outside of the software team, the idea being that getting folks around the company excited about what services could do for them, I was building support for allocating more resources to the monolith->service transformation.  This was admittedly a bit of a political play, but if you think that you can get things done by the strength of ideas alone, I’ve got a few bridges on the East River looking for new owners. 

### Support Functionality

For supporting functionality candidates, we had a few options

* Tax Collection - We were managing global tax rates and exemption statues, with our own internally managed tax tables.  These tables were manually updated monthly by a developer using SQL.  There are several companies offering rates and exemptions as a service.


* Shipping and Logistics - We were using the Shippo, UPS, and EasyPost APIs in separate places throughout our application to manage shipping label creation, address validation, leadtime calculation, and shipping rates. Having several tools to do this resulted in imperfect addresses, which sometimes lead to shipments begin rejected by one service that were fine by another.  


* Payments - we had individually implemented the APIs for Paypal and Braintree prior to their merger, meaning we had a few different payment options to manage.  Furthermore, we were manually handling bank transfers and had no option for ACH, which put a ton of workload on our accounting team. 

After some consideration, we decided to build a tax collection service.  In addition to the obvious opportunity for improvement on our existing implementation, there were two other driving factors in its selection


1. [South Dakota vs Wayfair](https://en.wikipedia.org/wiki/South_Dakota_v._Wayfair,_Inc.) was decided in favor of South Dakota.  In short, this meant that companies would have to charge sales tax in ALL US States and Territories rather than just the ones where they had nexus (business presence).  For Shapeways, this meant that instead of only charging tax in New York State, Washington State, and the District of Columbia, we would instead have to charge...everywhere.  Not only did this increase the number of tax tables that we would have to maintain exponentially, but it also required us to now file taxes in effectively 20x the number of places. We had already identified [Avalara](https://www.avalara.com/us/en/index.html) as a vendor who could help both manage rates and file taxes for us automatically - this sealed the deal.


1. We had begun launching external shopping experiences that needed to charge sales tax.  These were implemented in Shopify, and therefore couldn’t leverage our internally managed tax tables because there was no connectivity to do so. We were instead doing a poor job of estimating taxes to be collected, and creating financial uncertainty for the company. 

### Core Functionality

In terms of core functionality, there was a slimmer set of options.  We immediately ruled out anything that was going to impact the ability of employees, vendors, or customers to engage with our platform.  This ruled out….well, almost everything.  However, there was one piece of functionality that we’d always wanted to build but never did - real-time equipment monitoring.

The 3D printing space has a spectrum of production technologies and companies making them. All machines have a few things in common - they manufacture products, they take in consumable materials, and run for some time with either successful or failed jobs.  Some more modern companies provide APIs from which to extract this information.  Others have logfiles which you can process to glean data.  Others still require more innovative approaches, such as building external sensors and embedded devices to track job data.  

We had long wanted to build an application that understood how to consume the available data we had from all machines and unify it with our ERP system to provide a complete picture of our production environment. As this required a specialized understanding of each machine in our stable, plus would require a new section of our monolith in the old world, we never got around to it.  However, this was a perfect candidate for our first core service.  It would provide new data about the production of our parts that we could unify with our existing ERP data, giving both our internal production teams and our customers deeper insight into the quality of the parts that we were producing

We had identified our first two services - Tax, and Equipment Monitoring.  The next post will focus on our approach to implementing the tax service.
