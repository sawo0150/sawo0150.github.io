---
title: "thoughts"
layout: category
taxonomy: Thoughts
entries_layout: grid
permalink: /categories/thoughts/
author_profile: true
sidebar_main: true
sidebar:
  nav: "categories"
---

평소 생각과 아이디어를 정리한 글입니다.

{% assign _count = site.categories['Thoughts'] | size %}
{% if _count == 0 %}
<p class="notice--info">아직 작성된 글이 없습니다. 곧 채워질 예정이에요 🙂</p>
{% endif %}
