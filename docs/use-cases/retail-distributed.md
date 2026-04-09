---
title: Retail & Distributed Sites
description: SD-WAN for retail chains and distributed locations — PCI compliance, segmentation, and rapid deployment.
tags:
  - retail
  - pci
  - distributed
  - use-case
---

# :material-store: Retail & Distributed Sites

Retail organizations with hundreds or thousands of stores benefit enormously from SD-WAN's centralized management, security, and cost savings.

## Key Requirements

- **PCI DSS compliance** for payment processing
- **Network segmentation** (POS, corporate, guest WiFi)
- **Rapid deployment** for new store openings
- **Low IT overhead** at remote locations
- **Reliable connectivity** for POS and inventory systems

## Segmentation Design

SD-WAN enables multi-VRF segmentation at each store:

| Segment | VLAN | VRF | Policy |
|---------|------|-----|--------|
| POS/Payment | 10 | PCI | Encrypted tunnel to DC only |
| Corporate | 20 | Corp | Full SD-WAN with SLA |
| Guest WiFi | 30 | Guest | Local internet breakout |
| IoT/Digital Signage | 40 | IoT | Cloud-only access |

## Benefits for Retail

- **60%+ cost savings** vs MPLS per store
- **Same-day deployment** with ZTP
- **Consistent security** across all locations
- **PCI compliance** built into the network design
