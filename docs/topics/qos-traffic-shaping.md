---
title: QoS & Traffic Shaping
description: Quality of Service and traffic shaping mechanisms in SD-WAN environments.
tags:
  - qos
  - traffic-shaping
  - dscp
---

# :material-speedometer: QoS & Traffic Shaping

Quality of Service (QoS) ensures that critical applications receive priority treatment over the WAN, even during congestion.

## QoS in SD-WAN

SD-WAN QoS operates at multiple levels:

### Application-Level QoS

- Identify applications via DPI
- Assign bandwidth guarantees and limits per application
- Prioritize real-time traffic over bulk transfers

### DSCP Marking

- Mark packets with DSCP values for downstream QoS treatment
- Remark traffic as it enters the overlay
- Preserve or override original markings

### Traffic Shaping

- **Per-interface shaping** -- Limit total bandwidth per WAN link
- **Per-application shaping** -- Guarantee minimum and set maximum bandwidth per app
- **Burst handling** -- Allow temporary bursts above the guaranteed rate

## Common QoS Design

| Priority | DSCP | Applications | Bandwidth |
|----------|------|-------------|-----------|
| Real-Time | EF (46) | VoIP, video | 30% guaranteed |
| Business Critical | AF41 (34) | ERP, CRM | 30% guaranteed |
| Default | CS0 (0) | Web, email | 30% best effort |
| Scavenger | CS1 (8) | Social, streaming | 10% max |

!!! tip "Shape to the WAN link speed"
    Always configure traffic shaping to match your actual WAN link bandwidth, not the interface speed. A 1Gbps Ethernet port connected to a 100Mbps broadband service should be shaped to ~95Mbps to prevent the ISP from dropping packets randomly.
