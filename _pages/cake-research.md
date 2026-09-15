---
layout: page
title: cake research
permalink: /cake-research/
description: Experiments in cake, made for birthdays, gatherings, and the occasional craving.
nav: true
nav_order: 3
_styles: |
  .cake-gallery {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 2.5rem 2rem;
  }
  .cake-entry { min-width: 0; }
  .cake-photos {
    display: flex;
    flex-wrap: nowrap;
    align-items: center;
    gap: 0.5rem;
    margin-bottom: 1rem;
  }
  .cake-photos a { flex: 1; min-width: 0; }
  .cake-photos img {
    display: block;
    width: 100%;
    height: auto;
    border-radius: 0.35rem;
  }
  .cake-entry h2 { font-size: 1.35rem; }
  .cake-entry p { margin-bottom: 0; }
  @media (max-width: 575px) {
    .cake-gallery { grid-template-columns: minmax(0, 1fr); }
  }
---

<div class="cake-gallery">
  {% for cake in site.data.cakes %}
    <section class="cake-entry" aria-labelledby="cake-{{ forloop.index }}">
      <div class="cake-photos">
        {% for photo in cake.images %}
          <a href="{{ photo.path | relative_url }}" aria-label="View full photo: {{ photo.alt | escape }}">
            <img src="{{ photo.path | relative_url }}" alt="{{ photo.alt | escape }}" loading="lazy" decoding="async">
          </a>
        {% endfor %}
      </div>
      <h2 id="cake-{{ forloop.index }}">{{ cake.title }}</h2>
      <p>{{ cake.caption }}</p>
    </section>
  {% endfor %}
</div>
