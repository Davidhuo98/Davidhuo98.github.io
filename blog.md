---
layout: default
title: 文章
permalink: /blog/
---

<section class="page-header">
  <h1>文章</h1>
</section>

<div class="tags-filter">
  <button class="tag filter-btn active" data-tag="all">全部</button>
  {% assign all_tags = site.posts | map: "tags" | join: "," | split: "," | uniq | sort %}
  {% for tag in all_tags %}
    <button class="tag filter-btn" data-tag="{{ tag }}">{{ tag }}</button>
  {% endfor %}
</div>

<div class="posts-list" id="posts-list">
  {% for post in site.posts %}
    <article class="post-item" data-tags="{{ post.tags | join: ',' }}">
      <time datetime="{{ post.date | date_to_xmlschema }}">
        {{ post.date | date: "%Y-%m-%d" }}
      </time>
      <div class="post-item-main">
        <a href="{{ post.url | relative_url }}" class="post-item-title">
          {{ post.title }}
        </a>
        {% if post.excerpt %}
          <p class="post-item-excerpt">
            {{ post.excerpt | strip_html | truncatewords: 30 }}
          </p>
        {% endif %}
      </div>
      {% if post.tags %}
        <div class="post-item-tags">
          {% for tag in post.tags limit: 3 %}
            <button class="tag filter-btn" data-tag="{{ tag }}">{{ tag }}</button>
          {% endfor %}
        </div>
      {% endif %}
    </article>
  {% endfor %}
</div>

<script>
const btns = document.querySelectorAll('.filter-btn');
const posts = document.querySelectorAll('.post-item');

btns.forEach(btn => {
  btn.addEventListener('click', () => {
    const tag = btn.dataset.tag;

    btns.forEach(b => b.classList.remove('active'));
    document.querySelectorAll(`[data-tag="${tag}"]`).forEach(b => b.classList.add('active'));

    posts.forEach(post => {
      const tags = post.dataset.tags.split(',');
      post.style.display = (tag === 'all' || tags.includes(tag)) ? '' : 'none';
    });
  });
});
</script>
