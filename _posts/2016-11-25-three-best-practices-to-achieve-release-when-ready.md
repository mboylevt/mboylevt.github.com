---
layout: post
title: "Three Best Practices to Achieve Release When Ready"
description: "How to stop worrying and love deployment"
thumbnail: /assets/img/how-shapeways-software-enables-3d-printing-at-scale/neutronium.png
category: software
tags: shapeways devops deployment
image:
  feature: 3-best-practices.png
---
Originally posted on the [Nordic APIs Blog](https://nordicapis.com/three-best-practices-to-achieve-release-when-ready/)
<br><br>
Watch a presentation @ 2016 Nordic APIs Platform Summit in Stockholm, Sweden based on this blog here - [Continuous Integration and Delivery at Shapeways](https://www.youtube.com/watch?v=qmsHazZcu9c)

<br>
<figure>
	<center>
  		<img src="/assets/img/three-best-practices/kraken.jpeg" />
  		<figcaption>A Confident Developer Ships Their Code</figcaption>
	</center>
</figure>
<br>

Let’s talk about <strong>releasing software</strong>. Take a minute, close your eyes, and conjure up the following in your head: you’ve been working on a new feature/killer app for a few weeks, and now it’s time to go live.
What are you feeling? Excitement, looking forward to expanding your offering? Relief, now that you’re done with a huge chunk of work? Pride, in you and your team’s work? Awesome! But are you nervous? Nervous that you’ve broken a key piece of functionality that will cause issues for your users? What about anxiety that you’re pushing something so huge that you’ve got no idea whether or not it’ll even work in the first place? Do you fear that you’ve spent weeks on a feature that no one will use?

Don’t despair — you’re not alone. Developers the world over feel these things too, and to a certain extent they’re healthy. Nervousness, anxiety, and fear keep us on our toes. They ensure diligence and rigor in specification, execution, and testing. That said, these feelings can also lead to bad behaviors that rob us of productivity, and our users of the features they need to be successful.

We’ve all been there. We’ve all created constructs to try to alleviate these feelings: scheduled weekly, bi-weekly, monthly, even quarterly releases. Big blocks of time set aside for testing to ensure that everything is working to spec. Overnight change windows so as not to impact users in case something goes wrong. These strategies aren’t themselves bad, but take a look at their goals. The goal of every one of the above strategies to condense the responsibility of releasing software into a tidy box, which we will deal with in the time we’ve set aside to do so. However this makes a pretty bold assumption: that releasing software is something to be feared. Pushing code is hard and causes problems, so we seek to minimize the frequency of doing so because that’s easier for us.

The purpose of this post is to challenge that assumption. The answer to this problem isn’t to minimize the number of releases, but to maximize them. <strong>Push more code</strong>. Push code as soon as it’s ready. Don’t wait for arbitrary release windows: that’s software that your community needs, that you have, that you’re not giving them. Don’t wring your hands in anxiety over a release that may have unexpected consequences: you’re never going to catch them all, and you’re definitely never going to fix the ones you’ve got if you don’t let people find them.

Instead, release software in <strong>small, scoped chunks</strong> so that you can manage your impact areas. Ensure that the code you’ve written is <strong>automatically tested</strong> to your standards, so that you don’t have to think about regressions. Releasing should be simple, so that when you do break something (and you will, so accept that too) you can easily release the fix. Finally, make sure that you know when you’ve broken something via monitoring. Humans staring at log files don’t scale, but automated alerting does.

Following these tactics will make release a non-event with minimal anxiety. Ready to get started? Good. Let’s talk about <strong>three best practices</strong> that’ll get you where you need to go.
<br>
## Step 1: Continuous Integration
<br>
<figure>
	<center>
  		<img src="/assets/img/three-best-practices/cycle.png" />
  		<figcaption>The Circle of Life.  Software Life.</figcaption>
	</center>
</figure>
<br>

<strong>Continuous Integration</strong>, or [CI](https://nordicapis.com/10-continuous-integration-tools-spur-api-development/), is the practice of continuously integrating committed code into
a shared repository. That shared repository is built, tested, and analyzed via whatever tools are deemed appropriate by the team. This is far from a new concept, but it’s the cornerstone of any successful release when ready process.

Step one is to select a CI tool. Fortunately, there are an abundance of tools to choose from. <strong>Self hosted solutions</strong> (you’ll need a server, and to maintain it yourself) include Jenkins and TeamCity. <strong>Web-based solutions</strong> (you’ll usually have to pay someone money for these) include Travis, SolanoCI, and CircleCI. Many developers like Jenkins — it’s completely FOSS, has a massive developer community for plugins and extensions, and just released a pretty sweet v2.0 that addressed a lot of user concerns.

Once you’ve got your tool you then need to figure out how to use it. The basic steps for CI are <strong>build</strong> (if you’re using a compiled language, or some sort of minification / obfuscation / optimization platform) and <strong>test</strong>. Test, in this case, is variable. The most basic tests you should run are your <strong>unit tests</strong>.

However, you don’t have to stop there. Got <strong>integration tests</strong> to ensure that your APIs are fulfilling their contracts? You could run those here if they’re fast enough. Got <strong>functional tests</strong> to ensure your user flows are working? Fire ’em up. How about <strong>performance</strong> or capacity tests to ensure that your changes are keeping with your response time and uptime numbers? Now might be a good time.

Of course, running all this testing takes time. And since we’re not talking about [Continuous Delivery](http://nordicapis.com/maintaining-api-security-in-a-continuous-delivery-environment/) here (pushing each and all commit to production as soon as it’s done), running this battery of tests on intermediate changelists is likely more overhead than it’s worth. That doesn’t mean you shouldn’t run these tests, but rather that you should be strategic about when you run them. More on that later.

Finally, consider using your CI platform to automate <strong>code quality tools</strong>. These include static analysis tools such as linters or complexity cycle detectors, test quality tools like code coverage reports, or just straight up information such as line of code counters. Information such as this rarely leads to direct performance improvements on your software, but they are invaluable for code readability, code maintainability, and development velocity.

## Step 2: Make Deployment Brainless
<br>
<figure>
	<center>
  		<img src="/assets/img/three-best-practices/easybutton.jpeg" />
  		<figcaption>The Circle of Life.  Software Life.</figcaption>
	</center>
</figure>
<br>

Now that you’ve got continuous integration up and running it’s time to actually take that code and push it live. When you first start this is simple: SSH to server, Git pull, maybe restart Apache…voila! New code live on website. Party time.

As your product continues to grow you’re required to do more things, like build mustache templates, generate [API documentation](http://nordicapis.com/using-templates-for-documentation-driven-api-design/), update [client libraries](http://nordicapis.com/what-languages-should-your-api-helper-libraries-support/), minify your JS/CSS, update crons, resolve dependency conflicts, and more. This quickly turns your one command <strong>deployment process</strong> which took minutes into an intimidating affair that can take hours if performed manually. Plus, it’s a lot for one person to remember which is why many companies put the responsibility of releasing software on a particular team like Quality or Ops.

This scenario has several major drawbacks. For one, manual deployment takes a long time. Secondly, entrusting a single team with this responsibility has dangerous consequences. If only a few people can deploy code you have to wait for one of them to be available to release. A slightly less obvious but equally damaging force at play in this system is that the developer is not ultimately responsible for being sure that their changes go live. The handoff in this process makes it unclear who’s responsible for helping address issues that may arise upon release. If the developer who wrote the code was releasing the code, then they’d be better able to determine what’s going wrong on release and how to fix it.

So now that we’ve painted the picture the solution becomes pretty clear: <strong>make deployment brainless</strong>. Make it fast too, but most of all make it so easy that anyone can do it. How? Like this:

### A: Write a deployment script.
This script should be able to take a build/artifact/tag/some identifier of what code to release as an input, and perform a full deploy of that code to your production servers. It should handle getting the build to the servers, installing it in the correct directories, updating the required client libraries, and restarting the necessary services to perform a full deploy of your software. Once you’ve got this script run you can fire it off from your continuous integration environment that we talked about above. Developers can then simply run this script from your continuous integration tool when they want to deploy, and it’ll handle it for them. Once you’ve got it written do your best to make it fast. Computers are good at repetitive tasks: make your script fast, and you’ll free up your people to focus on the things they’re good at, instead of watching a computer shovel bits around the Internet.

So, not a bad start. However, there’s still a lot to remember: Where does the script live? What arguments does it take? If there are several steps — and there usually are — in which order do I run them? Enter chatops.

### B: Use ChatOps.
[Chatops](http://nordicapis.com/12-frameworks-to-build-chatops-bots/) is a new term for an old concept: using a chat client to manage things like deployment, test runs, and general ops-related tasks. The term was coined by GitHub, who also released a wonderful little bot named [Hubot](https://hubot.github.com/), enabler of Chatops. Hubot is, at it’s core, a simple bot: it listens to your chats, and takes actions based on what you’re saying. The neat thing about Hubot is that it comes with a pile of integration scripts for tools such as Jenkins, which enable Hubot to run jobs triggered by you from your chat client.

You’re starting to see it, right? Instead of having to find your CI server, remember the name of the deployment job (and how to run it), you can simply have Hubot do that for you. It’ll kick that deployment job off for you and tell you when it’s done. But that’s not all: it’s fully open source and it’s very easy to write scripts to extend its functionality. This makes it possible to build things like queueing systems for pre-production test environments, scripts to refresh local databases with the latest changes from production, or tracking who’s going to lunch where — hey, you’ve gotta have fun too.

These two steps create a system that lets anyone deploy code to the production website. That’s powerful: it enables you to create a culture of autonomy WITH accountability: you can now push code at 2AM on a Sunday (but you’d better be there to fix it if something goes wrong). This drives <strong>engagement</strong> on your team, and scales as your team and organization get larger — two things that a traditional deployment process fails to achieve.

## Step 3: Monitoring
<br>
<figure>
	<center>
  		<img src="/assets/img/three-best-practices/vigilance.jpeg" />
  		<figcaption>Failure comes from the failure to imagine failure</figcaption>
	</center>
</figure>
<br>
Now we’re getting somewhere: we’ve got a CI environment which is testing and reporting on every build, and we’ve also got a deployment script and ChatOps running, handling our deploys for us. Nice! However, there’s one thing we haven’t touched on yet: how do you know when something’s gone wrong? <strong>Monitoring</strong>.

Monitoring your systems and setting alerts is required when deploying an app of significant size. Like the tasks mentioned above, computers are better at monitoring than humans: they don’t sleep, eat, or get bored. They’re really good at collecting and storing data too.

There are many great monitoring solutions out there: for infrastructure/app monitoring [Datadog](https://www.datadoghq.com/), [New Relic](https://newrelic.com/), and [Nagios](https://www.nagios.com/) are a few options. Each of these comes with a suite of built-in integrations to monitor common infrastructure and services: Apache, NGINX, HAProxy. Etc. There are also tools for monitoring and analyzing logs ([Logstash](https://www.elastic.co/products/logstash), [fluentd](http://www.fluentd.org/)), tracking user traffic and conversion ([Google Analytics](https://analytics.google.com/), [Mixpanel](https://mixpanel.com/)), and for building business intelligence ([Looker](https://looker.com/), [Tableau](http://www.tableau.com/)). We’ll focus this post using infrastructure monitoring as an example, but the same techniques apply to other monitoring systems.

So, you’re already ahead of the game: you’ve got new data about what’s going on under the hood. The next obvious question is: what do you do with it?

### Identifying KPIs for Infrastructure Monitoring

The first step for many people is to start setting up alerts on everything: HAProxy error rates, server CPU load, and IOWait times for storage. This isn’t the worst thing as it forces you to look at your entire infrastructure, but it’s directionless: without a specific idea of what you’re looking for you’ll have a hard time separating the signal from the noise.

Rather than diving right into handling alerts and tracking incidents, I’d instead recommend first identifying your KPIs for your infrastructure. For APIs in particular a few common KPIs are as follows:
* <strong>Request volume:</strong> How much traffic are we getting? How does that relate to our average? Is our usage growing?
* <strong>2xx/3xx/4xx/5xx rates:</strong> Are we providing our customers with a good experience? Are we seeing lots of 400/500s? If so, we should determine why and resolve.
* <strong>Server load:</strong> How are we scaling? How many more clients can we handle before we need to expand our hosting capacity?
* <strong>Error log rate:</strong> How is our error rate (syslog, application log, etc)? Is it elevated based on the norm? If so, did we just release something broken? Or are things not behaving as our users expect?

Once you select your KPIs you’ll need to allow time to identify your <strong>normal</strong> ranges. For example you might expect 400 concurrent connections at a given time, or a 99% 200/300 rate and a 1% 400/500 rate. Once you’ve identified these norms and an acceptable range around them, you can then set up <strong>alerting</strong> for when they are exceeded. This can be as simple as a threshold (we’re over 2% 400/500 rate = alert) to more complicated setups (we’re seeing a larger-than-normal request volume from IP address X over the past 10 mins — is this DDoS?). This will enable you to quickly identify and react to abnormal situations instead of having to wait for your customers to tell you that there’s a problem.

Once you’ve identified your KPIs, the next step is to <strong>display</strong> them as prominently as you can. Work in an office? Put a TV on the wall and display your real-time KPIs for all to see. Working from home? Keep a tab open and check it frequently. Use a chat app like Slack? Enable an integration to post your KPIs directly to channels of interest. You’ll find that the more you show these numbers around and speak to their importance the more you and your team will focus on improving them.

Armed with <strong>automated</strong> alerting a developer is able to diagnose, debug, fix, and release quickly. Since we’re automatically testing and releasing so frequently, our change footprint is small and easy to fix.

## Release When Ready is Really About Developer Ownership
<br>
<figure>
	<center>
  		<img src="/assets/img/three-best-practices/ownership.jpeg" />
  		<figcaption>Responsibility doesn't stop when the Epic closes in JIRA</figcaption>
	</center>
</figure>
<br>
All of this adds up to one of the most important drivers of developer engagement: <strong>Ownership</strong>. Now that developers are able to code, test, release, and monitor software on production without help from external teams, the world is their oyster. Want to push something to production on a Saturday night? Go for it! Need to release at 3AM because you’ve just closed a security hole? No one is stopping you.

It’s important to note, however, that decentralizing ownership heightens individual <strong>responsibility</strong>. Developers with the power to make changes must be present to fix them as well — meaning that if something broke during your 3AM deploy you must patch it up before logging out. Tempering the power of ownership with the onus of responsibility and the techniques discussed above will create a team that moves fast, releases confidently, and feels valued due to the investment made in their tools.

There you have it: three techniques your team can use to embrace the release when ready mindset. This is certainly not the only way to achieve release when ready, but they are proven techniques that have been used with success.