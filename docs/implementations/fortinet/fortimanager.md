---
title: "Fortinet: FortiManager Integration"
description: Centralized SD-WAN management with FortiManager — templates, orchestration, and policy deployment.
tags:
  - fortinet
  - fortimanager
  - management
  - orchestration
---

# :material-monitor-dashboard: FortiManager Integration

FortiManager provides centralized management for FortiGate SD-WAN deployments, enabling template-based configuration and SD-WAN orchestration at scale.

## SD-WAN Orchestration

FortiManager's SD-WAN Orchestrator provides:

- **Visual topology** -- Drag-and-drop site placement
- **Template-based deployment** -- Define once, apply to all sites
- **Hub/spoke auto-configuration** -- Automatic IPsec and BGP setup
- **Performance monitoring** -- Real-time SLA dashboards

## Key Features

| Feature | Description |
|---------|-------------|
| SD-WAN Templates | Reusable templates for members, health checks, rules |
| ADOM | Administrative domains for multi-tenant management |
| Device Groups | Group FortiGates for bulk operations |
| Scripts | CLI scripts for advanced customization |
| API | REST API for full automation |
| Workflow | Approval workflows for configuration changes |

## Template Deployment

1. Create SD-WAN template with members, health checks, and rules
2. Create IPsec template with overlay tunnels
3. Create BGP template with neighbor configuration
4. Assign templates to device group
5. Install to all devices in the group

## FortiDeploy (ZTP)

FortiDeploy enables zero-touch provisioning:

1. Register FortiGate serial numbers in FortiDeploy
2. Assign FortiManager IP and credentials
3. Ship device to site
4. Device auto-connects to FortiManager and pulls config

!!! tip "Use ADOMs"
    For multi-tenant or multi-region deployments, use ADOMs (Administrative Domains) to separate management contexts. Each ADOM can have its own templates, policies, and admin roles.
