---
layout: page
title: Experience
lede: Where I've worked and studied.
permalink: /experience/
---

{{ site.data.experience.summary }}

{%- if site.resume %}
<p><a class="button" href="{{ site.resume | relative_url }}" download>Download my resume (PDF)</a></p>
{%- endif %}

<h2>Work</h2>
<ol class="timeline">
{%- for job in site.data.experience.jobs %}
  <li>
    <h3>{{ job.company }}{% if job.role %} <span class="role">· {{ job.role }}</span>{% endif %}</h3>
    {%- if job.period or job.location %}<p class="meta">{{ job.period }}{% if job.period and job.location %} · {% endif %}{{ job.location }}</p>{% endif %}
    {%- if job.points %}
    <ul>{% for p in job.points %}<li>{{ p }}</li>{% endfor %}</ul>
    {%- endif %}
    {%- if job.achievements %}
    <p class="sub">Key achievements</p>
    <ul>{% for a in job.achievements %}<li>{{ a }}</li>{% endfor %}</ul>
    {%- endif %}
  </li>
{%- endfor %}
</ol>

<h2>Education</h2>
<ul class="plain">
{%- for ed in site.data.experience.education %}
  <li><strong>{{ ed.school }}</strong><br><span class="meta">{{ ed.degree }} · {{ ed.period }}</span></li>
{%- endfor %}
</ul>

<h2>Skills</h2>
<dl class="skills">
{%- for g in site.data.experience.skills %}
  <dt>{{ g.group }}</dt>
  <dd class="chips">{% for i in g.items %}<span>{{ i }}</span>{% endfor %}</dd>
{%- endfor %}
</dl>

<h2>Open source</h2>
<ul class="plain">
{%- for o in site.data.experience.open_source %}
  <li><strong>{% if o.url %}<a href="{{ o.url }}">{{ o.name }}</a>{% else %}{{ o.name }}{% endif %}</strong><br><span class="meta">{{ o.summary }}</span></li>
{%- endfor %}
</ul>

<h2>Certifications</h2>
<ul class="plain">
{%- for c in site.data.experience.certifications %}
  <li><strong>{{ c.name }}</strong><br><span class="meta">{{ c.issuer }} · {{ c.date }}</span></li>
{%- endfor %}
</ul>

<h2>Societies</h2>
<ul class="plain">
{%- for s in site.data.experience.societies %}
  <li>{{ s.name }}<br><span class="meta">{{ s.period }}</span></li>
{%- endfor %}
</ul>
