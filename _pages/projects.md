---
layout: page
title: Projects
permalink: /projects/
description: Engineering work — open-source contributions and tools for AI agents.
nav: true
nav_order: 3
display_categories: [open source, agent tooling, web apps, llm apps, machine learning]
---

<!-- pages/projects.md: an editorial numbered list instead of al-folio's cards (styles in assets/css/main.scss) -->

<ol class="kp-projects">
{%- assign n = 0 -%}
{%- for category in page.display_categories -%}
{%- assign categorized_projects = site.projects | where: "category", category | sort: "importance" -%}
{%- for project in categorized_projects -%}
{%- assign n = n | plus: 1 %}
<li><a href="{% if project.redirect %}{{ project.redirect }}{% else %}{{ project.url | relative_url }}{% endif %}">
<span class="kp-num">{% if n < 10 %}0{% endif %}{{ n }}</span>
<div><h3>{{ project.title }}</h3><p>{{ project.description }}</p><span class="kp-label">{{ project.category }}</span></div>
<span class="kp-arrow" aria-hidden="true">&rarr;</span>
</a></li>
{%- endfor -%}
{%- endfor %}
</ol>
