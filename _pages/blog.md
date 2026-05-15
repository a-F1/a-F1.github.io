---
layout: archive
title: ""
permalink: /blog/
author_profile: true
---

<style>
.blog-section-title {
    font-family: "Inter", "Segoe UI", -apple-system, BlinkMacSystemFont, sans-serif;
    font-weight: 800;
    font-size: 2.2rem;
    color: #333;
    margin: 10px 0 28px;
    letter-spacing: 0.3px;
    position: relative;
    padding-bottom: 0.4em;
}
.blog-section-title::after {
    content: "";
    display: block;
    width: 80px;
    height: 3px;
    background: #4485C7;
    margin-top: 10px;
    border-radius: 3px;
}

.blog-empty {
    color: #777;
    font-style: italic;
    padding: 14px 0;
}

.blog-card {
    display: block;
    text-decoration: none;
    color: inherit;
    padding: 22px 26px;
    margin-bottom: 18px;
    background: #fff;
    border-radius: 12px;
    box-shadow: 0 3px 15px rgba(0,0,0,0.06);
    border: 1px solid rgba(0,0,0,0.05);
    transition: transform 0.25s, box-shadow 0.25s;
}
.blog-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 8px 25px rgba(68,133,199,0.18);
    color: inherit;
    text-decoration: none;
}
.blog-card-title {
    font-size: 1.2rem;
    font-weight: 700;
    color: #1f3a68;
    margin: 0 0 6px;
    line-height: 1.4;
}
.blog-card-meta {
    font-size: 0.9rem;
    color: #666;
    margin-bottom: 8px;
}
.blog-card-meta .blog-tag {
    display: inline-block;
    background: #f0f6ff;
    color: #1f4ca7;
    border-radius: 20px;
    padding: 2px 10px;
    margin-right: 6px;
    font-size: 0.78rem;
    font-weight: 600;
    border: 1px solid #d8e4f5;
}
.blog-card-excerpt {
    font-size: 0.98rem;
    color: #444;
    line-height: 1.6;
    margin: 0;
}
</style>

<div class="blog-section-title">📝 Blog</div>

{% if site.posts.size == 0 %}
  <p class="blog-empty">No posts yet. Stay tuned.</p>
{% else %}
  {% for post in site.posts %}
    <a class="blog-card" href="{{ post.url | relative_url }}">
      <div class="blog-card-title">{{ post.title }}</div>
      <div class="blog-card-meta">
        <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%B %d, %Y" }}</time>
        {% if post.tags %}
          &nbsp;·&nbsp;
          {% for tag in post.tags %}<span class="blog-tag">{{ tag }}</span>{% endfor %}
        {% endif %}
      </div>
      {% if post.excerpt %}
        <p class="blog-card-excerpt">{{ post.excerpt | strip_html | truncatewords: 50 }}</p>
      {% endif %}
    </a>
  {% endfor %}
{% endif %}

<div style="height: 100px;"></div>
