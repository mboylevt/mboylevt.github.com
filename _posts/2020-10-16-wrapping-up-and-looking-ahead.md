---
layout: post
title: "Wrapping Up and Looking Ahead"
description: "A Project List from Beyond the Employment Gap"
category: software
order: 9
tags: monolith-to-microservices architecture
image:
  feature: microservices-header.jpg
---

<div style="font-size: 6;"><i>This is the ninth and final post in a series about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>
<figure>
  <center>
      <img src="/assets/img/wrapping-up/wrap.jpg" />
      <figcaption>Low-Carb!</figcaption>
  </center>
</figure>

Regular readers (hah) of this blog will have noticed that I have ended my time at Shapeways.  You can read all about it [here](https://mboyle.org/software/the-parting-glass/) if that’s of interest to you.  With that in mind, I wanted to draw this series to a conclusion, and talk about some things that are on the horizon for Shapeways’ continued journey towards microservices, and what I hope to see them continue to do as a fan and cheerleader from the sidelines. 


### What We’ve Done So Far
<figure>
  <center>
      <img src="/assets/img/wrapping-up/memory-lane.jpg" />
      <figcaption>It's been fun!</figcaption>
  </center>
</figure>
Before we talk about where we’re going, I’d like to take a moment to reflect on where we are.  At the outset of this blog series, all of Shapeways’ production web properties were running out of a single monolithic codebase.  This was difficult to work with, but also functional, and was capable of running the business of the day.  It took a change in our approach to our business, plus a desire to expand from a service bureau to a technology platform, to make it worth the time and effort to convert this monolith into microservices.  

Once the decision was made, of equal importance was identifying a senior individual contributor to lead the charge.  It was imperative that this task would have 100% of their focus - this was a job too big for a manager to take on in tandem. That person happened to be me - this isn’t to say that I was the only one capable of doing it (far from it), but my familiarity with the business model, current software stack, and vision of the future was hard to compete against. 

The first step after figuring out who was going to do the work was how to break it down.  This required a critical look at our current offering, breaking down the functionalities of our monolith into standalone, digestible chunks.  Once we’d done that, the next step was to identify promising candidates for services from both our core and support functionalities, and then selecting + building our first series of microservices.  It was critical that these services were built with clear APIs and abstractions, and that their APIs were the only way to interact with the data for which they were responsible.  And, once you’ve got your services running: go build some more!  To date, we’ve built around 10 separate services at Shapeways, and have many more coming as the company continues to evolve its software.


### What’s To Come
<figure>
  <center>
      <img src="/assets/img/wrapping-up/looking-ahead.jpg" />
      <figcaption>Engage!</figcaption>
  </center>
</figure>
And there, my friends, is where my contribution to Shapeways comes to an end...of course, that doesn’t mean I don’t have opinions on the next steps that should be taken!  This was my baby after all :)  Here are, in order-ish, where I believe the team should focus future efforts as the usage continues to grow.

 1. *Completion* - Perhaps the most obvious next step of all is to complete implementing all core features of the monolith as microservices, rendering the monolith redundant and able to be retired.  This will give Shapeways permission to stop maintaining the monolith, freeing up people inside the Engineering organization to focus on tending and growing their new architecture.  Which is good...cause they’re gonna need it!


 1. *Service Discovery* - The system as it’s implemented now is not designed to scale immediately.  That’s not to say that it’s incapable of doing so, but rather that the implementation is incomplete.   One big area that remains unsolved for Shapeways is to implement [service discovery](https://en.wikipedia.org/wiki/Service_discovery).  There are many implementations of this, from the battle-tested but brittle DNS to the designed-for-the-task [xDS from Envoy Proxy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/upstream/service_discovery#:~:text=The%20endpoint%20discovery%20service%20is,endpoints%20from%20the%20discovery%20service.). 


 1. *Containerization* - We kept around our existing deployment system when migrating from the monolith, adapting it along the way to work for our newly virtualized microservice deployments.  However, true build+test virtualization is not yet complete for the Shapeways infrastructure.  Completing that task is vital to creating an easily scalable, resilient, and autohealing platform that can deliver reliability for Shapeways’ customers.  Personally, I’d be pursuing a combination of modern state-of-the-industry tools and standards such as [Docker](https://www.docker.com/), [Kubernetes](https://kubernetes.io/), and [CNAB](https://cnab.io/) to keep our infra healthy. 


 1. *API Gateway*  - as of the moment, Powered by Shapeways only exposes APIs internally for intra-service communication.  However, as the system continues to expand and functionality moves from the monolith to the service mesh, Shapeways will need to expose external API endpoints.  These would be used by external software products to programmatically retrieve things like model pricing, geometry, optimized files, or order status.  These pieces of information are core to building a truly interconnected platform - you can’t integrate with another companies ERP system if you don’t give them APIs!  Fortunately the [API Gateway](https://microservices.io/patterns/apigateway.html) pattern is here to help, and there are many commercial and open-source gateways available for easy installation and management of data.  These gateways not only provide ingress and egress for your data, but can also provide telemetry, security, and observability for your APIs. 
