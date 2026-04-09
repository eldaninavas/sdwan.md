---
title: "Fortinet: SD-WAN Basics"
description: How to enable and configure SD-WAN on FortiGate — step by step from scratch.
tags:
  - fortinet
  - fortigate
  - sdwan-basics
  - configuration
---

# :material-play-circle: Fortinet SD-WAN Basics

This page covers how to enable SD-WAN on a FortiGate and configure the fundamental building blocks.

## Prerequisites

Before configuring SD-WAN:

- FortiGate with FortiOS 7.0+ (7.4 recommended)
- At least two WAN interfaces (e.g., `wan1`, `wan2`)
- WAN interfaces must **not** be referenced in existing firewall policies or static routes (SD-WAN takes over)
- Default gateway should be removed from individual WAN interfaces

## Enable SD-WAN

### Step 1: Create the SD-WAN Zone

SD-WAN is configured under the `system sdwan` hierarchy:

```bash
config system sdwan
    set status enable
end
```

### Step 2: Add SD-WAN Members

Add your WAN interfaces as SD-WAN members:

```bash
config system sdwan
    config members
        edit 1
            set interface "wan1"
            set gateway 203.0.113.1
            set cost 0
        next
        edit 2
            set interface "wan2"
            set gateway 198.51.100.1
            set cost 0
        next
    end
end
```

### Step 3: Configure Health Checks

Health checks monitor the quality of each WAN link:

```bash
config system sdwan
    config health-check
        edit "Internet"
            set server "8.8.8.8" "1.1.1.1"
            set protocol ping
            set interval 500
            set failtime 3
            set recoverytime 3
            set members 1 2
        next
    end
end
```

### Step 4: Create SD-WAN Rules

SD-WAN rules define how traffic is steered:

```bash
config system sdwan
    config service
        edit 1
            set name "VoIP-Priority"
            set mode sla
            set dst "all"
            set src "all"
            config sla
                edit "Internet"
                    set id 1
                next
            end
            set priority-members 1
        next
    end
end
```

### Step 5: Create Firewall Policies

Reference the SD-WAN zone in firewall policies:

```bash
config firewall policy
    edit 1
        set name "LAN-to-WAN"
        set srcintf "lan"
        set dstintf "virtual-wan-link"
        set srcaddr "all"
        set dstaddr "all"
        set action accept
        set schedule "always"
        set service "ALL"
        set nat enable
    next
end
```

### Step 6: Default Route via SD-WAN

```bash
config router static
    edit 1
        set dst 0.0.0.0/0
        set sdwan-zone "virtual-wan-link"
    next
end
```

## Verification

Check SD-WAN status:

```bash
# View SD-WAN members
diagnose sys sdwan member

# View health check results
diagnose sys sdwan health-check

# View SD-WAN service rules
diagnose sys sdwan service

# View SD-WAN routing
get router info routing-table all
```

!!! warning "Interface removal"
    Before adding a WAN interface to SD-WAN, you must remove it from all existing firewall policies, static routes, and any other references. FortiOS will reject the configuration otherwise.

!!! tip "Next steps"
    - [SD-WAN Zones](sdwan-zones.md) -- Learn about zone-based configuration
    - [Health Check & SLA](health-check-sla.md) -- Deep dive into health monitoring
    - [SD-WAN Rules](sdwan-rules.md) -- Advanced rule configuration
