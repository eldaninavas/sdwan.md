---
title: Manufacturing & IoT
description: SD-WAN for manufacturing — OT/IT convergence, IoT security, and segmentation.
tags:
  - manufacturing
  - iot
  - ot
  - use-case
---

# :material-factory: Manufacturing & IoT

Manufacturing environments use SD-WAN to securely connect factories, warehouses, and distribution centers while maintaining strict segmentation between OT (Operational Technology) and IT networks.

## OT/IT Convergence Challenges

- **Legacy protocols** -- Many OT devices use non-IP protocols
- **Security** -- OT devices often lack built-in security
- **Segmentation** -- OT must be isolated from IT and internet
- **Reliability** -- Production downtime is extremely costly
- **Visibility** -- Need to monitor OT traffic without disrupting operations

## SD-WAN Segmentation for Manufacturing

| Segment | Traffic | Policy |
|---------|---------|--------|
| OT / SCADA | PLC, HMI, SCADA | Air-gapped or strict firewall to DC |
| IT Corporate | Email, ERP, web | Standard SD-WAN policies |
| IoT Sensors | Telemetry, monitoring | Cloud-only (IoT platform) |
| Guest / Vendor | Contractor access | Isolated internet breakout |

## Benefits

- **Micro-segmentation** prevents lateral movement between OT and IT
- **Centralized visibility** into all factory connections
- **Rapid site deployment** for new facilities
- **Multi-transport** support for diverse factory locations
