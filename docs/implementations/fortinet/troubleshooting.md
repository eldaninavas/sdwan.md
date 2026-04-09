---
title: "Fortinet: Troubleshooting"
description: FortiGate SD-WAN troubleshooting guide — diagnostic commands, common issues, and resolution steps.
tags:
  - fortinet
  - troubleshooting
  - diagnostics
---

# :material-wrench: Troubleshooting

Essential diagnostic commands and common issues for FortiGate SD-WAN troubleshooting.

## Diagnostic Commands

### SD-WAN Status

```bash
# SD-WAN member status
diagnose sys sdwan member

# Health check results (most important command)
diagnose sys sdwan health-check

# SD-WAN service/rule matching
diagnose sys sdwan service

# SD-WAN internet-service match
diagnose sys sdwan internet-service-app-ctrl-list
```

### IPsec Tunnels

```bash
# List all tunnels
diagnose vpn tunnel list

# IKE gateway status
diagnose vpn ike gateway list

# Debug IKE negotiation
diagnose debug application ike -1
diagnose debug enable

# Clear IKE SA (force renegotiation)
diagnose vpn ike gateway clear name <tunnel-name>
```

### Routing

```bash
# Full routing table
get router info routing-table all

# BGP summary
get router info bgp summary

# BGP neighbor details
get router info bgp neighbor <ip>

# Route lookup
diagnose firewall proute list
```

### Traffic Flow

```bash
# Sniffer (packet capture)
diagnose sniffer packet any 'host 10.0.0.1' 4 0 l

# Session table
diagnose sys session filter dst 10.0.0.1
diagnose sys session list

# Debug flow (trace packet through FortiGate)
diagnose debug flow filter addr 10.0.0.1
diagnose debug flow trace start 100
diagnose debug enable
```

## Common Issues

### Tunnel Won't Establish

1. Check WAN connectivity: `execute ping <remote-gw>`
2. Verify matching Phase 1 settings (encryption, DH group, PSK)
3. Check NAT traversal settings if behind NAT
4. Review IKE debug: `diagnose debug application ike -1`

### SD-WAN Not Steering Traffic

1. Verify health checks are passing: `diagnose sys sdwan health-check`
2. Check rule matching: `diagnose sys sdwan service`
3. Confirm firewall policy allows traffic through SD-WAN zone
4. Verify the application is correctly identified by ISDB

### High Latency on Overlay

1. Check underlay latency: `execute ping-options source <wan-ip>` then `execute ping <remote-gw>`
2. Verify MTU: `diagnose sniffer packet <interface> 'icmp' 4 1`
3. Check for packet loss on underlay
4. Review QoS/shaping configuration

### BGP Routes Not Propagating

1. Check BGP neighbor state: `get router info bgp summary`
2. Verify route advertisements: `get router info bgp neighbor <ip> advertised-routes`
3. Check route maps and filters
4. Verify tunnel interface is up and has IP

!!! tip "Always start with health checks"
    90% of SD-WAN issues can be diagnosed by checking `diagnose sys sdwan health-check`. If health checks show a link as down or degraded, start troubleshooting at the underlay level.
