---
title: "Fortinet: SD-WAN Zones"
description: Configure SD-WAN zones on FortiGate for traffic segmentation and policy control.
tags:
  - fortinet
  - zones
  - segmentation
---

# :material-vector-polygon: Fortinet SD-WAN Zones

SD-WAN zones group SD-WAN members logically and are referenced in firewall policies, similar to traditional firewall zones.

## Default Zone

When SD-WAN is enabled, FortiGate creates a default zone called `virtual-wan-link`. All SD-WAN members are added to this zone by default.

## Custom Zones

You can create custom zones to segment SD-WAN traffic:

```bash
config system sdwan
    config zone
        edit "underlay"
        next
        edit "overlay"
        next
    end
end
```

## Assign Members to Zones

```bash
config system sdwan
    config members
        edit 1
            set interface "wan1"
            set zone "underlay"
            set gateway 203.0.113.1
        next
        edit 2
            set interface "wan2"
            set zone "underlay"
            set gateway 198.51.100.1
        next
        edit 3
            set interface "vpn-hub1"
            set zone "overlay"
        next
        edit 4
            set interface "vpn-hub2"
            set zone "overlay"
        next
    end
end
```

## Use Zones in Firewall Policies

```bash
config firewall policy
    edit 1
        set name "LAN-to-Overlay"
        set srcintf "lan"
        set dstintf "overlay"
        set srcaddr "all"
        set dstaddr "corporate-subnets"
        set action accept
        set schedule "always"
        set service "ALL"
    next
    edit 2
        set name "LAN-to-Internet"
        set srcintf "lan"
        set dstintf "underlay"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
```

## Zone Design Best Practices

| Zone | Members | Use Case |
|------|---------|----------|
| `underlay` | wan1, wan2, lte | Direct internet, SaaS breakout |
| `overlay` | vpn-hub1, vpn-hub2 | Corporate traffic via VPN to DC |
| `sla-sensitive` | wan1 (MPLS) | Ultra-low-latency apps (if MPLS exists) |

!!! info "Zones simplify policy management"
    Using zones, you can write a single firewall policy for all overlay members instead of per-interface policies. This scales much better as you add more WAN links or VPN tunnels.
