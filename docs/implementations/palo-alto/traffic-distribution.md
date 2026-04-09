---
title: "Palo Alto: Traffic Distribution"
description: SD-WAN traffic distribution policies and application steering on Palo Alto platforms.
tags:
  - palo-alto
  - traffic-distribution
  - steering
---

# :material-steering: Traffic Distribution

Traffic distribution controls how SD-WAN steers application traffic across available WAN paths.

## PAN-OS Traffic Distribution

### Traffic Distribution Profiles

```
Objects > SD-WAN > Traffic Distribution Profile
  Name: Critical-Apps
    Method: Best Available Path
    Path Quality Profile: Real-Time
    
  Name: SaaS-Direct
    Method: Best Available Path
    Path Quality Profile: Interactive
    
  Name: General-Traffic
    Method: Top Down Priority
    Link Priority:
      1. ISP1 (Fiber)
      2. ISP2 (Cable)
      3. LTE (Backup)
      
  Name: Load-Balanced
    Method: Weighted Session Distribution
    ISP1 Weight: 70
    ISP2 Weight: 30
```

### Distribution Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| Best Available Path | Select path meeting SLA | Real-time and interactive apps |
| Top Down Priority | Use first available in priority order | Cost-conscious failover |
| Weighted Session Distribution | Distribute by weight ratio | Load balancing |

### SD-WAN Policy Rules

```
Policies > SD-WAN > SD-WAN Policy
  Rule 1:
    Name: VoIP-Steering
    Source: Trust Zone
    Application: sip, rtp, ms-teams-audio
    Traffic Distribution: Critical-Apps
    
  Rule 2:
    Name: Office365-Direct
    Source: Trust Zone
    Application: ms-office365-base, ms-teams
    Traffic Distribution: SaaS-Direct
    
  Rule 3:
    Name: Default
    Source: Trust Zone
    Application: any
    Traffic Distribution: General-Traffic
```

## Prisma SD-WAN Traffic Steering

### Application Policy (API)

```python
app_policy = {
    "name": "VoIP-Steering",
    "app_def_ids": ["<voip-app-def-id>"],
    "network_context_id": "<network-context-id>",
    "paths_allowed": {
        "active_paths": [
            {
                "label": "private-wan",
                "path_type": "direct"
            }
        ],
        "backup_paths": [
            {
                "label": "public-internet",
                "path_type": "vpn"
            }
        ]
    },
    "transfer_type": "real_time",
    "app_transfer_profile_id": "<transfer-profile-id>"
}

sdk.post.tenant_networkpolicylocalprefixes(app_policy)
```

### Transfer Types

Prisma SD-WAN classifies traffic by transfer type:

| Transfer Type | Description | Default Path Behavior |
|---------------|-------------|----------------------|
| `real_time` | VoIP, video conferencing | Best quality path, no retransmission |
| `interactive` | Web apps, APIs | Low latency, moderate tolerance |
| `bulk` | File transfer, backup | High throughput, tolerant of latency |
| `default` | Unclassified traffic | Balanced path selection |

## Verification

=== "PAN-OS"

    ```
    > show sdwan traffic-distribution
    > show sdwan connection all
    > show running resource-monitor second last 5
    ```

=== "Prisma SD-WAN"

    ```python
    # Check policy status via API
    policies = sdk.get.networkpolicyrules()
    for policy in policies.json()['items']:
        print(f"{policy['name']}: {policy['admin_state']}")
    ```

!!! info "App-ID vs AppFabric"
    PAN-OS uses **App-ID** (signature-based DPI) for application identification. Prisma SD-WAN uses **AppFabric** (ML-based flow classification). Both achieve high accuracy, but AppFabric identifies apps faster without deep inspection.
