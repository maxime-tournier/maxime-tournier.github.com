---
title: Notes
layout: note
---

It are working nao

# Latest posts

{% for post in site.posts %}
   -  [{{ post.title }}]({{ post.url }})
{% endfor %}


# Pages
{% for page in site.pages %}
   -  [{{ page.title }}]({{ page.url }})
{% endfor %}


