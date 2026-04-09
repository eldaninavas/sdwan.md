---
title: Transport Independence
description: How SD-WAN enables the use of any combination of MPLS, broadband, LTE/5G, and satellite transport.
tags:
  - transport
  - mpls
  - broadband
  - lte
---

# :material-swap-horizontal: Transport Independence

One of SD-WAN's most powerful features is **transport independence** -- the ability to use any combination of WAN transport services without being locked into a single provider or technology.

## Supported Transport Types

| Transport | Bandwidth | Latency | Cost | Best For |
|-----------|-----------|---------|------|----------|
| MPLS | Medium | Low | High | Critical real-time apps |
| Broadband | High | Variable | Low | Bulk data, general use |
| LTE/5G | Medium | Medium | Medium | Backup, mobile sites |
| Satellite | Low-Medium | High | High | Remote locations |
| P2P Fiber | Very High | Very Low | Medium | Data center interconnect |

## How It Works

SD-WAN treats all transports as equal members of the overlay. The policy engine decides which transport to use based on:

1. **Application requirements** -- VoIP needs low latency; backups need bandwidth
2. **Real-time link quality** -- Measured via active health checks (ping, HTTP, DNS)
3. **Cost policies** -- Prefer cheaper links for non-critical traffic
4. **Compliance** -- Some traffic must stay on private circuits

## Dual-ISP Best Practices

- Use different ISPs at each site for true diversity
- Ensure different last-mile technologies when possible (fiber + cable, or fiber + LTE)
- Configure active-active with weighted load balancing
- Set SLA thresholds appropriate for each application tier

!!! example "Typical branch design"
    A typical branch uses two ISPs (broadband) as primary with LTE as backup. SD-WAN health checks monitor all three links and dynamically steers traffic to maintain application SLAs.
