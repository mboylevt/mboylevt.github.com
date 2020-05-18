---
layout: post
title: "Monolith to Microservices - Introduction"
description: "This is the first post in a series on Shapeways' transition from a monolith to microservices."
category: software
order: 1
tags: monolith-to-microservices api devops
image:
  feature: microservices-header.jpg
---
<div style="font-size: 6;"><i>This is the first in a series of posts about the transition from a monolith to microservices that I led at Shapeways as the Vice President of Architecture. This series will dig into the decision process around making the switch, selling it to the executive team, getting the organization on board, planning for success, and executing the change. I hope you find it useful!  You can find the series index <a href="/monolith-to-microservices">here</a>.</i></div>

In the fall of 2018, Shapeways decided that we were going to take our business in a new direction..  We wanted to expand from being a 3D Printing service bureau to being a technology platform, connecting business, consumers, and industries through our global supply chain and production facilities. As we weren’t yet sure exactly what group of products and features would be valuable to our future customers, we wanted to be able to test things quickly, built out simple MVPs to validate, and be comfortable tossing aside products that didn’t fit the bill.

With this goal in mind, we looked to our current technology offering...and found it not up to the task.  The application powering Shapeways.com and Inshape (our in-house ERP system) was built in 2010 as a monolithic PHP application.  It had carried us dutilfy for 8 years, but it was showing its age.  It was written using a custom PHP framework that had built up some rust over the years, and it showed.  While reliability and uptime were fine, our site performance was below industry standards, and (perhaps more importantly) it was very difficult to develop new features at speed. 
<br>
<figure>
  <center>
      <img src="/assets/img/microservices-intro/monolith.gif" />
      <figcaption>A Code Review Session with our Monolith</figcaption>
  </center>
</figure>
<br>
This wasn’t going to work for our new direction.  However, we did have two advantages in our existing monolith 

 1. The business logic to run our internal model processing, order production, and customer management was sound, robust, and performant compared to the rest of the system
 1. We had built a REST API into the monolith to enable our customers to add 3D printing to their own apps and websites.  This API had been active since 2013, and was in regular use by some of our largest customers. 

This API exposed all the key features required by our core business, was robust and reliable, and provided us with an effective (if not optimal) interface to our complex business logic. This meant that we could effectively draw a black box around the majority of the existing platform, and iterate on top of it, while continuing to improve the original in a separate workstream.

Once we knew that we had a solid foundation to build on, we began exploring different potential architectures to fit our business needs. Given the desires for speed of iteration, ability to test new functionality quickly for a variety of customers, and the assumption that these customers would have very different needs, we determined we needed an architecture that would lend itself to composability, reuse, and fast scalability.  When combined with the fact that we already had an API providing a clean interface to the model upload and purchase functionality of Shapeways, and that the microservices community and ecosystem is mature and robust, a microservice architecture emerged as a clear path forward for us. 

The next post in this series will elaborate on the choice of a microservices architecture, including business needs, pros and cons, organizational structuring, and the final decision process. 

