---
layout: archive
title: ""
permalink: /blog/
author_profile: true
---

<style>
.blog-header {
    margin: 4px 0 28px;
}
.blog-header h1 {
    font-family: "Inter", "Segoe UI", -apple-system, BlinkMacSystemFont, sans-serif;
    font-weight: 800;
    font-size: 2.1rem;
    color: #222;
    margin: 0 0 6px;
    letter-spacing: 0.2px;
}
.blog-header .blog-subtitle {
    color: #666;
    font-size: 0.98rem;
    margin: 0;
}
.blog-header::after {
    content: "";
    display: block;
    width: 64px;
    height: 3px;
    background: #4485C7;
    margin-top: 14px;
    border-radius: 3px;
}

.post-list {
    list-style: none;
    margin: 0;
    padding: 0;
}
.post-list-item {
    background: #fff;
    border: 1px solid #e5e9ef;
    border-radius: 12px;
    padding: 20px 24px;
    margin-bottom: 16px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.04);
    transition: transform 0.2s ease, box-shadow 0.2s ease, border-color 0.2s ease;
}
.post-list-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 22px rgba(68,133,199,0.16);
    border-color: #c5d8ef;
}
.post-list-item a.post-link {
    text-decoration: none;
    color: inherit;
    display: block;
}
.post-meta {
    font-size: 0.82rem;
    color: #888;
    letter-spacing: 0.4px;
    text-transform: uppercase;
    margin-bottom: 4px;
}
.post-title {
    font-size: 1.25rem;
    font-weight: 700;
    color: #1f3a68;
    margin: 0 0 6px;
    line-height: 1.4;
    transition: color 0.18s;
}
.post-list-item:hover .post-title {
    color: #4485C7;
}
.post-excerpt {
    color: #555;
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0 0 6px;
}
.post-tags {
    margin-top: 4px;
}
.post-tag {
    display: inline-block;
    color: #4485C7;
    font-size: 0.78rem;
    margin-right: 8px;
}
.blog-empty {
    color: #777;
    font-style: italic;
    padding: 14px 0;
}
</style>

<div class="blog-header">
  <h1>Blog</h1>
  <!-- <p class="blog-subtitle">Notes on optimization, reasoning, and other things I am working on.</p> -->
</div>

{% if site.posts.size == 0 %}
  <p class="blog-empty">No posts yet. Stay tuned.</p>
{% else %}
  <ul class="post-list">
  {% for post in site.posts %}
    <li class="post-list-item">
      <a class="post-link" href="{{ post.url | relative_url }}">
        <div class="post-meta">
          <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %d, %Y" }}</time>
        </div>
        <h2 class="post-title">{{ post.title }}</h2>
        {% if post.excerpt %}
          <p class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 45 }}</p>
        {% endif %}
        {% if post.tags %}
          <div class="post-tags">
            {% for tag in post.tags %}<span class="post-tag">#{{ tag }}</span>{% endfor %}
          </div>
        {% endif %}
      </a>
    </li>
  {% endfor %}
  </ul>
{% endif %}

<div style="height: 100px;"></div>
