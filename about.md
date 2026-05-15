---
layout: default
title: 关于
permalink: /about/
---
<section class="page-header">
  <h1>关于我</h1>
</section>

<div class="about-bio">
  <p>{{ site.description }}</p>
  <p>目前专注于机器学习和后端开发，喜欢把想法做成能跑起来的东西。这个博客用来记录项目过程和技术笔记。</p>
</div>

---

## 技术栈

{% if site.data.skills %}
  <div class="skills-grid">
    {% for group in site.data.skills %}
      <div class="skill-group">
        <h3>{{ group.category }}</h3>
        <ul>
          {% for skill in group.items %}
            <li>{{ skill }}</li>
          {% endfor %}
        </ul>
      </div>
    {% endfor %}
  </div>
{% endif %}

---

## 联系我

有项目合作或技术交流，欢迎通过以下方式联系：

<div class="about-links">
  <a href="https://github.com/{{ site.github_username }}" target="_blank" rel="noopener" class="btn btn-github">GitHub ↗</a>
  {% for link in site.data.social %}
    {% unless link.name == "GitHub" %}
      <a href="{{ link.url }}" target="_blank" rel="noopener" class="btn btn-secondary">{{ link.name }} ↗</a>
    {% endunless %}
  {% endfor %}
</div>
