---
title: "Fortinet: Multi-Hub Deployments"
description: Multi-hub SD-WAN deployments with Fortinet — regional hubs, failover, and load balancing.
tags:
  - fortinet
  - multi-hub
  - regional
---

# :material-server-network: Multi-Hub Deployments

Multi-hub deployments use two or more hubs to provide regional coverage, load balancing, and redundancy.

## Design Patterns

### Dual Hub (Same Region)

Both hubs in the same data center or region for HA:

- Active-Active: Spokes connect to both, traffic distributed
- Active-Passive: Spokes connect to both, one is standby

### Regional Hubs

Hubs in different geographic regions:

- Spokes connect to the nearest hub
- Inter-hub backbone tunnels
- Regional failover if a hub goes down

## Spoke Configuration (Dual Hub)

```bash
# Tunnel to Hub 1
config vpn ipsec phase1-interface
    edit "vpn-hub1"
        set interface "wan1"
        set remote-gw 203.0.113.1
        set ike-version 2
        set auto-discovery-receiver enable
        set auto-discovery-shortcuts independent
        set psksecret "secret1"
        set net-device enable
        set add-route disable
    next
end

# Tunnel to Hub 2
config vpn ipsec phase1-interface
    edit "vpn-hub2"
        set interface "wan1"
        set remote-gw 203.0.113.2
        set ike-version 2
        set auto-discovery-receiver enable
        set auto-discovery-shortcuts independent
        set psksecret "secret2"
        set net-device enable
        set add-route disable
    next
end
```

### SD-WAN Members

```bash
config system sdwan
    config members
        edit 10
            set interface "vpn-hub1"
            set zone "overlay"
            set priority 1
        next
        edit 11
            set interface "vpn-hub2"
            set zone "overlay"
            set priority 1
        next
    end
end
```

### BGP to Both Hubs

```bash
config router bgp
    set as 65000
    config neighbor
        edit "10.10.10.1"
            set remote-as 65000
            set interface "vpn-hub1"
        next
        edit "10.10.20.1"
            set remote-as 65000
            set interface "vpn-hub2"
        next
    end
end
```

## Hub-to-Hub Backbone

Connect hubs directly for inter-regional traffic:

```bash
# On Hub 1
config vpn ipsec phase1-interface
    edit "hub-to-hub2"
        set interface "wan1"
        set remote-gw 203.0.113.2
        set ike-version 2
        set psksecret "hub-secret"
    next
end
```

!!! tip "BGP route preference"
    Use BGP local preference or MED to prefer the local hub over the remote hub. This ensures traffic stays regional when possible and only crosses the hub backbone when needed.
