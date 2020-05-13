---
layout: page
title: Matt Boyle
tagline: Thoughts on Software
---
{% include JB/setup %}

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
## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)
    
    author :
      name : Name Lastname {{ name }}
      email : blah@email.test
      github : username
      twitter : username

The theme should reference these variables whenever needed.
    
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

    $ rm -rf _posts/core-samples

Here's a sample "posts list".

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


