---
layout: post
title: "Shapeways Skunkworks: The Coyote Framework"
description: "A functional testing framework written in Python"
thumbnail: /assets/img/how-shapeways-software-enables-3d-printing-at-scale/neutronium.png
category: software
tags: shapeways devops deployment python testing
image:
  feature: acme-header.png
  credit: The Acme Inventory Series - Rob Loukotka
---
Originally posted on the [Shapeways Tech blog](https://medium.com/shapeways-tech/shapeways-skunkworks-the-coyote-framework-85ef734b0ff7)

It’s a common enough idea: Where there’s software, there’s bugs. Whether you’re of a technical bent or not, you’ve almost certainly experienced a software bug in your life. From programs randomly crashing, to your favorite mobile game not recognizing your latest achievement, bugs have one commonality: they create a lousy (or, at the very least unexpected) user experience.

As developers, we strive to release high-quality software for our users. We also want to do it fast. But, building software fast can be a scary business: how can you know it’s right? How can you prevent regressions? The not-so-revolutionary answer to this is testing. Preferably fast, accurate, and repeatable testing. Generally, this is automated in the form of unit tests and integration/functional tests. The unit testing world is pretty well fleshed out, with xUnit style libraries existing for most major languages. The world of functional testing, particularly for the web, is a bit less feature-rich from a development standpoint.

## The Trouble with Functional Testing
<figure>
	<center>
  		<img src="/assets/img/coyote-framework/waiting.jpeg" />
  		<figcaption>Waiting for Functional Testing</figcaption>
	</center>
</figure>
<br>

Functional tests on the web have somewhat of a bad rap. They’re seen as being unreliable or flakey, and hard to debug and understand if you didn’t write them yourself. They’re slow, not taking advantage of setup data, and are hard to track failures without a human watching the screen . Finally, they put downward pressure on feature development, as any new feature or improvement will require that the tests are updated to reflect these changes.

The problems mentioned above are real. However, the reason they occur isn’t a fundamental flaw in functional testing and associated tools, but with how developers approach the way functional tests are written. For many, testing is a task to be completed, not software to be designed. Minimal effort is preferred, leading to copy/paste code snippets everywhere, creating duplication, which makes it quite the task when the time comes for maintenance. Running the tests can take a long time, as they’re often run against slow pre-production hardware, which leads to longer load times, and reduced interest in running them at all.

## The Goal

We decided to solve these problems by building a framework. The goal of the [Coyote framework](https://github.com/Shapeways/coyote_framework) is to remove the burden from functional testing. It makes it easy to write, read, debug, and update functional tests. It’s also designed to increase the speed of your tests via enabling fast creation of setup data, so that you can focus your tests on actually testing your site, not jumping through hoops to create setup data.

## Coyote?
<figure>
  <center>
      <img src="/assets/img/coyote-framework/dinnertime.gif" />
      <figcaption>Chasing Greatness</figcaption>
  </center>
</figure>
<br>
Our PHP framework which we use for Shapeways.com is called Roadrunner, as it’s designed to be fast. When the time came to choose a name for the framework that was emerging for testing at Shapeways, Coyote was a natural fit. We’ve got a few other hat tips to Warner Brothers in our pipeline as well, which I look forward to sharing later.

## Sounds Cool.  How does it work?
I’m glad you asked :) Coyote provides base classes for page objects, wraps and manages interactions with [Selenium Webdriver](http://www.seleniumhq.org/projects/webdriver/) and python's [requests](http://docs.python-requests.org/en/master/) library, and gives you strong logging and debugging features for your tests.

### Page Objects
Coyote is designed to follow the [Page Object](http://martinfowler.com/bliki/PageObject.html) pattern. The big win here is that each page (or reusable component, like the login modal) and its interactions are encapsulated in a single class. That means that, if you change the behavior or design of a component or page, you only have to reflect that change in your test code in one place. This removes a lot of the downward pressure on change.

The other win for page objects is that they allow you to abstract the page interaction code (find this element, enter that text, click that button) from the behavior, say “log in”. So instead of test code that looks like this:

```python
  username = webdriver.find_element('loginUsername')
  username.send_text('matt')
  password = webdriver.find_element('loginPassword')
  password.send_text('PinkPonyPinkPony')
  webdriver.find_element('loginButton').click()
```

You get this:

```python
  login_page.enter_username('matt')
  login_page.enter_password('PinkPonyPinkPony')
  login_page.click_login()
```

Coyote introduces a convention, site objects, which are used to group several interactions (even across several pages) at once. For example, the above page object interactions would be rolled into a single function in a site object with a prototype like this:

```python
  def login(username, password)
```

The true benefit of this pattern is realized when your tests go from being a mess of calls to webdriver (or your interaction tool of choice) to being behavioral descriptions. As an example, if you wanted to write a test that logged in and added an item to your cart, the test code would simply be

```python
  login('matt', 'PinkPonyPinkPony')
  visit_product_page('SomeProductId')
  add_to_cart()
```

This makes it clear what the test is doing up front, and where it’s falling over when it fails.

### Web Interactions
Coyote also contains wrappers around Webdriver and Requests, for browser-based and raw HTTP communications.

WebdriverWrapper handles common exceptions that Webdriver does not, as well as provides convenience methods for dealing with asynchronous requests. By doing this, it prevents a large number of webdriver-related test flappyness, specifically around stale DOM exceptions. It also provides excellent debugging output, including taking a screenshot w/ the stack trace overlaid on top of it for later review and headless debugging.

Additionally, WebdriverWrapper provides convenience wrappers around webdriver’s “find” constructs, as well as Locators, first-class objects representing HTML elements to be searched. These are intended to be contained within a Page Object,in a locators class. For example, here are the locators for the Page Object representing our [Developer Portal[(http://developers.shapeways.com)

```python
  class locators(object):
   get_started_button = Locator('css', '.action-button.solo', 'main button to get started')
```

With locators stored like this, your calls to find elements go from this

```python
  def is_page_loaded(self):
   '''Tests if page is loaded'''
   return self.webdriver.find_element(By.css, '.action-button.solo')
```

to this

```python
  def is_page_loaded(self):
   '''Tests if page is loaded'''
   return self.dw.is_present(self.locators.get_started_button)
 ```

 This may not seem like much, but it becomes indispensable when you have to change a locator that is used multiple times per page. Locators make this type of refactoring a breeze.

RequestDriver is a wrapper around python requests. In addition to the standard HTTP stuff that requests does, RequestDriver provides convenience methods around session management and cookie manipulation. The purpose of this module is twofold: it lets you create setup data for tests really quickly, and it allows for performing integration-style tests where you need ’em. The big one here is setup data time: instead of having to rely on pre-created data (or on webdriver to generate it), you can simply hit the relevant http endpoints. This both enables you to get setup data created fast, and ensures that it’s created in the same way as it would be by the web interactions, since you’re using the same endpoints. Solid.

Here’s an example RequestDriver call. You’ll notice it looks almost exactly like a python requests call….because it basically is :)

```python
  response = self.rd.request(
        uri=ShapewaysUrlBuilder.build_login_page_url(
             host=site.path_www, 
             scheme=site.https_protocol
        ),
        method=self.rd.POST,
        data={
             'username': username,
             'password': password,
             'targetUrl': target_url
        }
   )
```

You can see here as well that we’re using a URL builder to generate our links: we’ve included the base class for this as well for your enjoyment.

### Everything Else
There are several other constructs included in the Coyote Framework, including DB support (base classes for entities, etc), logging, config tools, and various utility functions. If you have specific questions on how these work, please either reference the examples (coming soon!), or leave us a comment on Github. We’ll be happy to explain.

Happy testing!

In case you missed it above, here’s a link to the [repo](https://github.com/Shapeways/coyote_framework) on Github.

## Acknowledgements
This little bundle of joy wouldn’t have been possible with out the hard work of current and former Shapeways employees, specifically [Hans Wang](https://github.com/hansw47) and Zheng Qin. Thanks guys!