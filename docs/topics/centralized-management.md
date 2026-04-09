---
title: Centralized Management
description: Single pane of glass management for SD-WAN — orchestrators, templates, and policy deployment.
tags:
  - management
  - orchestrator
  - automation
---

# :material-monitor-dashboard: Centralized Management

Centralized management is a core pillar of SD-WAN, replacing the per-device CLI configuration model of traditional WANs with a single orchestration platform.

## Key Capabilities

- **Template-based configuration** -- Define once, deploy to hundreds of sites
- **Policy management** -- Centralized application and security policies
- **Firmware management** -- Staged upgrades across the entire fabric
- **Monitoring & alerting** -- Real-time health dashboards and SLA tracking
- **Role-based access control** -- Granular permissions for different admin roles
- **API-driven automation** -- REST APIs for integration with CI/CD, ITSM, and SIEM

## Management Platforms by Vendor

| Vendor | Management Platform |
|--------|-------------------|
| Fortinet | FortiManager + FortiAnalyzer |
| Cisco (Viptela) | vManage |
| VMware (VeloCloud) | VCO (VeloCloud Orchestrator) |
| Palo Alto | Prisma SD-WAN Controller |
| Aruba/HPE | Aruba EdgeConnect Orchestrator |

## Best Practices

1. **Use templates** -- Never configure sites individually
2. **Version control** -- Track configuration changes with revision history
3. **Staged rollouts** -- Test changes on pilot sites before global deployment
4. **Backup configs** -- Regular automated backups of all device configurations
5. **Integrate with ITSM** -- Link changes to tickets for audit trail

!!! tip "Fortinet management"
    FortiManager provides centralized SD-WAN management with SD-WAN Orchestrator templates. See [FortiManager Integration](../implementations/fortinet/fortimanager.md).
