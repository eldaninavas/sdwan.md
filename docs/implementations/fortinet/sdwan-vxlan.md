---
title: "Fortinet: SD-WAN with VXLAN"
description: Integrating VXLAN with FortiGate SD-WAN for L2 extension over L3 overlay.
tags:
  - fortinet
  - vxlan
  - overlay
---

# :material-layers-triple: SD-WAN with VXLAN

VXLAN (Virtual Extensible LAN) can be combined with SD-WAN to extend Layer 2 domains across the WAN overlay.

## Use Cases

- **L2 extension** between data centers
- **VM mobility** across sites
- **Legacy applications** requiring L2 adjacency
- **Multi-tenant** segmentation with VXLAN VNIs

## Configuration

### VXLAN Interface over IPsec

```bash
config system vxlan
    edit "vxlan-dc"
        set vni 1000
        set remote-ip "10.10.10.1"
        set interface "vpn-to-hub"
    next
end

config system interface
    edit "vxlan-dc"
        set ip 172.16.100.2/24
        set allowaccess ping
    next
end
```

## Considerations

- VXLAN adds 50 bytes of overhead on top of IPsec
- Ensure MTU is adjusted accordingly (recommend 9000 on underlay if possible)
- Use VXLAN sparingly -- L3 routing is preferred for SD-WAN scalability

!!! warning "MTU overhead"
    IPsec + VXLAN encapsulation can exceed 100 bytes. If your underlay doesn't support jumbo frames, you'll need to reduce the inner MTU or enable fragmentation, which impacts performance.
