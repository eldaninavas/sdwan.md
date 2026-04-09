---
title: Knowledge Graph
description: Interactive visualization of SD-WAN topics, use cases, and implementations and how they connect.
hide:
  - toc
---

# :material-graph: Knowledge Graph

Explore how SD-WAN topics, use cases, and implementations are interconnected. Click any node to navigate to that page.

<div id="filters" style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:1rem;"></div>

<div id="graph-container" style="width:100%;height:70vh;border:1px solid rgba(38,166,154,0.15);border-radius:12px;background:rgba(10,26,24,0.3);"></div>

<div style="margin-top:0.5rem;display:flex;gap:1rem;align-items:center;">
  <button id="fs-btn" onclick="toggleFS()" style="padding:0.4rem 1rem;border-radius:6px;border:1px solid rgba(38,166,154,0.3);background:transparent;color:var(--md-default-fg-color--light);cursor:pointer;font-size:0.8rem;">Fullscreen</button>
</div>

<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/cytoscape-panzoom@2.5.3/cytoscape.js-panzoom.css">
<script src="https://cdn.jsdelivr.net/npm/cytoscape@3.28.1/dist/cytoscape.min.js"></script>
<script src="assets/javascripts/knowledge-graph.js"></script>
