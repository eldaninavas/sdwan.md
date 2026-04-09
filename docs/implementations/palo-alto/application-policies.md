---
title: "Palo Alto: Application Policies"
description: Application-aware SD-WAN policies and QoS configuration on Palo Alto platforms.
tags:
  - palo-alto
  - application-policies
  - qos
  - app-id
---

# :material-application-cog: Application Policies

Application policies define how specific applications are identified, prioritized, and steered across the SD-WAN fabric.

## PAN-OS Application Policies

### App-ID for SD-WAN

PAN-OS uses its native **App-ID** engine to identify applications for SD-WAN steering:

```
Policies > SD-WAN > SD-WAN Policy
  Rule: Zoom-Priority
    Source Zone: Trust
    Destination Zone: Untrust
    Application: zoom, zoom-meeting, zoom-phone
    Service: application-default
    Traffic Distribution: Real-Time-Profile
    QoS Profile: EF-Marking
```

### QoS for SD-WAN Traffic

```
Objects > QoS > QoS Profile
  Name: SD-WAN-QoS
  Egress Max: 100 Mbps
  Classes:
    Class 1 (Real-Time):
      Priority: real-time
      Guaranteed: 30%
      Max: 50%
      DSCP: EF
    Class 2 (Business-Critical):
      Priority: high
      Guaranteed: 30%
      Max: 50%
      DSCP: AF41
    Class 3 (Default):
      Priority: medium
      Guaranteed: 20%
      Max: 100%
      DSCP: CS0
    Class 4 (Scavenger):
      Priority: low
      Guaranteed: 5%
      Max: 20%
      DSCP: CS1
```

### Applying QoS to SD-WAN Interfaces

```
Network > QoS > QoS Policy
  Rule: WAN-QoS
    Source Interface: ethernet1/3 (LAN)
    Destination Interface: ethernet1/1 (WAN1)
    QoS Profile: SD-WAN-QoS
    Application:
      zoom, ms-teams → Class 1
      oracle, sap → Class 2
      web-browsing → Class 3
      youtube, netflix → Class 4
```

## Prisma SD-WAN Application Policies

### Application Definitions (API)

```python
# Create custom application definition
app_def = {
    "app_def_name": "Custom-ERP",
    "abbreviation": "ERP",
    "app_type": "custom",
    "tcp_rules": [
        {
            "server_ip_prefix": "10.100.0.0/16",
            "server_port": "443",
            "client_ip_prefix": "any"
        }
    ],
    "transfer_type": "interactive",
    "category": "business",
    "ingress_traffic_pct": 30,
    "path_affinity": "strict"
}

sdk.post.appdefs(app_def)
```

### Global Policy Set (API)

```python
# Define application policy in global policy set
policy_rule = {
    "name": "ERP-High-Priority",
    "description": "Ensure ERP gets best path",
    "app_def_ids": ["<erp-app-def-id>"],
    "network_context_id": None,
    "paths_allowed": {
        "active_paths": [
            {"label": "private-wan", "path_type": "direct"},
            {"label": "public-internet", "path_type": "vpn"}
        ]
    },
    "transfer_type": "interactive",
    "priority_num": 100
}

sdk.post.networkpolicyrules(policy_set_id, policy_rule)
```

### Built-in Application Categories

Prisma SD-WAN includes pre-defined application categories:

| Category | Examples | Default Transfer Type |
|----------|----------|----------------------|
| Unified Communications | Zoom, Teams, WebEx | Real-Time |
| Business SaaS | Salesforce, Workday | Interactive |
| Cloud Infrastructure | AWS, Azure, GCP | Interactive |
| Collaboration | Slack, Google Workspace | Interactive |
| Social Media | Facebook, Twitter | Bulk |
| Streaming | YouTube, Netflix | Bulk |

!!! tip "App-ID updates"
    PAN-OS App-ID signatures are updated weekly via content updates. Ensure your firewalls have active Threat Prevention licenses and auto-update enabled to identify the latest applications.
