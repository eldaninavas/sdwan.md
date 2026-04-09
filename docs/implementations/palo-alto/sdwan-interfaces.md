---
title: "Palo Alto: SD-WAN Interfaces"
description: Configure WAN interfaces for SD-WAN on both Prisma SD-WAN and PAN-OS platforms.
tags:
  - palo-alto
  - interfaces
  - wan
---

# :material-ethernet: SD-WAN Interfaces

WAN interface configuration is the first step in any SD-WAN deployment. Both Palo Alto platforms handle interfaces differently.

## PAN-OS SD-WAN Interfaces

### SD-WAN Interface Profile

Create a profile that defines the WAN link characteristics:

```
Objects > SD-WAN > SD-WAN Interface Profile
  Name: Fiber-ISP1
  Link Type: Ethernet
  Max Download (Mbps): 500
  Max Upload (Mbps): 500
  ISP Name: AT&T Fiber
  Path Monitor:
    Enabled: Yes
    Monitor Destinations: 8.8.8.8, 1.1.1.1
    Interval: 3 seconds
    Failure Condition: 3 failures
    Recovery Condition: 5 successes
```

### Assign Profile to Interface

```
Network > Interfaces > ethernet1/1
  Type: Layer 3
  Zone: WAN
  IPv4:
    Type: DHCP Client (or Static)
  SD-WAN Interface Profile: Fiber-ISP1
```

### Multiple WAN Links

```
ethernet1/1 → Fiber-ISP1 (500/500 Mbps)
ethernet1/2 → Cable-ISP2 (200/20 Mbps)
ethernet1/3 → LTE-Backup (50/10 Mbps)
```

## Prisma SD-WAN Interfaces

### WAN Interface (API)

```python
# Configure WAN interface
wan1 = {
    "name": "WAN 1",
    "type": "wan",
    "admin_state": "enabled",
    "mtu": 1500,
    "addresses": [{"address_type": "dhcp"}],
    "label_id": "<wan-label-id>",
    "bw_config": {
        "uplink_bw": 500,
        "downlink_bw": 500
    },
    "link_config": {
        "media_type": "Ethernet",
        "speed": "auto"
    },
    "cost": 0
}

sdk.post.interfaces(site_id, element_id, wan1)
```

### WAN Labels

Prisma SD-WAN uses **labels** to classify WAN links:

| Label | Description |
|-------|-------------|
| `public-internet` | Standard broadband / fiber |
| `private-wan` | MPLS or private circuits |
| `lte-4g` | Cellular LTE backup |
| `lte-5g` | 5G cellular |

### LAN Interface (API)

```python
lan1 = {
    "name": "LAN 1",
    "type": "lan",
    "admin_state": "enabled",
    "mtu": 1500,
    "addresses": [
        {
            "address_type": "static",
            "address": "192.168.1.1/24"
        }
    ],
    "dhcp_server": {
        "enabled": True,
        "start_address": "192.168.1.100",
        "end_address": "192.168.1.200",
        "dns_servers": ["8.8.8.8", "8.8.4.4"]
    }
}

sdk.post.interfaces(site_id, element_id, lan1)
```

## Link Bandwidth Configuration

Accurate bandwidth configuration is critical for SD-WAN traffic steering:

| Parameter | Description | Impact |
|-----------|-------------|--------|
| Upload BW | Maximum upstream bandwidth | QoS shaping, load balancing ratios |
| Download BW | Maximum downstream bandwidth | Capacity planning, spillover thresholds |
| Cost | Relative cost metric | Prefer cheaper links for bulk traffic |

!!! warning "Measure actual bandwidth"
    Always set bandwidth values to the **actual measured throughput**, not the ISP-advertised speed. A "100 Mbps" connection often delivers 85-95 Mbps. Over-provisioning leads to packet drops; under-provisioning wastes capacity.
