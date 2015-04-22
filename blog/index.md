---
title: This is my title
layout: post
---

Here is my page.

Pages:
{% for page in site.pages %}
   -  [{{ page.title }}]({{ page.url }})
{% endfor %}

Posts:
{% for post in site.posts %}
   -  [{{ post.title }}]({{ post.url }})
{% endfor %}

