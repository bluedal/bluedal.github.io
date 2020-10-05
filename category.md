---
layout: page
title: Category
---

{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date_to_string }}</span>
      </li>
    {% endfor %}
  </ul>
{% endfor %}