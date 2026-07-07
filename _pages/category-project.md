---
title: "project"
layout: category
taxonomy: Project
entries_layout: grid
permalink: /categories/project/
author_profile: true
sidebar_main: true
sidebar:
  nav: "categories"
---

학교 밖에서 진행한 개인/사이드 프로젝트 모음입니다.

{% assign _count = site.categories['Project'] | size %}
{% if _count == 0 %}
<p class="notice--info">아직 작성된 글이 없습니다. 곧 채워질 예정이에요 🙂</p>
{% endif %}
