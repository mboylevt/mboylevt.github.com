---
layout: post
title: "Seeking Scale"
description: "Starting My New Job at Datadog"
category: software
tags: career software
image:
  feature: datadog.jpg
---
<div style="font-size: 6;"><i>Reflections on my first 60 days at Datadog.</i></div>

I got a new job!  Well, I’ve had it for some time now, but I figured rather than bang on about it on Week 1 I’d hold off for a bit and learn a least a little bit of the lay of the land.  After a long, restful break after my [last day at Shapeways](https://mboyle.org/software/the-parting-glass/) (approximately 48 hours), I started my first day as an Engineering Manager at [Datadog](https://datadoghq.com).  I’m now leading two fast-growing teams which are responsible for extracting customer data from cloud providers and funneling it into our intakes. 

<i>Shameless plug alert: these teams are growing, and fast, so if you’re looking for something new and want to work on critical teams at a large public company...hit me up.</i>

## Why Datadog?

“Datadog, wow, that’s a pretty big change from Shapeways!”, you might be thinking to yourself...and you’d be right.  After 8.5 years working as both a senior IC and VP of Engineering in a startup environment, I wanted to take the next step in my career.  Step 1, of course, was figuring out what that actually meant.  Long term, I’d love to be a CTO of a growing company - startup, public, or otherwise.  While I enjoyed my stint as a senior IC, cranking out solutions to difficult problems, I knew that I wanted to get back into leadership and management in my next gig - I missed setting strategy, and working with my colleagues to make that strategy into a reality.  I figured I had two options - I could go be CTO or VP Engineering for an early- to mid-stage startup, or I could take a title demotion and go work at a larger company and learn about scale.  The former, I’d already done, and felt a bit repetitive.  The latter, however...while I’d worked at big companies in the past, I’d never been in a leadership position before.  I realized that this gap would eventually become career limiting - I’d never be hired as a CTO of a mid-sized company if I kept my experience to managing teams of 30 or less. 

Decision made, I started seeking out companies who I respected and fit the profile. And, while I spoke with lots of different places (and was ignored by just as many, if not more), I always had a good feeling about Datadog.  See I’ve been a DD customer since 2013, and have loved watching it grow and evolve.  Hell, read old posts on this blog - you’ll see they’re full of Datadog charts, graphs, and alerts. I had a deep respect for their product, knew that they were growing, and liked that they were already public. Datadog fit my requirements to a T - and fortunately, they felt the same way about me :)

## Operating at Scale

As mentioned above, I knew that I had some gaps that I wanted to fill in my experience. The north star I was pursuing in a larger company was “learn how they do X at scale”.  At first glance, I thought of engineering problems - building distributed systems at scale, managing uptime, reliability, multi-cloud presence and deployment.  These were all challenges that I had little experience with at the scale Datadog operates, and would definitely be an excellent learning experience.  However, what really got me excited about the role was to learn more about how a company ran at that scale, on the human side.  How are decisions made in an organization with over 800 engineers?  How is a vision set, how do you build a strategy to drive towards that vision, and how do you devise tactics to make progress against that strategy?  How do we collaborate, communicate, share knowledge, in a team so vast?  The more I thought about it, the more I realized that *this* was the real gap that I had to fill.

## Initial Thoughts and Comparisons

So, I’ve been at Datadog for around 60 days now, which obviously means that I know all there is to know...or not. Here are my initial thoughts on some differences and similarities between my previous gig and my new one, with a GIANT CAVEAT that these are 100% subject to change, and that any mis-representation or error is my own.  I look forward to looking back on these in a year and laughing, a lot, about the mistakes and assumptions that I’m about to type up below. 

### Differences

There are, as one would imagine, significant differences between Shapeways and Datadog.  I’m breaking these down into two main categories: Business, and Scale.  

From a business perspective, Shapeways is a venture-backed B2C digital manufacturing and technology platform with heavy B2B tendencies.  Datadog, on the other hand, is a publicly-traded enterprise SaaS company.  The size, scale, and scope of these two businesses leads to some predictable difference.  Shapeways is a startup, lacking a huge bankroll, and has to be scrappy and fast with its business and technology decisions, prioritizing time-to-market and first mover advantage to land customers to drive future innovation.  Datadog has a much larger balance sheet and an entrenched, high-tough customer base, leading to more considered decisions around product and feature launches, taking care not to regress on previous promises to customers.  Additionally, Shapeways is a manufacturing company - it must consider physical magins, COGS, and physical supply chains to deliver products to customers, leading to a complex calculation of EBITDA.  Datadog, being a SaaS company, has a much smaller number of factors to consider prior to EBITDA, none of which are physical in nature. This leads to faster growth, faster iteration time vs physical products, and sheds some light on Shapeways’ decision to build a manufacturing technology platform to supplement its 3D Printing Service :)

