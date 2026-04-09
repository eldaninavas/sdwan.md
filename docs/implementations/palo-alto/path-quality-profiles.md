---
title: "Palo Alto: Path Quality Profiles"
description: Configure path quality monitoring and SLA profiles for SD-WAN path selection on Palo Alto platforms.
tags:
  - palo-alto
  - path-quality
  - sla
  - monitoring
---

# :material-heart-pulse: Path Quality Profiles

Path quality profiles define the SLA thresholds used for SD-WAN path selection. Both platforms continuously monitor link health and steer traffic based on these profiles.

## PAN-OS Path Quality Profile

### Create a Profile

```
Objects > SD-WAN > Path Quality Profile
  Name: Real-Time
  Sensitivity: High
  SLA Thresholds:
    Latency: 50 ms
    Jitter: 10 ms
    Packet Loss: 0.1%
  
  Name: Interactive
  Sensitivity: Medium
  SLA Thresholds:
    Latency: 150 ms
    Jitter: 30 ms
    Packet Loss: 1%
  
  Name: Best-Effort
  Sensitivity: Low
  SLA Thresholds:
    Latency: 300 ms
    Jitter: 50 ms
    Packet Loss: 5%
```

### Sensitivity Levels

| Sensitivity | Probe Interval | Failback | Use Case |
|-------------|---------------|----------|----------|
| High | 1 second | Fast | VoIP, video |
| Medium | 3 seconds | Normal | Business apps |
| Low | 10 seconds | Slow | Bulk traffic |

### Path Monitor Configuration

```
Network > Interfaces > ethernet1/1 > SD-WAN Interface Profile
  Path Monitor:
    Enable: Yes
    Failure Condition: 3 consecutive
    Recovery Condition: 5 consecutive
    Monitor Destinations:
      - 8.8.8.8 (ICMP)
      - 1.1.1.1 (ICMP)
      - 208.67.222.222 (ICMP)
```

## Prisma SD-WAN Path Quality

Prisma SD-WAN uses **Application Performance Rules** with automatic path quality measurement:

### Path Quality Policy (API)

```python
path_policy = {
    "name": "VoIP-Path-Quality",
    "description": "Real-time traffic path quality requirements",
    "app_def_ids": ["<voip-app-id>"],
    "path_preference": {
        "strategy": "best_available",
        "constraints": {
            "latency_ms": 50,
            "jitter_ms": 10,
            "packet_loss_pct": 0.1
        }
    },
    "service_context": {
        "type": "allowed",
        "backup_service_label": "private-wan"
    }
}

sdk.post.appdefs_path_policies(path_policy)
```

### Autonomous DEM (ADEM)

Prisma SD-WAN includes **Autonomous Digital Experience Monitoring**:

- End-to-end application performance visibility
- Hop-by-hop latency analysis
- Automatic root cause identification
- No additional agents required on ION devices

## Comparison

| Feature | PAN-OS SD-WAN | Prisma SD-WAN |
|---------|--------------|---------------|
| Probe type | ICMP to defined targets | Automatic per-app measurement |
| Metrics | Latency, jitter, loss | Latency, jitter, loss + app response |
| Configuration | Manual profile definition | ML-assisted, policy-based |
| Granularity | Per-interface | Per-application |

!!! tip "Multiple monitor destinations"
    Always configure at least 2-3 path monitor destinations. If a single target goes down, the link will incorrectly show as failed. Use diverse targets (8.8.8.8, 1.1.1.1, and your ISP's gateway).
