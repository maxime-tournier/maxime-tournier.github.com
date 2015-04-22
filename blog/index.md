---
title: Notes
layout: note
---

# Notes

These are random research notes, generally providing some quick
reference on recurring mathematical issues. I'm just leaving this
here :)

## TODO
   - tags ?

## Latest Posts

{% for post in site.posts %}
   -  [{{ post.title }}]({{ post.url }})
{% endfor %}

## Pages

{% for page in site.pages %}
   -  [{{ page.title }}]({{ page.url }})
{% endfor %}



