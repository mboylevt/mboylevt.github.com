---
layout: post
title: "Push All the Code: Deployment Tools at Shapeways"
description: "Ship it."
category: software
tags: shapeways, devops, deployment, continuous integration
image:
  feature: ship-it.png
---
Originally posted on the [Shapeways Tech blog](https://medium.com/shapeways-tech/push-all-the-code-deployment-tools-at-shapeways-b925164ef221)
<br>
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/automation.gif" />
      <figcaption>From “Machine Civilization” by World Order</figcaption>
  </center>
</figure>
<br>
Releasing software developed as a team can be hard. Complex interactions, changes to databases, multiple developers changing the same file…all of these behaviors are both required for developing software, and possible sources of bugs, regressions, performance degradation, and other software maladies. This has lead to teams fearing releasing, which leads to teams releasing less frequently (I’m scared of that: why would I do it often?), which leads to more changes per release, which leads to more complexity, which leads to bugs, which leads to…you guessed it, fearing the release.

It doesn’t have to be this way. Breaking the cycle is possible. With the right tools, test coverage, and development best practices, you can migrate from fearing your weekly/monthly/quarterly release to releasing code when it’s ready, multiple times per day. We’ve done it at Shapeways, and we’re certainly not the first, and definitely not the last (hopefully, you’re next!) Here’s how we did it.

## Enabling Release when Ready
There are three behaviors which have enabled us to move to releasing multiple times per day:
1. Investing in Internal Tooling
1. Following Development Best Practices
1. Writing and Running Tests

The remainder of this post will focus on our internal tooling. Our development best practices and writing/running tests behaviors will be covered in later posts. Onward!

## Using Jenkins to Achieve CI

We realized right away that we needed to follow Continuous Integration best practices. We set up a [Jenkins](https://jenkins-ci.org/) server in the office, and began using it for building and testing our deliverables every time we wanted to deploy to a testing environment. This did two things: it ensured that our unit tests were being run on every meaningful push, and it removed the need for developers to have intimate knowledge of the deployment process. Just click a button, and it’s there.

Ensuring that unit tests are run on all builds is one of the fundamental principles of CI: by doing so, you both protect yourself against regressions and remove the need for each and every developer to remember to run the tests themselves. Which, as anyone who’s ever written software as part of a team before knows, is a Good Thing. One less thing for developers to think about == one more step which will not be missed when the pressure is on. Plus, when done right, it can create a culture of quality, where developers enjoy writing tests because they see the benefit in their daily lives.

However, let’s not understate the importance of giving developers a button to deploy software. The benefit is twofold: all developers now have the knowledge and power to deploy (feels good, man), and your tools/ops/quality team can now <strong>change the deployment process</strong> without breaking everyones knowledge of how it works! You’ve provided your dev team with an interface that they can use to deploy, no matter what or how the deployment process has changed under the hood. Awesome. Because you’re gonna need to change your deployment process at some point as you scale.

So, that’s pretty cool, right? Developers can now simply click a button to deploy any branch of code to any environment you’ve got, be it dev, qa, or even production. Definitely cool. Nothing could be cooler…except, why do I have to go to some random webpage to press a button? Can’t you make it easier than that?

## Enter Slack+Hubot

For those of you who don’t know, Slack is a chat client. It’s way more than that, but for now, let’s just focus on the chat client aspect, and accept that we use it at Shapeways for communications across the company. One of the coolest features of Slack is its API: you can build integrations between whatever tools you’ve got (so long as they have APIs, and if they don’t, why are you using them?) and Slack. You can probably see where this is going.

But before we get there, let’s talk a little bit about the concept of ChatOps. A term coined by the good folks over at [GitHub](https://github.com/), ChatOps, by definiton is:
> An approach to communication that allows teams to collaborate and manage many aspects of their infrastructure, code, and data from the comfort and safety of a chat room. - https://victorops.com/blog/chatops-for-dummies

GitHub is by no means the first company to play with this concept, but we’re gonna focus on them for the moment because they’re the originator of the next Really Cool Thing we’re gonna talk about: [Hubot](https://hubot.github.com/).

Hubot is, at its core, a chat bot. It can do clever things, like tell you when the 6 train is running behind (hint: all the f$&%ing time), and convert liters to gallons. However, its real value is that it can integrate with many tools and facilitate communication between them. This is where things get even better for your deployment pipeline.

Take the above example: you’re deploying using Jenkins. Super. Hubot ships right out of the box w/ a Jenkins integration: just plug in your credentials, and Hubot can now directly execute Jenkins jobs. Check it out, and note that names and urls have been smudged to protect the innocent :wink:
<br>
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/hubot-deploy.png" />
      <figcaption>A Developer Deploys to Beta</figcaption>
  </center>
</figure>

So, there you have it. By simply creating the right Jenkins jobs and adding hubot to your chat app, you can manage all of your deployment requirements directly from your company chat application. Developers can now deploy to development, staging, and even production with a quick message. That’s even easier than clicking a button.

## But wait, there’s more
While the built-in integrations are nice (and contain most of the base functionality that you need to get your ChatOps platform going), you’re probably going to find something that you want which Hubot simply can’t do. No worries: Hubot ships with its own scripting platform, enabling you to add your own functionality. At Shapeways, we’ve used this for both good and entertainment.

A few examples:

Reserving environments for deployment (with faces!), and preventing you from deploying an environment you don’t own. This saves us from asking the “wait, who’s got QA1 again?” question.
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/hubot-list-envs.png" />
      <figcaption>Looking for a shared environment</figcaption>
  </center>
</figure>
Queueing System for environments. This way, you don’t have to call dibs. Just get in line, and Hubot will automatically reserve the environment for you once it becomes available.
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/hubot-reserve-envs.png" />
      <figcaption>Reserving a shared environment</figcaption>
  </center>
</figure>
Executing our suite of automated functional tests against any of our environments using Selenium Grid. Custom reporting provided by our internally developed framework, [Coyote](/software/shapeways-skunkworks-the-coyote-framwork/).
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/hubot-test.png" />
      <figcaption>Running functional tests against a QA environment</figcaption>
  </center>
</figure>
Permission management using HubotAuth: we don’t want just ANYONE to be able to deploy our site.
<figure>
  <center>
      <img src="/assets/img/push-all-the-code/hubot-who-has.png" />
      <figcaption>You'd better be on this list if you want to push code</figcaption>
  </center>
</figure>
Pretty cool, right? We’re looking to build in even further integrations into Hubot, and will contribute back those which don’t have specific Shapeways IP into the OSS community.

That’s a brief overview of some of the internal tooling that we’ve employed here at Shapeways to get us to Release when Ready. Stay tuned for coming posts on how we changed our development and testing practices to get us releasing multiple times per day.