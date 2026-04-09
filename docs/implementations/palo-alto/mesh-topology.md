---
title: "Palo Alto: Mesh Topology"
description: Full and partial mesh SD-WAN topologies with Palo Alto platforms.
tags:
  - palo-alto
  - mesh
  - topology
---

# :material-vector-polygon: Mesh Topology

Mesh topologies provide direct site-to-site connectivity without routing through a hub.

## Full Mesh

Every site connects directly to every other site:

- Lowest latency for inter-site traffic
- Tunnel count grows exponentially: n(n-1)/2
- Best for small deployments (< 20 sites) with heavy inter-site traffic

### PAN-OS Full Mesh

```
Panorama > SD-WAN > VPN Clusters > Add
  Name: Full-Mesh
  Type: Full Mesh
  
  Sites:
    Device Group: All-Sites-DG
    Template Stack: All-Sites-TS
    
  Tunnel Settings:
    All-to-All: Yes
    Crypto Profile: Suite-B-GCM-256
```

### Prisma SD-WAN Full Mesh

```python
# Anynet automatically creates optimal mesh
# Set topology to full mesh via policy
topology = {
    "type": "mesh",
    "sites": ["<site-1>", "<site-2>", "<site-3>"],
    "tunnel_type": "ipsec",
    "admin_state": "active"
}
```

## Partial Mesh (Hybrid)

Combination of hub-spoke for default traffic and dynamic direct tunnels on demand:

- Hub-spoke as baseline topology
- On-demand direct tunnels between spokes when traffic justifies it
- Best balance of simplicity and performance
- Most common production design

## Tunnel Count Calculator

| Sites | Full Mesh Tunnels | Hub-Spoke Tunnels |
|-------|-------------------|-------------------|
| 10 | 45 | 10 |
| 50 | 1,225 | 50 |
| 100 | 4,950 | 100 |
| 500 | 124,750 | 500 |

!!! warning "Full mesh scalability"
    Full mesh becomes impractical beyond ~50 sites due to tunnel count. Use hub-spoke with on-demand spoke-to-spoke tunnels for larger deployments.
