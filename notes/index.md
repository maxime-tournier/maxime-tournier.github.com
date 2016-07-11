---
title: Research Notes
---

![yo euclid](euclid.png)


These are random research notes, generally providing quick reference
on recurring mathematical issues. Hopefully they can be helpful to
others :)

<ul>
{% for p in site.pages %}
{% if page.url != p.url %}
<li><a href="{{p.url}}">{{p.title}}</a></li>
{% endif %}
{% endfor %}
</ul>


