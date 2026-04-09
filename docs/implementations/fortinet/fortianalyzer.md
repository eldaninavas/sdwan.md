---
title: "Fortinet: FortiAnalyzer Monitoring"
description: SD-WAN analytics and monitoring with FortiAnalyzer — SLA tracking, application visibility, and reporting.
tags:
  - fortinet
  - fortianalyzer
  - analytics
  - monitoring
---

# :material-chart-line: FortiAnalyzer Monitoring

FortiAnalyzer provides deep analytics and reporting for FortiGate SD-WAN deployments.

## SD-WAN Dashboards

FortiAnalyzer includes pre-built SD-WAN dashboards:

- **SD-WAN SLA Monitor** -- Real-time link quality across all sites
- **Application Usage** -- Top applications by bandwidth and session count
- **Link Utilization** -- Per-interface bandwidth consumption over time
- **SLA Violations** -- Historical SLA breach events
- **Tunnel Status** -- IPsec tunnel up/down history

## Key Metrics

| Metric | Source | Retention |
|--------|--------|-----------|
| Link latency/jitter/loss | SD-WAN health checks | Configurable |
| Application traffic | DPI logs | Configurable |
| Tunnel status | IPsec logs | Configurable |
| Security events | UTM logs | Configurable |
| Bandwidth utilization | Traffic logs | Configurable |

## Custom Reports

Create custom reports for:

- Monthly SLA compliance per site
- Top applications consuming WAN bandwidth
- Link failover frequency and duration
- Security incidents per branch

## Integration

- **SIEM integration** -- Forward logs to Splunk, QRadar, etc.
- **REST API** -- Query analytics data programmatically
- **Email alerts** -- Automated notifications on SLA violations
- **SNMP traps** -- Integration with network monitoring tools

!!! info "FortiAnalyzer sizing"
    Size FortiAnalyzer based on log volume: ~1-2 GB/day per FortiGate in a typical SD-WAN deployment. For 100+ sites, consider FortiAnalyzer in HA mode with expanded storage.
