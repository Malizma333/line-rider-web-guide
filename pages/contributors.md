---
layout: page
title: Contributors
permalink: /contributors/
nav_order: 6
published: false
---

# Contributors

These are all of the people who contributed to this guide.

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li style="display: flex; align-items: center; justify-content: left; margin-bottom: 5px;">
    <a href="{{ contributor.html_url }}">
      <div style="border: 2px solid black; border-radius: 50%; width: 32px; height: 32px; position: relative; overflow: hidden;">
        <img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}">
      </div>
    </a>
    <span style="margin-left: 5px;">
      {{ contributor.login }}
    </span>
  </li>
{% endfor %}
</ul>