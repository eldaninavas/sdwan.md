---
title: SaaS Optimization
description: SD-WAN for optimizing SaaS application performance — Microsoft 365, Salesforce, Zoom.
tags:
  - saas
  - microsoft-365
  - optimization
  - use-case
---

# :material-cloud-check: SaaS Optimization

SD-WAN dramatically improves SaaS application performance by enabling direct internet breakout and intelligent path selection for cloud applications.

## The Problem

In traditional WANs, SaaS traffic is backhauled through the data center:

- Branch -> MPLS -> Data Center -> Internet -> SaaS
- Adds 100-300ms of unnecessary latency
- Creates a bottleneck at the data center internet edge
- Poor user experience for Microsoft 365, Zoom, Salesforce

## SD-WAN Solution

Direct internet breakout at each branch:

- Branch -> Local Internet -> SaaS
- Traffic identified by DPI and steered directly to the internet
- Full security applied locally (NGFW, web filter)
- SLA monitoring ensures quality to the SaaS provider

## SaaS-Specific Optimizations

| Application | Optimization | Impact |
|-------------|-------------|--------|
| Microsoft 365 | Direct breakout + MS peering | 50-80% latency reduction |
| Zoom/Teams | Lowest-latency path selection | Better video/audio quality |
| Salesforce | Persistent connections | Faster page loads |
| SAP | Priority path + QoS | Consistent ERP performance |

!!! tip "Microsoft recommends local breakout"
    Microsoft explicitly recommends local internet breakout for Microsoft 365 traffic. SD-WAN is the ideal technology to implement this recommendation securely.
