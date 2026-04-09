---
title: "Palo Alto: Troubleshooting"
description: SD-WAN troubleshooting for PAN-OS and Prisma SD-WAN — diagnostic commands and common issues.
tags:
  - palo-alto
  - troubleshooting
  - diagnostics
---

# :material-wrench: Troubleshooting

Essential diagnostic commands and common issues for Palo Alto SD-WAN troubleshooting.

## PAN-OS SD-WAN Diagnostics

### SD-WAN Status

```
# Overall SD-WAN status
> show sdwan connection all

# SD-WAN tunnel status
> show sdwan tunnel all

# Path quality metrics
> show sdwan path-monitor stats

# Traffic distribution status
> show sdwan traffic-distribution

# SD-WAN session table
> show sdwan session all
```

### VPN/IPsec Diagnostics

```
# IPsec SA status
> show vpn ipsec-sa

# IKE SA status
> show vpn ike-sa

# Tunnel interface status
> show interface tunnel

# Debug IKE
> debug ike global on debug
> less mp-log ikemgr.log
```

### Routing Diagnostics

```
# Routing table
> show routing route

# BGP summary
> show routing protocol bgp summary

# BGP neighbor detail
> show routing protocol bgp peer

# Route lookup
> test routing fib-lookup virtual-router default ip 10.0.0.1
```

### Traffic Flow Diagnostics

```
# Packet capture
> debug dataplane packet-diag set capture stage receive
> debug dataplane packet-diag set filter match source 192.168.1.100
> debug dataplane packet-diag set capture on

# Session table
> show session all filter source 192.168.1.100

# Flow debug
> debug dataplane packet-diag set log-feature flow basic
> debug dataplane packet-diag set log-feature flow all
```

## Prisma SD-WAN Diagnostics

### ION Device Status (API)

```python
# Check element (ION) status
elements = sdk.get.elements()
for elem in elements.json()['items']:
    print(f"{elem['name']}: {elem['connected']} | SW: {elem['software_version']}")

# Check interface status
interfaces = sdk.get.interfaces(site_id, element_id)
for iface in interfaces.json()['items']:
    print(f"{iface['name']}: {iface['admin_state']} | {iface['operational_state']}")

# Check tunnel status
tunnels = sdk.get.vpnlinks()
for tunnel in tunnels.json()['items']:
    print(f"{tunnel['name']}: {tunnel['status']}")
```

### ION CLI (Direct Access)

```bash
# SSH to ION device
ssh admin@<ion-ip>

# Show system status
show system status

# Show interfaces
show interface summary

# Show tunnels
show vpn status

# Show routing table
show route

# Show application flows
show flow summary

# Debug path selection
debug sdwan path-selection
```

## Common Issues

### SD-WAN Tunnel Won't Establish (PAN-OS)

1. Verify WAN interface has connectivity: `ping source <wan-ip> host <remote-gw>`
2. Check IKE negotiation: `show vpn ike-sa` and review `ikemgr.log`
3. Ensure SD-WAN plugin version matches on Panorama and firewall
4. Verify SD-WAN license is active: `show system info | match sdwan`

### Traffic Not Being Steered (PAN-OS)

1. Verify SD-WAN policy matches traffic: `show sdwan traffic-distribution`
2. Check App-ID is identifying the application: `show session id <id>`
3. Confirm path quality profile thresholds: `show sdwan path-monitor stats`
4. Review security policy -- traffic may be blocked before reaching SD-WAN

### ION Device Not Connecting to Controller (Prisma)

1. Verify WAN connectivity and DNS resolution
2. Check device is claimed in the portal (Inventory > Devices)
3. Ensure NTP is synchronized
4. Review controller reachability: `show system controller-status`

### Poor Application Performance

1. Check path quality: `show sdwan path-monitor stats` (PAN-OS) or ADEM dashboard (Prisma)
2. Verify QoS is applied correctly
3. Check for MTU issues (especially with IPsec overhead)
4. Look for asymmetric routing on the underlay

!!! tip "Start with path quality"
    Like Fortinet, 90% of SD-WAN issues trace back to path quality. Check link health first, then verify policy matching, and finally review routing.
