---
title: "Fortinet: Hub & Spoke (ADVPN)"
description: Configure ADVPN (Auto Discovery VPN) for dynamic spoke-to-spoke tunnels in Fortinet SD-WAN.
tags:
  - fortinet
  - advpn
  - hub-spoke
  - dynamic-tunnels
---

# :material-hub: Hub & Spoke (ADVPN)

ADVPN (Auto Discovery VPN) enables dynamic spoke-to-spoke IPsec tunnels. Instead of routing all traffic through the hub, spokes can establish direct tunnels on demand.

## How ADVPN Works

1. All spokes establish static tunnels to the hub
2. When Spoke A needs to reach Spoke B, the hub signals both spokes
3. Spoke A and Spoke B establish a direct IPsec tunnel
4. Traffic flows directly between spokes
5. The shortcut tunnel is torn down after inactivity

## Hub Configuration

### IPsec Phase 1

```bash
config vpn ipsec phase1-interface
    edit "vpn-hub"
        set type dynamic
        set interface "wan1"
        set ike-version 2
        set peertype any
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set net-device enable
        set add-route disable
        set auto-discovery-sender enable
        set psksecret "your-secret-here"
        set dpd on-idle
        set dpd-retryinterval 10
    next
end
```

### IPsec Phase 2

```bash
config vpn ipsec phase2-interface
    edit "vpn-hub-p2"
        set phase1name "vpn-hub"
        set proposal aes256gcm
        set dhgrp 14
        set auto-negotiate enable
    next
end
```

### Hub Interface Configuration

```bash
config system interface
    edit "vpn-hub"
        set ip 10.10.10.1/32
        set remote-ip 10.10.10.254/24
        set allowaccess ping
    next
end
```

## Spoke Configuration

### IPsec Phase 1

```bash
config vpn ipsec phase1-interface
    edit "vpn-to-hub"
        set interface "wan1"
        set ike-version 2
        set peertype any
        set proposal aes256gcm-prfsha256
        set dhgrp 14
        set net-device enable
        set add-route disable
        set auto-discovery-receiver enable
        set auto-discovery-shortcuts independent
        set remote-gw 203.0.113.1
        set psksecret "your-secret-here"
        set dpd on-idle
        set dpd-retryinterval 10
    next
end
```

### IPsec Phase 2

```bash
config vpn ipsec phase2-interface
    edit "vpn-to-hub-p2"
        set phase1name "vpn-to-hub"
        set proposal aes256gcm
        set dhgrp 14
        set auto-negotiate enable
    next
end
```

## Key ADVPN Settings

| Setting | Hub | Spoke | Description |
|---------|-----|-------|-------------|
| `auto-discovery-sender` | enable | disable | Hub sends shortcut info |
| `auto-discovery-receiver` | disable | enable | Spoke receives shortcuts |
| `auto-discovery-shortcuts` | N/A | independent | How shortcuts are formed |
| `net-device` | enable | enable | Required for ADVPN |
| `add-route` | disable | disable | BGP handles routing |

## Verification

```bash
# View ADVPN shortcuts on spoke
diagnose vpn ike shortcut-list

# View active tunnels
diagnose vpn tunnel list name vpn-to-hub

# View IPsec SA
diagnose vpn ipsec status

# View ADVPN route cache
get router info routing-table all
```

!!! warning "NAT traversal"
    If spokes are behind NAT, ensure `set nattraversal enable` in Phase 1. ADVPN shortcuts between NATed spokes require both spokes to be able to reach each other's public IP via the negotiation facilitated by the hub.
