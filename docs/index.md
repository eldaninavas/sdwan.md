---
title: SD-WAN - Software-Defined Wide Area Networking Knowledge Base
description: The definitive open-source guide to SD-WAN. Learn what SD-WAN is, how it works, use cases, vendor implementations (Fortinet, Cisco, VMware), and best practices.
tags:
  - home
  - sd-wan
  - software-defined-wan
  - sdwan tutorial
hide:
  - toc
---

<div class="sdwan-hero" markdown>

# SD-WAN: Software-Defined WAN

<p class="sdwan-tagline">
  The definitive open-source knowledge base for <strong>SD-WAN</strong>.<br>
  Learn how SD-WAN works. Configure it. Contribute.
</p>

[:material-rocket-launch: &nbsp; Get Started](topics/index.md){ .sdwan-btn .sdwan-btn-primary }
&nbsp;&nbsp;
[:fontawesome-brands-github: &nbsp; Contribute](https://github.com/eldaninavas/sdwan.md){ .sdwan-btn .sdwan-btn-secondary }

</div>

<div class="sdwan-stats" markdown>
<div class="sdwan-stat">
  <span class="sdwan-stat-number">20+</span>
  <span class="sdwan-stat-label">Topics Covered</span>
</div>
<div class="sdwan-stat">
  <span class="sdwan-stat-number">9+</span>
  <span class="sdwan-stat-label">Use Cases</span>
</div>
<div class="sdwan-stat">
  <span class="sdwan-stat-number">100%</span>
  <span class="sdwan-stat-label">Open Source</span>
</div>
</div>

---

## :material-compass: Explore

<div class="sdwan-grid" markdown>

<div class="sdwan-card" markdown>

### :material-book-open-variant: Topics

New to SD-WAN? Start here. Understand architecture, overlay networks, application-aware routing, and security.

[:material-arrow-right: Learn the basics](topics/index.md){ .md-button }

</div>

<div class="sdwan-card" markdown>

### :material-traffic-light: Use Cases

Discover real-world applications: branch connectivity, multi-cloud access, SaaS optimization, MPLS migration, and more.

[:material-arrow-right: See use cases](use-cases/index.md){ .md-button }

</div>

<div class="sdwan-card" markdown>

### :material-server-network: Implementations

Hands-on configuration guides for Fortinet FortiGate, with more vendors coming soon.

[:material-arrow-right: View implementations](implementations/index.md){ .md-button }

</div>

<div class="sdwan-card" markdown>

### :material-account-group: Community

Join the community! Learn how to contribute, report issues, or suggest new content.

[:material-arrow-right: Get involved](community/index.md){ .md-button }

</div>

</div>

---

## :material-head-question: What is SD-WAN and How Does It Work?

**SD-WAN (Software-Defined Wide Area Networking)** is a virtual WAN architecture that allows enterprises to leverage any combination of transport services -- including MPLS, LTE, and broadband internet -- to securely connect users to applications. A centralized control function intelligently directs traffic across the WAN based on application requirements and real-time link quality.

SD-WAN works by creating an **overlay network** on top of existing WAN connections. Devices at each site establish encrypted tunnels and the centralized controller manages policies, security, and traffic steering dynamically. This enables powerful capabilities like application-aware routing, zero-touch provisioning, and built-in encryption -- all while reducing costs and complexity.

**Why choose SD-WAN over traditional WAN?**

- **Cost reduction** -- Leverage affordable broadband and LTE alongside or instead of expensive MPLS
- **Application-aware routing** -- Steer traffic based on app identity and real-time SLA metrics
- **Centralized management** -- Single pane of glass for all WAN sites, policies, and security
- **Transport independence** -- Use any combination of MPLS, broadband, LTE/5G, and satellite
- **Built-in security** -- IPsec encryption, segmentation, and integration with SASE frameworks
- **Zero-touch provisioning** -- Deploy new sites in minutes, not weeks

[:material-arrow-right: Learn what SD-WAN is in detail](topics/what-is-sdwan.md){ .md-button }  &nbsp; [:material-arrow-right: See SD-WAN architecture](topics/architecture.md){ .md-button }

!!! tip "This is a community project"
    SDWAN.md is built and maintained by the networking community. Every page has an **edit button** (:material-pencil:) -- if you spot an error or want to add content, we welcome your contributions!

---

<p style="text-align: center; color: var(--md-default-fg-color--lighter); font-size: 0.85rem;">
  SDWAN.md is not affiliated with any vendor. Content is provided under the MIT License.
</p>
