---
layout: page
title: Experience
lede: Where I've worked and studied.
permalink: /experience/
---

{%- if site.resume %}
<p><a class="button" href="{{ site.resume | relative_url }}">Download my resume (PDF)</a></p>
{%- endif %}

<h2>Work</h2>
<ol class="timeline">
{%- for job in site.data.experience.jobs %}
  <li>
    <h3>{{ job.company }}{% if job.role %} <span class="role">· {{ job.role }}</span>{% endif %}</h3>
    {%- if job.period or job.location %}<p class="meta">{{ job.period }}{% if job.period and job.location %} · {% endif %}{{ job.location }}</p>{% endif %}
    {%- if job.summary %}<p>{{ job.summary }}</p>{% endif %}
  </li>
{%- endfor %}
</ol>

<h2>Education</h2>
<ol class="timeline">
{%- for ed in site.data.experience.education %}
  <li>
    <h3>{{ ed.school }}</h3>
    {%- if ed.period %}<p class="meta">{{ ed.period }}</p>{% endif %}
  </li>
{%- endfor %}
</ol>

<h2>Certifications</h2>
<ul class="plain">
{%- for c in site.data.experience.certifications %}
  <li><strong>{{ c.name }}</strong><br><span class="meta">{{ c.issuer }} · {{ c.date }}</span></li>
{%- endfor %}
</ul>
