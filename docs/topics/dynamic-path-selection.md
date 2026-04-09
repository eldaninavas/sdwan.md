---
title: Dynamic Path Selection
description: Real-time path selection in SD-WAN based on link quality metrics like latency, jitter, and packet loss.
tags:
  - path-selection
  - sla
  - failover
---

# :material-transit-connection-variant: Dynamic Path Selection

Dynamic path selection allows SD-WAN to continuously monitor link health and move traffic between paths in real-time to maintain application performance.

## How It Works

1. **Health probes** -- SD-WAN sends periodic probes (ICMP, HTTP, DNS) across each WAN link
2. **Metric collection** -- Latency, jitter, packet loss, and bandwidth utilization are measured
3. **SLA evaluation** -- Measured values are compared against defined thresholds
4. **Path decision** -- Traffic is steered to the best path meeting the SLA

## Key Metrics

| Metric | What It Measures | Impact |
|--------|------------------|--------|
| Latency | Round-trip time | VoIP quality, app responsiveness |
| Jitter | Variation in latency | Audio/video quality |
| Packet Loss | Percentage of lost packets | Retransmissions, quality degradation |
| MOS Score | Mean Opinion Score (derived) | Overall voice quality indicator |
| Bandwidth | Available throughput | Capacity for bulk transfers |

## Failover Behavior

### Sub-Second Failover

Modern SD-WAN achieves failover in under 1 second:

- Health probes detect degradation within 1-3 probe intervals
- Traffic is instantly redirected to an alternate path
- No routing convergence needed (overlay switching)
- Sessions can be maintained across path changes

### Failback

When the original path recovers:

- Some vendors failback immediately
- Others use a hold-down timer to prevent flapping
- Configurable per vendor and per policy

!!! warning "Probe interval tuning"
    Aggressive probe intervals (e.g., 500ms) detect failures faster but increase overhead. Balance detection speed with bandwidth consumption, especially on metered links like LTE.
