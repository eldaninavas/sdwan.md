---
title: "Fortinet: SD-WAN Rules"
description: Create application-aware SD-WAN rules on FortiGate for traffic steering based on app identity and SLA.
tags:
  - fortinet
  - rules
  - traffic-steering
  - application-aware
---

# :material-format-list-checks: SD-WAN Rules

SD-WAN rules (also called SD-WAN services) define how specific traffic is steered across WAN members based on application identity, SLA requirements, and routing strategy.

## Rule Modes

FortiGate SD-WAN supports several steering modes:

| Mode | Description | Use Case |
|------|-------------|----------|
| `manual` | Fixed member priority | Simple failover |
| `priority` | Use highest-priority available member | Ordered preference |
| `sla` | Use best member meeting SLA target | Quality-based |
| `lowest-cost-sla` | Cheapest member meeting SLA | Cost optimization |
| `load-balance` | Distribute across members | Bandwidth aggregation |

## Example Rules

### VoIP - SLA Mode

```bash
config system sdwan
    config service
        edit 1
            set name "VoIP"
            set mode sla
            set internet-service enable
            set internet-service-app-ctrl 38941 38943
            config sla
                edit "Internet-HC"
                    set id 1
                next
            end
            set priority-members 1 2
        next
    end
end
```

### Microsoft 365 - Lowest Cost SLA

```bash
config system sdwan
    config service
        edit 2
            set name "Microsoft365"
            set mode lowest-cost-sla
            set internet-service enable
            set internet-service-name "Microsoft-Office365"
            config sla
                edit "Internet-HC"
                    set id 2
                next
            end
            set priority-members 1 2
        next
    end
end
```

### Backup Traffic - Manual with Load Balancing

```bash
config system sdwan
    config service
        edit 3
            set name "Backup-Traffic"
            set mode load-balance
            set dst "backup-server-subnet"
            set src "lan-subnet"
            set protocol 6
            set start-port 1
            set end-port 65535
            set priority-members 2 1
            set load-balance-mode source-dest-ip-based
        next
    end
end
```

### Corporate Apps via Overlay

```bash
config system sdwan
    config service
        edit 4
            set name "Corporate-Apps"
            set mode sla
            set dst "corporate-dc-subnets"
            set src "all"
            config sla
                edit "Overlay-HC"
                    set id 1
                next
            end
            set priority-members 10 11
            set priority-zone "overlay"
        next
    end
end
```

## Load Balancing Modes

| Mode | Description |
|------|-------------|
| `source-ip-based` | Hash on source IP |
| `source-dest-ip-based` | Hash on source + destination IP |
| `weight-based` | Distribute by member weight |
| `usage-based` | Distribute based on current utilization |
| `inbandwidth` | Based on inbound bandwidth |
| `outbandwidth` | Based on outbound bandwidth |
| `bibandwidth` | Based on both directions |

## Internet Service Database (ISDB)

FortiGuard ISDB provides application identification for SD-WAN rules:

```bash
# List available internet services
diagnose internet-service list

# Show ISDB entries for Microsoft 365
diagnose internet-service name Microsoft-Office365

# Show ISDB app control IDs
diagnose internet-service app-ctrl-list
```

## Rule Processing Order

1. Rules are evaluated **top-to-bottom** (lowest edit ID first)
2. First matching rule wins
3. Traffic not matching any rule uses the **implicit rule** (default SD-WAN behavior)
4. Place more specific rules before generic ones

!!! tip "Use ISDB over manual addresses"
    The FortiGuard ISDB is automatically updated with the latest IP ranges for cloud services. Use `internet-service-name` instead of manually maintaining destination address objects for SaaS applications.
