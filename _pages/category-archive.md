---
title: "카테고리"
layout: archive
permalink: /categories/
author_profile: true
sidebar_main: true
---

글의 성격에 따라 카테고리별로 모아봤어요.

<div class="feature__wrapper">
{% for cat in site.data.categories %}
  {% assign count = site.categories[cat.taxonomy] | size %}
  <div class="feature__item">
    <div class="archive__item">
      <div class="archive__item-body">
        <h2 class="archive__item-title"><i class="{{ cat.icon }}" aria-hidden="true"></i> {{ cat.label }}</h2>
        <div class="archive__item-excerpt">
          <p>{{ cat.description }}</p>
          <p>{{ count }}개의 글</p>
        </div>
        <p><a href="{{ site.baseurl }}/categories/{{ cat.slug }}/" class="btn btn--primary">보러 가기</a></p>
      </div>
    </div>
  </div>
{% endfor %}
</div>
