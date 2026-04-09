---
title: High Availability
description: SD-WAN redundancy, failover mechanisms, and resilience design patterns.
tags:
  - high-availability
  - redundancy
  - failover
---

# :material-server-security: High Availability

High availability (HA) in SD-WAN ensures that WAN connectivity remains operational even when individual components fail.

## HA Layers

### Transport Redundancy

- Multiple WAN links per site (dual broadband, MPLS + broadband, etc.)
- Active-active or active-standby link configuration
- Automatic failover based on health checks

### Device Redundancy

- Active-passive or active-active FortiGate/edge pairs at critical sites
- Stateful failover maintains existing sessions
- Typically deployed at hubs and large branches

### Hub/Controller Redundancy

- Dual-hub deployments for hub-and-spoke topologies
- Controller clustering for management plane availability
- Geographic distribution for disaster recovery

## Design Patterns

| Pattern | Sites | Description |
|---------|-------|-------------|
| Single Hub | Small deployments | Simple, low cost |
| Dual Hub (Active-Active) | Medium | Both hubs serve traffic |
| Dual Hub (Active-Passive) | Medium | Standby hub for failover |
| Multi-Hub Regional | Large/Global | Regional hubs reduce latency |
| Full Mesh | Any | Direct site-to-site, no hub dependency |

!!! info "Fortinet dual-hub"
    Fortinet supports dual-hub HA with ADVPN for automatic spoke-to-spoke tunnels. See [Dual-Hub HA](../implementations/fortinet/dual-hub-ha.md).
