---
title: Research Notes
---

![yo euclid](euclid.png)


These are random research notes, providing quick reference on recurring
mathematical issues. Please let me know if you find anything useful in there :)

{% assign categories = site.pages | map: "categories" | uniq %}
{% for category in categories %}
<h2>{{category}}</h2>
<ul>
    {% for p in site.pages %}
        {% if p.categories contains category %}
        <li> <a href="{{p.url}}">{{p.title}}</a> </li>
        {% endif %}
    {% endfor %}
    </ul>
{% endfor %}


<h2>misc</h2>
<ul>
    {% for p in site.pages %}
        {% if p.categories %}
        
        {% else %}
        <li> <a href="{{p.url}}">{{p.title}}</a> </li>
        {% endif %}
    {% endfor %}
</ul>






