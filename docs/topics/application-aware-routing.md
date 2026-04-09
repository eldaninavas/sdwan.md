---
title: Application-Aware Routing
description: How SD-WAN identifies applications using DPI and routes them based on business policies.
tags:
  - routing
  - dpi
  - application-awareness
---

# :material-routes: Application-Aware Routing

Application-aware routing is the cornerstone of SD-WAN. Unlike traditional routing that makes decisions based on IP prefixes, SD-WAN identifies **applications** and routes them according to business intent.

## Application Identification

SD-WAN uses several methods to identify applications:

- **Deep Packet Inspection (DPI)** -- Analyzes packet headers and payloads
- **SNI inspection** -- Reads the Server Name Indication in TLS handshakes
- **IP/Port matching** -- Traditional L3/L4 classification
- **DNS-based** -- Resolves domain names to identify SaaS applications
- **Cloud signatures** -- Vendor-maintained databases of cloud app IP ranges

## Policy-Based Routing

Once an application is identified, SD-WAN applies routing policies:

```
Application: Microsoft 365
├── Preferred Path: Direct Internet Breakout (ISP1)
├── Backup Path: ISP2
├── SLA Requirement: Latency < 100ms, Loss < 0.5%
└── Action on SLA violation: Failover to backup

Application: VoIP (RTP)
├── Preferred Path: MPLS
├── Backup Path: ISP1 (lowest latency)
├── SLA Requirement: Latency < 150ms, Jitter < 30ms
└── Action on SLA violation: Immediate failover

Application: Backup Traffic
├── Preferred Path: ISP2 (cheapest)
├── Backup Path: ISP1
├── SLA Requirement: None (best effort)
└── Action on SLA violation: None
```

## Application Categories

Most SD-WAN vendors group applications into tiers:

| Tier | Examples | Policy |
|------|----------|--------|
| Business Critical | ERP, CRM, VoIP | Best path, strict SLA |
| SaaS | M365, Salesforce, Zoom | Direct internet breakout |
| Default | Web browsing, email | Load balanced |
| Non-Essential | Social media, streaming | Lowest-cost path |

!!! tip "Fortinet application awareness"
    FortiGate SD-WAN uses the FortiGuard ISDB (Internet Service Database) to identify thousands of applications and cloud services. See [Fortinet SD-WAN Rules](../implementations/fortinet/sdwan-rules.md) for configuration details.
