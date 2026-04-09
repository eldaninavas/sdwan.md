---
title: "Palo Alto: Panorama Management"
description: Centralized PAN-OS SD-WAN management with Panorama — templates, device groups, and SD-WAN plugin.
tags:
  - palo-alto
  - panorama
  - management
  - centralized
---

# :material-monitor-dashboard: Panorama Management

Panorama is the centralized management platform for PAN-OS SD-WAN, providing template-based configuration, monitoring, and the SD-WAN plugin.

## SD-WAN Plugin Architecture

```
Panorama
├── SD-WAN Plugin (3.x)
│   ├── VPN Clusters
│   ├── Traffic Distribution
│   ├── Path Quality Profiles
│   └── SD-WAN Monitoring
├── Device Groups
│   ├── Hub-DG (hub firewalls)
│   └── Branch-DG (branch firewalls)
└── Template Stacks
    ├── Hub-TS (hub templates)
    └── Branch-TS (branch templates)
```

## Template Strategy

### Template Variables

Use variables for site-specific configuration:

```
Template Variable: $WAN1_IP
  Type: IP Address
  Description: WAN1 interface IP address
  
Template Variable: $WAN1_GW
  Type: IP Address
  Description: WAN1 default gateway
  
Template Variable: $LAN_SUBNET
  Type: IP Prefix
  Description: LAN subnet for the site

Template Variable: $SITE_NAME
  Type: String
  Description: Site identifier
```

### Template Stack Design

```
Hub Template Stack:
  ├── Hub-Base (interfaces, zones, routing)
  ├── SD-WAN-Hub (SD-WAN specific settings)
  └── Security-Base (security profiles)

Branch Template Stack:
  ├── Branch-Base (interfaces, zones)
  ├── SD-WAN-Branch (SD-WAN settings)
  └── Security-Base (shared security profiles)
```

## Device Group Strategy

```
Device Groups:
  Shared (top-level policies)
  ├── Hub-DG
  │   ├── Hub-DC1
  │   └── Hub-DC2
  └── Branch-DG
      ├── Branch-Small (PA-440)
      ├── Branch-Medium (PA-450)
      └── Branch-Large (PA-460)
```

## SD-WAN Monitoring in Panorama

The SD-WAN plugin provides:

- **Topology view** -- Visual map of all SD-WAN sites and tunnels
- **Path quality dashboard** -- Real-time latency, jitter, loss per link
- **Application bandwidth** -- Top applications per site
- **Tunnel status** -- Up/down status for all VPN tunnels
- **SLA compliance** -- Historical SLA adherence

## API Automation

```python
import pan.xapi

# Connect to Panorama
xapi = pan.xapi.PanXapi(
    hostname='panorama.example.com',
    api_key='your-api-key'
)

# Push SD-WAN template to devices
xapi.commit(
    cmd='<commit-all><shared-policy><device-group><entry name="Branch-DG"/></device-group></shared-policy></commit-all>'
)
```

!!! tip "Commit strategy"
    Always use **Commit and Push** (not just Commit) to deploy changes to managed firewalls. Use commit scheduling for maintenance windows and staged rollouts.
