---
title: "Palo Alto: Hub & Spoke"
description: Hub-and-spoke SD-WAN topology design for Palo Alto platforms.
tags:
  - palo-alto
  - hub-spoke
  - topology
---

# :material-hub: Hub & Spoke

Hub-and-spoke is the most common SD-WAN topology, with branch sites connecting to one or more centralized hubs.

## PAN-OS Hub-and-Spoke

### VPN Cluster Setup

In Panorama, create a hub-and-spoke VPN cluster:

```
Panorama > SD-WAN > VPN Clusters > Add
  Name: Corp-HubSpoke
  Type: Hub and Spoke
  
  Hub Configuration:
    Device Group: Hub-DG
    Template Stack: Hub-TS
    WAN Interfaces:
      - ethernet1/1 (ISP1: 1Gbps)
      - ethernet1/2 (ISP2: 500Mbps)
      
  Branch Configuration:
    Device Group: Branch-DG
    Template Stack: Branch-TS
    WAN Interfaces:
      - ethernet1/1 (WAN1)
      - ethernet1/2 (WAN2)
    
  Advanced:
    Enable Spoke-to-Spoke via Hub: Yes
    Enable Direct Spoke-to-Spoke: Yes (on-demand)
```

### Spoke-to-Spoke Traffic

PAN-OS SD-WAN supports two spoke-to-spoke models:

1. **Via Hub** -- All inter-branch traffic routes through the hub (default)
2. **Direct** -- Dynamic spoke-to-spoke tunnels on demand (like ADVPN)

### Hub Redundancy

```
Panorama > SD-WAN > VPN Clusters > Corp-HubSpoke
  Hub Members:
    Primary Hub: PA-5260-DC1 (priority 1)
    Secondary Hub: PA-5260-DC2 (priority 2)
  
  Failover:
    Preemptive: No
    Monitor: Hub health checks
```

## Prisma SD-WAN Hub-and-Spoke

### Site Roles

```python
# Hub site configuration
hub_site = {
    "name": "DC-East",
    "element_cluster_role": "HUB",
    "admin_state": "active",
    "policy_set_id": "<hub-policy-set>",
    "hub_cluster_member_id": "<cluster-id>"
}

# Spoke site configuration
spoke_site = {
    "name": "Branch-NYC",
    "element_cluster_role": "SPOKE",
    "admin_state": "active",
    "policy_set_id": "<spoke-policy-set>",
    "hub_cluster_member_id": "<cluster-id>"
}
```

### Topology Rules (API)

```python
topology_rule = {
    "name": "Branch-to-DC",
    "description": "Route branch traffic through DC hub",
    "source_site_ids": ["<all-spoke-sites>"],
    "destination_site_ids": ["<dc-hub-id>"],
    "path_preference": "active_active",
    "network_context_id": "<context-id>"
}

sdk.post.topology_rules(topology_rule)
```

## Design Recommendations

| Hub Count | Spokes | Use Case |
|-----------|--------|----------|
| Single hub | 1-50 | Small deployments |
| Dual hub (same region) | 50-200 | HA requirement |
| Multi-region hubs | 200+ | Global deployments |

!!! tip "Direct spoke-to-spoke"
    Enable direct spoke-to-spoke tunnels for branch-to-branch traffic like VoIP between offices. This eliminates the extra latency hop through the hub. Both platforms support on-demand spoke-to-spoke tunnels.
