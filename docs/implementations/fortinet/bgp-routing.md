---
title: "Fortinet: BGP & Routing"
description: BGP configuration with FortiGate SD-WAN — overlay routing, route advertisement, and community-based steering.
tags:
  - fortinet
  - bgp
  - routing
  - overlay
---

# :material-routes: BGP & Routing

BGP is the primary routing protocol used in FortiGate SD-WAN deployments, especially for hub-and-spoke topologies with ADVPN.

## BGP over SD-WAN Overlay

### Hub Configuration

```bash
config router bgp
    set as 65000
    set router-id 10.10.10.1
    config neighbor-group
        edit "ADVPN-Spokes"
            set remote-as 65000
            set route-reflector-client enable
            set next-hop-self enable
            set soft-reconfiguration enable
            set interface "vpn-hub"
        next
    end
    config neighbor-range
        edit 1
            set prefix 10.10.10.0/24
            set neighbor-group "ADVPN-Spokes"
        next
    end
    config network
        edit 1
            set prefix 10.0.0.0/16
        next
    end
end
```

### Spoke Configuration

```bash
config router bgp
    set as 65000
    set router-id 10.10.10.100
    config neighbor
        edit "10.10.10.1"
            set remote-as 65000
            set next-hop-self enable
            set soft-reconfiguration enable
            set interface "vpn-to-hub"
            set connect-timer 1
        next
    end
    config network
        edit 1
            set prefix 192.168.100.0/24
        next
    end
end
```

## Route Tags and Communities

Use BGP communities to influence SD-WAN traffic steering:

```bash
config router route-map
    edit "TAG-PRIMARY"
        config rule
            edit 1
                set set-community "65000:100"
            next
        end
    next
end
```

## Route Redistribution

```bash
config router bgp
    config redistribute connected
        set status enable
        set route-map "REDISTRIBUTE-CONNECTED"
    end
    config redistribute static
        set status enable
    end
end
```

!!! tip "iBGP for SD-WAN"
    Use iBGP (same AS) for hub-and-spoke SD-WAN. The hub acts as a route reflector, redistributing spoke routes to all other spokes. This is simpler than eBGP and avoids AS-path issues.
