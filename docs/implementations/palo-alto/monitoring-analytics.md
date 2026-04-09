---
title: "Palo Alto: Monitoring & Analytics"
description: SD-WAN monitoring with ADEM, Panorama dashboards, and Prisma SD-WAN analytics.
tags:
  - palo-alto
  - monitoring
  - adem
  - analytics
---

# :material-chart-line: Monitoring & Analytics

Both Palo Alto SD-WAN platforms provide rich monitoring and analytics capabilities.

## PAN-OS SD-WAN Monitoring (Panorama)

### SD-WAN Dashboard

Panorama's SD-WAN plugin provides:

- **Topology map** -- Visual representation of all sites and tunnels
- **Link quality charts** -- Historical latency, jitter, and packet loss
- **Application bandwidth** -- Top apps by throughput per site
- **Tunnel status** -- Real-time VPN tunnel health
- **Alerts** -- SLA violations and link failures

### CLI Monitoring

```
# SD-WAN connection summary
> show sdwan connection all

# Path quality statistics
> show sdwan path-monitor stats interface ethernet1/1

# Traffic distribution stats
> show sdwan traffic-distribution

# Session distribution across links
> show session all filter state active

# Interface counters
> show interface ethernet1/1
> show interface ethernet1/2
```

### Log Forwarding for Analytics

```
Objects > Log Forwarding Profile
  Name: SD-WAN-Logs
  Log Type: Traffic
    Filter: (zone.src eq 'SD-WAN')
    Forward to: Panorama, SIEM
    
  Log Type: SD-WAN
    Forward to: Panorama
```

## Prisma SD-WAN Analytics

### Autonomous DEM (ADEM)

ADEM provides end-to-end application performance monitoring:

- **Hop-by-hop analysis** -- Identify bottlenecks between user and application
- **Application response time** -- Measure server-side and network-side latency
- **ISP performance** -- Compare ISP quality over time
- **Automatic root cause** -- ML identifies the likely cause of degradation

### Analytics API

```python
# Query application performance metrics
metrics = sdk.get.metrics_monitor(
    site_id,
    params={
        "start_time": "2026-04-01T00:00:00Z",
        "end_time": "2026-04-10T00:00:00Z",
        "metric": "application_health",
        "granularity": "hourly"
    }
)

for datapoint in metrics.json()['items']:
    print(f"{datapoint['timestamp']}: health={datapoint['value']}")
```

### Key Metrics

| Metric | Platform | Description |
|--------|----------|-------------|
| Path latency | Both | Round-trip time per WAN link |
| Path jitter | Both | Latency variation |
| Packet loss | Both | Loss percentage per link |
| App response time | Prisma SD-WAN | End-to-end application latency |
| ISP quality score | Prisma SD-WAN | ML-derived ISP rating |
| Tunnel throughput | Both | Bandwidth per VPN tunnel |
| Session count | PAN-OS | Active sessions per link |

## Integration with External Tools

| Tool | PAN-OS | Prisma SD-WAN |
|------|--------|---------------|
| Splunk | Syslog + Cortex Data Lake | API integration |
| ServiceNow | Panorama plugin | REST API webhooks |
| PagerDuty | Syslog alerting | Webhook notifications |
| Grafana | SNMP + API | API data source |

!!! tip "Cortex Data Lake"
    For unified logging across PAN-OS SD-WAN and Prisma SD-WAN, forward all logs to **Cortex Data Lake**. This provides a single analytics platform for security and SD-WAN events.
