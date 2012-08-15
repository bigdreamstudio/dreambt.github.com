---
layout: page
title: Welcome to SiQi Blog!
tagline: Open source, Cloud, Java, Linux and so on
---
{% include JB/setup %}

## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)

The theme should reference these variables whenever needed.

Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

## Sample Posts

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>{{ page.content | truncatehtml: 500 }}</li>
  {% endfor %}
</ul>

