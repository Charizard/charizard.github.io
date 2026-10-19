---
layout: page
title: Experience
lede: Where I've worked and studied.
permalink: /experience/
---

<ol class="timeline">
{%- for job in site.data.experience %}
  <li>
    <h3>{{ job.company }}{% if job.role != "" %} <span class="role">· {{ job.role }}</span>{% endif %}</h3>
    {%- if job.period != "" %}<p class="meta">{{ job.period }}</p>{% endif %}
    {%- if job.summary != "" %}<p>{{ job.summary }}</p>{% endif %}
  </li>
{%- endfor %}
</ol>
