---
title: This is my title
layout: post
---

Here is my page.

# Latest posts

{% for post in site.posts %}
   -  [{{ post.title }}]({{ post.url }})
{% endfor %}


# Pages
{% for page in site.pages %}
   -  [{{ page.title }}]({{ page.url }})
{% endfor %}