Then there’s the scale - Datadog is simply a much larger company, with larger technology requirements and more people at its disposal than Shapeways. Note that the following stated figures for both companies will be approximate, and please don’t get too bogged down in numbers - they’re more for proving order-of-magnitude difference than they are a tied-out representation of either.  

#### Technology

Let’s start with technology requirements.  Shapeways handles an admirable amount of data every day - thousands of uploads, thousands of orders, tens of thousands of physical parts produced, finished, and shipped globally.  At its peak, Shapeways sees a thousand or so concurrent visitors to the website, with hundreds more accessing backend manufacturing and supply chain management systems.  Compare this to Datadog, which regularly processes tens of millions of data points per second, in a multi-cloud environment...and that’s just the metric intake.  This ignores the breadth of products, including APM, Profiling, Logs….Datadog intakes, tags, processes, and presents a truly planet-scale amount of data.

This difference in scale demands different requirements of each company.  In the case of Shapeways, the requirements aren’t nearly as large as Datadog.  That said, availability and performance remain critical to the business, so redundancy and self-healing systems are critical to success.  To meet these requirements, Shapeways maintains two datacenters (one US, one EU), each of which are capable of running the entire suite of tools themselves, and ergo function largely as disaster recovery / hot standby DCs.  It takes about 10mins to flip the entirety of Shapeways' web presence between them, which is _quite the feat_ when you consider that the team responsible is made up of two people. Each of these DCs has a few hundred hosts, split between bare metal and virtualization, which handle the databases, apps, storage, and more required to run the business.  Shapeways also has the ability to scale compute into the cloud, and does so on a regular basis for the more fungible services.  

That system is now becoming too small for Shapeways’ growing business, and will need to be revisited by the team running those systems.  That’s a problem they’re well-equipped to solve, and one I believe will result in them migrating their services fully to the cloud.

Shapeways leverages a fairly standard set of services and applications to make the company go.  These include, but aren’t limited to
 * MySQL, MongoDB, Solr for databases
 * Redis, Memcached, APC for caching
 * ActiveMQ, Celery for queueing
 * Luigui, Airflow, Redshift, Looker for BI
 * Puppet, Terraform for configuration management
 * zfs, ubuntu, nginx, cumulus, quagga, strongswan for storage, networking, security, etc

 You could consider these to be “Boring” technology choices, and you’d have a strong argument.  You know what though?  I _like_ Boring technology running my mission-critical stuff.  Boring, in the software world, is often coded language for “stable”, “well-understood”, and “has a large community using it.”  Those are characteristics i _want_ when I’m building on a tight budget and a high uptime requirement.  You can hire people who already know Boring.  You can build Boring fast, because it’s well documented, and easy to get help when you need it.  Compare that to more interesting technologies, which are often developed by large companies to meet a specific scale challenge they’re experiencing. And, as much as we’d all like to be Google, it’s unlikely that your seed-round startup’s scaling problems require the same solutions as Google.  

Datadog, on the other hand, is a cloud-first company.  As a SaaS platform, Datadog’s need for compute scales directly with its customer base.  In the Shapeways case, it takes the same amount of compute to place an order for 100 of a single model as it does for 1.  Datadog, on the other hand, requires 100x compute to process 100 data points when compared to processing just 1.  Datadog has to go where its customers are - be they in AWS, GCP, Azure, Oracle, or using their own datacenters.  This means that Datadog must support installations in multiple clouds, in multiple regions, or face prohibitive costs extracting that data and loading it into another cloud provider.  

At the scale of the company, Datadog needs to focus not just on the uptime and reliability of their services, but also the performance.  When a company is  processing that much data, performance hits or timeouts can cause cascading effects that seriously degrade or stop service to their customers.  As this is Datadog’s entire business, there’s a huge amount of effort spent around both optimizing for performance, and building resilience into our systems to limit the blast radius of service degradation or failure. In order to meet the requirements, Datadog uses a broad set of technologies (too many to list here), but unlike Shapeways,  leverages them for specific purposes that are suited to their performance characteristics.  For example, if you need a database @ Datadog, you use PostGres.  However, if what you need is strong write performance and bulk read access, you would use Cassandra.  Need to drink from a firehose of data, and partition it automatically for consumption?  Kafka would be your tool of choice. The point is, Datadog has a different set of needs than a company like Shapeays, and those needs are reflected in the considered technology choices they make.

#### People

