---
title: Notes
layout: note
---

# Notes

![I'M A CHIKIN LOL](chikin.jpg)

These are random research notes, generally providing quick reference
on recurring mathematical issues. Hopefully they can be helpful to
others :)

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



