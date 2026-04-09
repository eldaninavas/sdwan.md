---
title: "Palo Alto: High Availability"
description: SD-WAN high availability configurations for PAN-OS and Prisma SD-WAN.
tags:
  - palo-alto
  - high-availability
  - redundancy
---

# :material-server-security: High Availability

HA ensures SD-WAN connectivity remains operational during device, link, or site failures.

## PAN-OS HA for SD-WAN

### Active/Passive HA Pair

```
Device > High Availability > General
  Setup:
    Enable HA: Yes
    Group ID: 1
    Mode: Active/Passive
    Peer HA1 Address: 10.0.0.2
    
  Election Settings:
    Priority: 100 (primary), 50 (secondary)
    Preemptive: No
    
  Link Monitoring:
    Enable: Yes
    Failure Condition: any
    Interfaces: ethernet1/1, ethernet1/2 (WAN links)
    
  Path Monitoring:
    Enable: Yes
    Destinations: 8.8.8.8, 1.1.1.1
```

### Active/Active HA (SD-WAN)

```
Device > High Availability > General
  Mode: Active/Active
  
  Packet Distribution:
    Session Setup: Primary Device
    
  SD-WAN Considerations:
    - Both devices participate in SD-WAN
    - Floating IP for tunnel endpoints
    - Shared session table for failover
```

## Prisma SD-WAN HA

### ION HA Pair

```python
# Configure HA cluster for ION devices at a site
ha_config = {
    "name": "Branch-NYC-HA",
    "site_id": "<site-id>",
    "cluster_type": "active_standby",
    "elements": [
        {
            "element_id": "<ion-primary-id>",
            "role": "primary",
            "priority": 100
        },
        {
            "element_id": "<ion-secondary-id>",
            "role": "secondary",
            "priority": 50
        }
    ],
    "preemption": False,
    "health_check_interval": 1000
}

sdk.post.element_clusters(ha_config)
```

## HA Design Matrix

| Layer | PAN-OS | Prisma SD-WAN |
|-------|--------|---------------|
| Device HA | Active/Passive or A/A | Active/Standby ION pair |
| Link redundancy | Multi-WAN + SD-WAN failover | Multi-WAN + path selection |
| Hub redundancy | Dual hub VPN cluster | Multi-hub Anynet |
| Controller | Panorama HA pair | Cloud-native (built-in) |

!!! info "Prisma SD-WAN controller HA"
    The Prisma SD-WAN controller is cloud-native and inherently highly available. You don't need to manage controller HA -- Palo Alto handles it in their cloud infrastructure.
