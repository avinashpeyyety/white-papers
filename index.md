---
layout: default
title: White Papers
permalink: /
---

<div class="landing">

# White Papers

Technical research and architecture guides by **Avinash Peyyety**.

<p class="site-credits">Writing assistance: SpaceXAI</p>

<h2>Agentic AI series</h2>
<p>A reading path for enterprise data and AI leaders, from the IDE assistant you already own to on-premises intelligence pods. Each paper expands one executive one-slider.</p>

<ol class="paper-list">
{% assign agentic = site.papers | where: "series", "agentic" | sort: "series_order" %}
{% for paper in agentic %}
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
</ol>

<h2>Other papers</h2>

<ul class="paper-list">
{% assign sorted_papers = site.papers | sort: 'date' | reverse %}
{% for paper in sorted_papers %}{% if paper.series != "agentic" %}
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
{% endif %}{% endfor %}
</ul>

</div>
