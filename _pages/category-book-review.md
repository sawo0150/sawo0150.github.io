---
title: "book review"
layout: category
taxonomy: BookReview
entries_layout: grid
permalink: /categories/book-review/
author_profile: true
sidebar_main: true
sidebar:
  nav: "categories"
---

책을 읽고 남긴 기록입니다.

{% assign _count = site.categories['BookReview'] | size %}
{% if _count == 0 %}
<p class="notice--info">아직 작성된 글이 없습니다. 곧 채워질 예정이에요 🙂</p>
{% endif %}
