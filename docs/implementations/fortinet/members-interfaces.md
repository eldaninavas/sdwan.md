---
title: "Fortinet: SD-WAN Members & Interfaces"
description: Configure SD-WAN members, interface assignments, costs, and weights on FortiGate.
tags:
  - fortinet
  - members
  - interfaces
---

# :material-ethernet: SD-WAN Members & Interfaces

SD-WAN members are the WAN interfaces that participate in the SD-WAN overlay. Each member has properties that control how it's used.

## Member Configuration

```bash
config system sdwan
    config members
        edit 1
            set interface "wan1"
            set zone "virtual-wan-link"
            set gateway 203.0.113.1
            set cost 10
            set weight 1
            set priority 1
            set status enable
            set comment "Primary ISP - Fiber"
        next
        edit 2
            set interface "wan2"
            set zone "virtual-wan-link"
            set gateway 198.51.100.1
            set cost 20
            set weight 1
            set priority 2
            set status enable
            set comment "Secondary ISP - Cable"
        next
        edit 3
            set interface "lte"
            set zone "virtual-wan-link"
            set gateway 0.0.0.0
            set cost 100
            set weight 1
            set priority 3
            set status enable
            set comment "LTE Backup"
        next
    end
end
```

## Member Properties

| Property | Description | Example |
|----------|-------------|---------|
| `interface` | Physical or virtual WAN interface | `wan1` |
| `zone` | SD-WAN zone assignment | `virtual-wan-link` |
| `gateway` | Next-hop gateway for this link | `203.0.113.1` |
| `cost` | Cost metric (lower = preferred) | `10` |
| `weight` | Load balancing weight | `1` |
| `priority` | Priority for failover | `1` |
| `status` | Enable/disable member | `enable` |
| `volume-ratio` | Traffic volume ratio for load balancing | `1` |
| `spillover-threshold` | Bandwidth threshold before spilling to next member | `0` (disabled) |

## VPN Tunnel Members

IPsec VPN tunnels can also be SD-WAN members:

```bash
config system sdwan
    config members
        edit 10
            set interface "vpn-to-hub1"
            set zone "overlay"
        next
        edit 11
            set interface "vpn-to-hub2"
            set zone "overlay"
        next
    end
end
```

!!! tip "Cost and priority"
    Use `cost` for load balancing decisions (traffic prefers lower-cost members). Use `priority` for strict failover (traffic only uses higher-priority members when lower-priority members fail health checks).
