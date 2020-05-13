---
layout: page
title: Matt Boyle
---
{% include JB/setup %}

Hi there - I'm Matt, a technologist based in New York City.  I love to work on difficult problems where software and the physical world meet.  
I'm presently the VP of Architecture at [Shapeways](https://www.shapeways.com), where we're bringing 3D printing to everyone. 

When I'm not doing tech stuff, you can find me parenting our son, riding a bike, climbing a rock, brewing some beer, or playing Dungeons and Dragons.

You can find me here:
<div class="container">
  <div class="row">
    <div class="social-icons-image col-xs-3">
        <a href="http://www.github.com/mboylevt" >
            <img src="/assets/img/social/github.svg" alt="Github">
             mboylevt
        </a>
    </div>
    <div class="social-icons-image col-xs-3">
        <a href="https://www.linkedin.com/in/matt-boyle-b750263/" >
            <img src="/assets/img/social/linkedin.svg" alt="LinkedIn">
             Matt Boyle
        </a>
    </div>
    <div class="social-icons-image col-xs-3">
        <a href="http://www.twitter.com/mboylevt" >
            <img src="/assets/img/social/twitter.svg" alt="Twitter">
             @mboylevt
        </a>
    </div>
  </div>
</div>

## Latest Blog Posts
<p>
<div class="row col-xs-12">
  {% for post in site.posts limit:3 %}
  <div>
    <a href="{{ BASE_PATH }}{{ post.url }}"><h3>{{ post.title }}</h3></a>
  <p>{% if post.thumbnail %}
    <img src="{{ post.thumbnail }}" style="height: 280px" align="center" />
  {% else %}
    <img src="/images/nothumbnail.jpg" style="height: 280px" align="center" />
  {% endif %}</p>
  <p>&nbsp;</p>
  <p>
  {{ post.description | strip_html | truncatewords:40 }}
  </p>
  <p>
  <a class="btn" href="{{ BASE_PATH }}{{ post.url }}">Read more...</a>
  </p>
  </div>
  {% endfor %}
</div>
</p>
