---
title: "Fortinet: Performance SLA"
description: Monitor and enforce performance SLAs on FortiGate SD-WAN with link quality metrics.
tags:
  - fortinet
  - performance-sla
  - monitoring
---

# :material-gauge: Performance SLA

Performance SLA monitoring in FortiGate SD-WAN continuously tracks link quality and enforces minimum performance standards for each application.

## SLA Target Configuration

```bash
config system sdwan
    config health-check
        edit "Performance-SLA"
            set server "10.0.0.1"
            set protocol ping
            set interval 500
            set failtime 3
            set recoverytime 3
            config sla
                edit 1
                    set latency-threshold 50
                    set jitter-threshold 10
                    set packetloss-threshold 0
                    set link-cost-factor latency jitter packet-loss
                next
            end
        next
    end
end
```

## Link Cost Factor

The `link-cost-factor` determines how the "best" link is calculated:

| Factor | Description |
|--------|-------------|
| `latency` | Lowest latency wins |
| `jitter` | Lowest jitter wins |
| `packet-loss` | Lowest packet loss wins |
| `inbandwidth` | Highest inbound bandwidth wins |
| `outbandwidth` | Highest outbound bandwidth wins |
| `bibandwidth` | Highest bidirectional bandwidth wins |
| `custom-profile-1` | Custom weighted formula |

## Monitoring Commands

```bash
# View real-time SLA status per member
diagnose sys sdwan health-check

# View SLA violation history
execute sdwan health-check-log <name>

# View current link quality metrics
diagnose sys sdwan member

# Performance SLA summary
diagnose sys sdwan service
```

!!! info "SLA IDs in rules"
    When you reference an SLA target in an SD-WAN rule, you use the SLA `id` within the health check. A single health check can have multiple SLA targets (e.g., ID 1 for real-time, ID 2 for interactive, ID 3 for bulk).
