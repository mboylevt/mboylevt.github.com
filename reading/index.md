---
layout: page
title: Reading List
image:
  feature: nypl.jpg
  comments: false
modified: 2020-05-25
---

<div class="container">
  <div class="last-update">Last updated {{ site.data.reading.lastupdate }}</div>
    <br>
    This is my endeavor to more closely track the books that I read.  I'm also attempting to classify them to better understand my reading habits, so know that any error in taxonomy is on my part, not the part of the authors nor the booksellers.
    {% for entry in site.data.reading.list %}
    <div class="year-container">
      <div class="year">
        <h4>{{ entry.year }}</h4>
        <div class="number">{{ entry.books | size }} books</div>
      </div>
      <div class="books">
        <ul class="reading-list {{ entry.year }}">
          {% for book in entry.books %}
          <li>
            <a href="{{ book.link }}" target="_blank" alt="_blank" rel="nofollow noopener">{{
              book.title
            }}</a>
            <span class="author">by {{ book.author }}</span
            >{% if book.star %}<span class="star">★</span>{% endif %}
          </li>
          {% endfor %}
        </ul>
      </div>
    </div>
    {% endfor %}
</div>