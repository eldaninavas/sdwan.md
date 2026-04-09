---
title: Traffic Steering & SLA
description: SLA-based traffic steering policies, failover strategies, and performance thresholds in SD-WAN.
tags:
  - traffic-steering
  - sla
  - policies
---

# :material-steering: Traffic Steering & SLA

Traffic steering is the process of directing application traffic to the optimal WAN path based on predefined SLA (Service Level Agreement) criteria.

## SLA Profiles

An SLA profile defines the acceptable performance thresholds for a class of traffic:

| SLA Profile | Latency | Jitter | Packet Loss | Use Case |
|-------------|---------|--------|-------------|----------|
| Real-Time | < 150ms | < 30ms | < 1% | VoIP, video conferencing |
| Interactive | < 200ms | < 50ms | < 2% | ERP, CRM, web apps |
| Bulk | < 500ms | N/A | < 5% | Backups, file sync |
| Best Effort | N/A | N/A | N/A | Updates, non-critical |

## Steering Strategies

### Lowest Latency

Route traffic to the path with the lowest measured latency. Best for real-time applications.

### Best Quality (Composite)

Use a weighted combination of latency, jitter, and packet loss to determine the best overall path.

### Cost-Based

Prefer lower-cost links (broadband over MPLS) when SLA requirements are met.

### Manual Priority

Administrator defines a strict preference order; failover only when the preferred path violates SLA.

## Load Balancing

SD-WAN can distribute traffic across multiple paths:

- **Per-source IP** -- All traffic from a source uses the same path
- **Per-source-destination** -- Pairs are pinned to a path
- **Per-session** -- Each new session is assigned independently
- **Weighted** -- Distribute proportionally based on link capacity

!!! info "Fortinet implementation"
    Fortinet SD-WAN supports all these strategies via SD-WAN rules with configurable load balancing modes. See [Fortinet SD-WAN Rules](../implementations/fortinet/sdwan-rules.md) for configuration examples.
