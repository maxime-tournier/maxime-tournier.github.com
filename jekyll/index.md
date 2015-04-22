---
title: This is my title
layout: post
---

Here is my page.

List:

{% for post in site.posts %}
   -  [{{ post.title }}]({{ post.url }})
{% endfor %}

