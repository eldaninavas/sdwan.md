---
title: Monitoring & Analytics
description: SD-WAN visibility, application analytics, SLA monitoring, and troubleshooting tools.
tags:
  - monitoring
  - analytics
  - visibility
---

# :material-chart-line: Monitoring & Analytics

Effective SD-WAN monitoring provides visibility into application performance, link health, and security events across the entire WAN fabric.

## Key Metrics to Monitor

| Category | Metrics |
|----------|---------|
| Link Health | Latency, jitter, packet loss, bandwidth utilization |
| Application | Response time, throughput, session count |
| SLA | SLA compliance %, failover events, violations |
| Security | Threat events, blocked connections, IPS alerts |
| Device | CPU, memory, session count, tunnel status |

## Monitoring Architecture

- **Real-time dashboards** -- Live view of link and application health
- **Historical analytics** -- Trend analysis for capacity planning
- **Alerting** -- Threshold-based and anomaly-based notifications
- **Reporting** -- Scheduled reports for management and compliance
- **Log correlation** -- Centralized log aggregation and analysis

## Troubleshooting Tools

- **Packet capture** -- Capture traffic at any point in the overlay or underlay
- **Traceroute** -- Trace the overlay and underlay path separately
- **SLA history** -- View historical SLA metrics for any link
- **Application logs** -- DPI-level application identification and traffic logs
- **Tunnel diagnostics** -- IPsec SA status, rekey events, DPD status

!!! tip "Fortinet analytics"
    FortiAnalyzer provides deep analytics for FortiGate SD-WAN deployments, including application usage, SLA tracking, and security event correlation. See [FortiAnalyzer Monitoring](../implementations/fortinet/fortianalyzer.md).
