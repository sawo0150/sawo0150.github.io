---
title: "competition"
layout: category
taxonomy: Competition
entries_layout: grid
permalink: /categories/competition/
author_profile: true
sidebar_main: true
sidebar:
  nav: "categories"
---

각종 대회, 챌린지, 공모전 참가 기록입니다.

{% assign _count = site.categories['Competition'] | size %}
{% if _count == 0 %}
<p class="notice--info">아직 작성된 글이 없습니다. 곧 채워질 예정이에요 🙂</p>
{% endif %}
