---
title: "Fortinet: Health Check & SLA"
description: Configure SD-WAN health checks and SLA monitoring on FortiGate for link quality monitoring.
tags:
  - fortinet
  - health-check
  - sla
  - monitoring
---

# :material-heart-pulse: Health Check & SLA

Health checks are the foundation of SD-WAN path selection. They continuously monitor the quality of each WAN link and trigger failover when SLA thresholds are violated.

## Health Check Configuration

### Basic Ping Health Check

```bash
config system sdwan
    config health-check
        edit "Internet-HC"
            set server "8.8.8.8" "1.1.1.1"
            set protocol ping
            set interval 500
            set failtime 3
            set recoverytime 3
            set members 1 2 3
            config sla
                edit 1
                    set latency-threshold 100
                    set jitter-threshold 30
                    set packetloss-threshold 1
                next
            end
        next
    end
end
```

### HTTP Health Check

```bash
config system sdwan
    config health-check
        edit "WebApp-HC"
            set server "app.example.com"
            set protocol http
            set port 443
            set security-mode tls
            set interval 1000
            set failtime 3
            set recoverytime 3
            set members 1 2
            config sla
                edit 1
                    set latency-threshold 200
                    set jitter-threshold 50
                    set packetloss-threshold 2
                next
            end
        next
    end
end
```

### DNS Health Check

```bash
config system sdwan
    config health-check
        edit "DNS-HC"
            set server "8.8.8.8"
            set protocol dns
            set interval 500
            set failtime 3
            set recoverytime 3
            set members 1 2
        next
    end
end
```

## Health Check Parameters

| Parameter | Description | Recommended |
|-----------|-------------|-------------|
| `interval` | Probe interval in ms | 500-1000ms |
| `failtime` | Consecutive failures before marking down | 3-5 |
| `recoverytime` | Consecutive successes before marking up | 3-5 |
| `threshold-warning-latency` | Warning threshold | 100ms |
| `threshold-alert-latency` | Alert threshold | 200ms |
| `update-cascade-interface` | Update routing on SLA fail | `enable` |
| `update-static-route` | Remove static routes on SLA fail | `enable` |

## SLA Targets

Define multiple SLA targets for different application tiers:

```bash
config system sdwan
    config health-check
        edit "Multi-SLA"
            set server "8.8.8.8"
            set protocol ping
            set interval 500
            set members 1 2
            config sla
                edit 1
                    set latency-threshold 50
                    set jitter-threshold 10
                    set packetloss-threshold 0
                    set link-cost-factor latency jitter packet-loss
                next
                edit 2
                    set latency-threshold 150
                    set jitter-threshold 30
                    set packetloss-threshold 1
                next
                edit 3
                    set latency-threshold 300
                    set jitter-threshold 50
                    set packetloss-threshold 5
                next
            end
        next
    end
end
```

## Monitoring Health Checks

```bash
# Real-time health check status
diagnose sys sdwan health-check

# Detailed health check history
diagnose sys sdwan health-check-history <name>

# SLA log
diagnose sys sdwan sla-log <health-check-name>
```

!!! warning "Don't probe too aggressively"
    Setting the interval below 500ms on LTE or satellite links can generate significant overhead and even trigger rate limiting from the target. Use 1000ms+ for metered connections.
