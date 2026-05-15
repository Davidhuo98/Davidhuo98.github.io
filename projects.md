---
layout: default
title: 项目
permalink: /projects/
---

<section class="page-header">
  <h1>项目</h1>
  <p>这里收录了我做过的项目，每个项目都附有开发文章和 GitHub 源码。</p>
</section>

<div class="projects-grid">
  {% assign sorted = site.projects | sort: "date" | reverse %}
  {% for project in sorted %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>