The technological differences are stark between the two companies, but that’s not why I joined.  I wanted to see how things were different in the organization of people. 

The first, and most obvious, thing to note is that Datadog is around 10x the size of Shapeways from a pure employee count.  However, from an Engineering perspective, it’s more like 40x the size.  A large portion of Shapeways’ headcount are folks working in our factories - the engineering team makes up around 10% of employees. Compare that to Datadog, where the engineering team is around 50% of total headcount.  Things are naturally different at that scale - here’s a few things I’ve observed so far.

 * There are many more layers of management at Datadog.  At Shapeways, it was pretty simple - 1 VP, 3 Directors, ~20 engineers.  At Datadog, it’s 1 CTO, 2 SVPs, ~5 VPs, 10s of Directors, maybe 10 Engineering Managers (a new role, more coming soon),  dozens of Team Leads, and hundreds of Engineers.  While this might sound like a mess of overhead, it’s actually quite efficient - vision and high-level strategy is set at the VP/Director level, OKRs, roadmaps, and goals are set at the EM/TL level, and the engineers perform the execution.  

 * All strategy and OKR documents are public, so people have unfettered access to information.  Everyone has a good idea of what they’re working on, why they’re working on it, and how they’re doing against their goal.  The engineering org is built on a ton of trust - individual devs have permissions to deploy to production pretty much anything that they deem ready.  This removes a lot of unnecessary barriers to context, provides a level of accountability, and enables the individual engineers to make and act upon decisions. 


 * While the company’s tech stack is huge, most people at Datadog work on a small, constrained piece of it. This enables them to learn deeply the characteristics of the software that they own, and to feel confident deploying changes to production.  Which is good, because…


 * Developers are responsible for the code they write.  At Datadog, if you write it - you run it.  There are no dedicated ops teams.  This is made possible largely because the developers are so focused on smaller product surface areas than at a startup like Shapeways.  At Shapeways,  day you may be doing frontend and the next you’re optimizing SQL.  At Datadog, you’re working on your team’s product, with your dedicated team.  

### Similarities

Those are some pretty big differences between two dramatically different companies...but they’re all expected.  What’s a bit more surprising is the collection of similarities that I’ve noticed between the two.  

First off - despite a dramatic staffing size difference, both Shapeways and Datadog employ a similar “small teams” philosophy.  Smaller teams (10 person max) communicate more clearly, know what each other are working on (because it’s probably the same project), and build meaningful bonds with one another.  Compare this to larger teams, where it’s more difficult to communicate, difficult to find in-kind work for 15-20 people to do, and therefore harder to build these bonds.  This is roughly equivalent to Bezos’ “two-pizza rule”, though I hesitate to call it so because I reject his overall belief that “communication is bad”.  Communication isn’t bad - it just becomes overhead as teams grow too large.

Another commonality between the two companies is that the engineers have a general enthusiasm for the product that they’re building.  Shapeways is full of 3D printing and manufacturing fiends - they experiment with the product, and use their firsthand knowledge and insights to bring improvements to the work that they do.  Same goes for Datadog - many people who work here have used Datadog in their previous jobs, and love the immediate value it brings to their work.  Unsurprisingly, we use Datadog’s product suite to monitor our own production software, and I’m not sure how easy it’d be to take those on-call shifts without it. 

A third similarity - despite being a publicly traded company and a NYC unicorn with extremely competitive compensation, it’s still hard to hire at Datadog!  Datadog has grown 2x a year for basically the past 4 years.  It’s difficult to find that many qualified candidates to sustain that growth, and there are tons of teams inside of Datadog who are vying for the same talent pool.  Also, Datadog is now a public company - there’s less potential equity to hit a home run with as opposed to a startup.  Now, these hiring challenges are definitely different than those at Shapeways, it’s worth mentioning that hiring didn’t get any easier w/ moving to a larger company with more resources. 

Finally, on a bit of a somber note, I’m still working out of my bedroom.  The COVID-19 pandemic is now entering it’s 9th month, and while vaccines are starting to be considered by the FDA, we’ve still got a long way to go here in NYC.  I’ve completed an entire job hunt, did a 5 week transition period, started a new job 60 days ago...and I’m still in the same chair, in the same bedroom, in the same small apartment as I was in 9 months ago.  And, guess what - I’m lucky.  My family works from home, and has remained healthy.  I have job security, food security, and housing security that’s not guaranteed to so many others.  So, if you’ve got those things, take a moment and be grateful for them - even though this is one of the hardest years of my life, I only have to look out my window to see how lucky i have it.  If this sounds like you, I encourage you to do the same, and think about what you can do for your community this holiday season.

