---
layout: page
title: Matt Boyle
tagline: Thoughts on Software
---
{% include JB/setup %}

Hi there - I'm Matt, a technologist based in New York City.  I love to work on difficult problems where software and the physical world meet.  
I'm presently the VP of Architecture at [Shapeways](https://www.shapeways.com), where we're bringing 3D printing to everyone.

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
