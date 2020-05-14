---
layout: post
title: "How API's Are Powering the Next Industrial Revolution"
description: "Digital manufacturing (leveraging digital file inputs to perform on-demand manufacturing: CNC routing, 3D Printing, etc) has been picking up steam since the late 90’s and is now replacing previous technologies as “state of the industry”.  This has been driven by the desire for custom-manufactured products, tailored to the needs of the specific individual or project."
category: software
tags: shapeways, 3d printing
image:
  feature: 4th-revolution.jpg
---
Originally posted on the [Nordic APIs Blog](https://nordicapis.com/how-apis-are-powering-the-next-industrial-revolution/)
<br><br>
Watch a keynote @ 2019 Nordic APIs Austin Summit in Austin, Texas based on this blog here - [Distributed Digital Manufacturing – How APIs are Powering the Next Industrial Revolution](https://www.youtube.com/watch?v=WYXzJoewGLw)

<br>
<figure>
  <center>
      <img src="/assets/img/next-industrial-revolution/api-header.jpg" />
      <figcaption>APIs Power the Next Industrial Revolution</figcaption>
  </center>
</figure>
<br>

When someone says “manufacturing”, people generally think of old, dingy photographs from the 1920s.  Employees are hard at work, leveraging machines and tools to churn out products for society.  While that’s historically accurate, the factories of today look quite a bit different - people work side by side w/ machines, using technology to help coordinate manufacturing processes.  Now, a new industrial revolution is taking place, and it’s powered by software and APIs. 

## How’d We Get Here?

Since the Industrial Revolution, advances in manufacturing capabilities have been linked to quantum leaps forward in technology.  The <strong>First Industrial Revolution</strong> saw the harnessing of steam power, enabling long distance transport via rail or ship, and the rise of factories to serve as centralized manufacturing centers.  The <strong>Second Industrial Revolution</strong> was powered by science, both through raw products (gasoline, airplanes, chemical fertilizers) and through a major advancement in factory work -  the Assembly Line.  The <strong>Third Industrial Revolution</strong>, AKA the Digital Revolution, saw the rise of semiconductors, personal computing, and the Internet, which have shaped the world we live in today.  

<figure>
  <center>
      <img src="/assets/img/next-industrial-revolution/4th-ir.jpg" />
      <figcaption>A History of Technological Advancement</figcaption>
  </center>
</figure>
<br>

[Klaus Schwab](https://www.weforum.org/about/klaus-schwab), founder and executive chairman of the World Economic Forum, contends in his book [<strong>The Fourth Industrial Revolution</strong>](https://www.weforum.org/about/the-fourth-industrial-revolution-by-klaus-schwab) (January 2017) that we’re on the cusp of entering our next great leap forward.  This time, we’re powered by cloud computing, artificial intelligence, and connected IoT-style devices.  These technologies are already changing the way that we live, work, and interact with each other (think GPS, social media, Smart Homes, and distributed work platforms like Slack).  In just 15 years, cell phones have advanced from portable telephones to a pillar of most people’s daily lives: a supercomputer in every pocket, connected to a global network of knowledge, ideas…and cat videos.  Pretty sure [Gordon Moore](https://en.wikipedia.org/wiki/Moore%27s_law) didn’t figure that into his eponymous law.  

While the previous examples are focused on the impact on our personal lives, we shouldn’t forget that Industrial Revolutions of any kind also impact how we design, manufacture, and distribute products across the globe.  From the very first Industrial Revolution to the latest Digital Revolution, the impact on manufacturing has been to drive people and machines into large, centralized factories, which summarily produce the products of the new age.  During the Digital Revolution we improved our global supply chain, connecting our manufacturing centers across borders and oceans to improve the flow of goods. However these manufacturing centers still required large numbers of people operating in close confines with machines and assembly lines to complete their tasks.  Now, with digital connectivity powered by APIs, that’s beginning to change.

## The Rise of Digital Manufacturing

But first, let’s define <strong>Digital Manufacturing</strong>.  Digital Manufacturing is, simply put, a design and manufacturing process centered around <strong>computers</strong>.  Creation, simulation, analysis, production inputs, and physical outputs are all handled by digitally-defined processes.  This is different from traditional manufacturing processes, which are made using non-digital processes.  Traditional manufacturing processes include die stamping, extrusion, and injection molding, while digital manufacturing processes include 3D Printing, CNC milling, water jetting, and laser cutting.

## Open Platforms Powered by APIs
<figure>
  <center>
      <img src="/assets/img/next-industrial-revolution/connected-devices.jpg" />
      <figcaption>Everything is Connected</figcaption>
  </center>
</figure>
<br>
While APIs themselves are nothing new and shiny, it’s important for us in the software world to remember that it takes time for advances in our field to propagate to others.  While RESTful APIs, microservices, and distributed applications have been with us for some time, they’re just starting to make their way into less software-heavy fields, such as manufacturing.  The digital manufacturing space, despite its futuristic technology, has historically shared the same constraint as the traditional manufacturing world: centralized manufacturing centers, with machines closely operated and controlled by humans.  

While <strong>automation</strong> certainly exists in this world, it still requires on- or near-site monitoring, and is almost always a closed-off, proprietary system with no potential for integration into 3rd party systems.  And, while both traditional and digital manufacturing systems have had monitoring and reporting capabilities, they too are often closed-off, making it difficult to integrate this data w/ other aspects of the business and understand its impact as a whole.  

However, this is changing, particularly in the world of 3D printing.  The latest crop of machines from both new and existing players in the market are shipping with APIs to facilitate integration into existing company infrastructure.  These APIs are of two flavors: <strong>Monitoring/Reporting</strong>, and <strong>Command/Control</strong>.  

## Monitoring and Reporting APIs

<strong>Monitoring</strong> and <strong>reporting</strong> APIs are leveraged to pull data about current and previous build performance.  This enables companies to pull machine-specific data into their data warehouses and integrate it with known product data, such as cost, defect rates, and maintenance schedules.  The availability of this data turns the black boxes of the printers clear, and enables the company to evaluate different methodologies for maintenance schedules, material mix rates, and replacement schedules for wear parts. Understanding and optimizing the performance characteristics of these machines enables companies to get the most out of their investments, and make it easy for employees to understand the impact of their processes and opportunities for improvement.  

In addition to historical analysis, these APIs can also track and monitor the real-time status of machines.  These APIs enable software teams to build reporting into tools employees are already using (like Slack, Email, ERP systems), meaning that issues requiring human attention are raised directly to the people who can act on them.  This removes the overhead of having to constantly walk around and check on machines, or having to have a separate screen or window dedicated solely to monitoring equipment.  

## Command and Control APIs

The next step to connectivity beyond monitoring and reporting is <strong>command</strong> and <strong>control</strong>.  These APIs are designed to prepare files for printing, queue them up, and start/complete jobs.  Integrating machines in this fashion enables operators to <strong>initialize manufacturing from anywhere in the world</strong> leveraging a connected manufacturing execution system.  

This means that, for example, an employee in Singapore could kick off a 12hour print job being shipped to Seattle in Los Angeles in the morning, meaning that their counterpart in Los Angeles could come into the manufacturing facility 12 hours later and remove the completed product, and ship it quickly to Seattle.  This enables a company to have a 24x7 manufacturing operation without a 24x7 staff, optimizing the amount of time that the machines have to work without having to hire people to work off-shifts in their central location.  

Thinking of this a different way, this also permits a company with a global sales and manufacturing base to manufacture the customers product as close to them as possible to cut down on logistics costs and time.  This way, if someone in Mumbai wants to order a 3d printed part, a company HQ’d in New York but with a production facility in Shenzhen can initialize the job in China, cutting down on delivery time and cost.

## APIs and Manufacturing In Review

So, how are APIs powering the next Industrial Revolution?  They’re:
* <strong>Making it easy</strong> for companies to integrate their Manufacturing systems with their core business software
* <strong>Enabling modular, composable</strong> manufacturing solutions, permitting companies to use one machine in different ways for different processes
* <strong>Providing real-time monitoring and historical reporting</strong> to help people better understand and leverage their machines
* <strong>Unlocking a truly global supply chain</strong> through remote command and control, empowering companies to deliver for their customers like never before.

This is just the beginning of what increased connectivity plans to bring to digital manufacturing.  As more equipment manufacturers add connectivity to their products, we’ll be able to automate and distribute more of the process of product creation closer to the final customer.






