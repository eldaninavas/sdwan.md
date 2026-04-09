---
title: "Fortinet: IPsec Overlays"
description: IPsec overlay tunnel configuration for FortiGate SD-WAN — design patterns and best practices.
tags:
  - fortinet
  - ipsec
  - overlay
  - tunnels
---

# :material-lock: IPsec Overlays

IPsec overlay tunnels form the backbone of FortiGate SD-WAN. This page covers tunnel design patterns and configuration.

## Overlay Design Patterns

### Single Overlay per WAN Link

Each WAN link gets its own overlay tunnel to each hub:

```
Spoke wan1 --> IPsec Tunnel 1 --> Hub wan1
Spoke wan2 --> IPsec Tunnel 2 --> Hub wan1
Spoke wan1 --> IPsec Tunnel 3 --> Hub wan2
Spoke wan2 --> IPsec Tunnel 4 --> Hub wan2
```

This provides maximum path diversity and granular SD-WAN control.

### Configuration Example (Spoke with 2 WAN, 2 Hubs)

```bash
# Tunnel 1: wan1 -> Hub1
config vpn ipsec phase1-interface
    edit "H1-wan1"
        set interface "wan1"
        set remote-gw 203.0.113.1
        set ike-version 2
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set auto-discovery-receiver enable
        set net-device enable
        set add-route disable
        set psksecret "secret"
    next
end

# Tunnel 2: wan2 -> Hub1
config vpn ipsec phase1-interface
    edit "H1-wan2"
        set interface "wan2"
        set remote-gw 203.0.113.1
        set ike-version 2
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set auto-discovery-receiver enable
        set net-device enable
        set add-route disable
        set psksecret "secret"
    next
end

# Tunnel 3: wan1 -> Hub2
config vpn ipsec phase1-interface
    edit "H2-wan1"
        set interface "wan1"
        set remote-gw 198.51.100.1
        set ike-version 2
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set auto-discovery-receiver enable
        set net-device enable
        set add-route disable
        set psksecret "secret"
    next
end

# Tunnel 4: wan2 -> Hub2
config vpn ipsec phase1-interface
    edit "H2-wan2"
        set interface "wan2"
        set remote-gw 198.51.100.1
        set ike-version 2
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set auto-discovery-receiver enable
        set net-device enable
        set add-route disable
        set psksecret "secret"
    next
end
```

### SD-WAN Members

```bash
config system sdwan
    config members
        edit 1
            set interface "H1-wan1"
            set zone "overlay"
        next
        edit 2
            set interface "H1-wan2"
            set zone "overlay"
        next
        edit 3
            set interface "H2-wan1"
            set zone "overlay"
        next
        edit 4
            set interface "H2-wan2"
            set zone "overlay"
        next
    end
end
```

## Troubleshooting Tunnels

```bash
# Check tunnel status
diagnose vpn tunnel list

# Check IKE SA
diagnose vpn ike gateway list

# Debug IKE negotiation
diagnose debug application ike -1
diagnose debug enable
```

!!! tip "Use `net-device enable`"
    Always set `net-device enable` for SD-WAN overlay tunnels. This creates a proper interface that can be added as an SD-WAN member and supports ADVPN shortcuts.
