---
layout: default
title: Search
permalink: /search/
---
<article class="entry">
  <div class="entry-head">
    <p class="entry-eyebrow">Codex</p>
    <h1>Search</h1>
    <p class="entry-summary">Find any page by name, tag, or a word from its text.</p>
  </div>
  <div class="entry-body">
    <input type="text" id="search-input" class="search-box" placeholder="Search the codex…" autofocus data-index-url="{{ '/search.json' | relative_url }}">
    <ul class="card-list" id="search-results"></ul>
  </div>
</article>

<script src="{{ '/assets/js/search.js' | relative_url }}"></script>
