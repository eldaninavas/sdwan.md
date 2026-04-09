---
title: "Fortinet: Dual-Hub HA"
description: High availability with dual-hub SD-WAN deployments on FortiGate.
tags:
  - fortinet
  - dual-hub
  - high-availability
---

# :material-server-security: Dual-Hub HA

Dual-hub high availability ensures that SD-WAN connectivity survives hub failures with automatic failover.

## Active-Active Dual Hub

Both hubs actively serve traffic. Spokes establish tunnels to both:

- Load balancing across both hubs via SD-WAN rules
- BGP advertises routes from both hubs with equal preference
- If one hub fails, all traffic shifts to the surviving hub

## Active-Passive Dual Hub

One hub is primary, the other is standby:

- Spokes connect to both but prefer the primary (BGP local preference)
- Standby hub takes over when primary fails
- Simpler design, slightly slower failover

## FortiGate HA at the Hub

For device-level redundancy at each hub:

```bash
config system ha
    set mode a-p
    set group-id 1
    set group-name "SDWAN-Hub"
    set priority 200
    set hbdev "ha1" 50
    set session-pickup enable
    set override disable
end
```

!!! info "Combine both HA types"
    For maximum resilience, deploy FortiGate HA pairs at each hub site AND use dual-hub ADVPN. This protects against both device failures (HA) and site failures (dual-hub).
