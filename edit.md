---
layout: default
title: Edit
permalink: /edit/
---
<article class="entry">
  <div class="entry-head">
    <p class="entry-eyebrow">Codex</p>
    <h1>Edit the Codex</h1>
    <p class="entry-summary">Enter your PIN to add or change a page.</p>
  </div>

  <div class="entry-body">

    <div id="pin-gate">
      <input type="password" inputmode="numeric" id="pin-input" class="search-box" placeholder="PIN" autofocus>
      <button class="btn" id="pin-submit">Unlock</button>
      <p class="editor-note" id="pin-error"></p>
    </div>

    <div id="editor-ui" hidden>

      <div class="editor-tabs">
        <button class="editor-tab active" data-tab="edit">Edit existing page</button>
        <button class="editor-tab" data-tab="new">Create new page</button>
      </div>

      <div id="tab-edit" class="editor-panel">
        <label class="editor-label" for="existing-select">Choose a page</label>
        <select id="existing-select" class="search-box"></select>
      </div>

      <div id="tab-new" class="editor-panel" hidden>
        <label class="editor-label" for="new-category">Section</label>
        <select id="new-category" class="search-box">
          <option value="characters">Characters</option>
          <option value="locations">Locations</option>
          <option value="factions">Factions</option>
          <option value="items">Items &amp; Artifacts</option>
          <option value="sessions">Session Log</option>
        </select>
        <label class="editor-label" for="new-title">Title</label>
        <input type="text" id="new-title" class="search-box" placeholder="e.g. Elyndra Nightshade">
        <p class="editor-note">Will be saved as: <code id="new-filename-preview">—</code></p>
      </div>

      <label class="editor-label" for="editor-textarea">Page content (Markdown)</label>
      <textarea id="editor-textarea" class="editor-textarea" rows="18" spellcheck="true"></textarea>

      <div class="editor-actions">
        <button class="btn" id="save-btn">Save to the codex</button>
        <span id="save-status" class="editor-note"></span>
      </div>
    </div>

  </div>
</article>

<script>
  window.CODEX_CONFIG = {
    workerUrl: {{ site.editor_worker_url | jsonify }}
  };
</script>
<script src="{{ '/assets/js/editor.js' | relative_url }}"></script>
