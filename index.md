---
layout: default
title: White Papers
permalink: /
---

<div class="landing">

# White Papers

Technical research and architecture guides by **Avinash Peyyety**.

<p class="site-credits">Writing assistance: Composer 2.5, Moonshot Kimi 2.5, Grok Build</p>

<ul class="paper-list">
{% assign sorted_papers = site.papers | sort: 'date' | reverse %}
{% for paper in sorted_papers %}
  <li>
    <a href="{{ paper.url | relative_url }}">
      {{ paper.title }}
    </a>
    <span class="meta">
      {{ paper.date | date: "%B %Y" }} · {{ paper.author }}
      {% if paper.slug %} · <code class="paper-url-slug">{{ paper.slug }}</code>{% endif %}
    </span>
    {% if paper.excerpt %}
    <p class="excerpt">{{ paper.excerpt }}</p>
    {% endif %}
  </li>
{% endfor %}
</ul>

</div>
