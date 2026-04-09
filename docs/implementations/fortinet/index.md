---
title: Fortinet SD-WAN
description: Comprehensive Fortinet FortiGate SD-WAN implementation guide — from basics to advanced multi-hub ADVPN deployments.
tags:
  - fortinet
  - fortigate
  - implementations
---

# :material-shield-check: Fortinet SD-WAN

Fortinet SD-WAN is built into every FortiGate next-generation firewall, providing an integrated solution that combines SD-WAN, NGFW, IPS, and advanced routing in a single appliance.

## Why Fortinet SD-WAN?

- **Integrated security** -- NGFW, IPS, web filter, and sandbox in one box
- **No additional licensing** -- SD-WAN is included in every FortiGate
- **ASIC acceleration** -- Purpose-built SPU/NP processors for performance
- **FortiManager** -- Centralized SD-WAN orchestration
- **FortiAnalyzer** -- Deep analytics and SLA monitoring
- **ADVPN** -- Dynamic spoke-to-spoke tunnels

## Guide Contents

| Section | Description |
|---------|-------------|
| [SD-WAN Basics](sdwan-basics.md) | Enable and configure SD-WAN on FortiGate |
| [SD-WAN Zones](sdwan-zones.md) | Create and manage SD-WAN zones |
| [Members & Interfaces](members-interfaces.md) | Add WAN interfaces as SD-WAN members |
| [Health Check & SLA](health-check-sla.md) | Configure SLA health checks |
| [SD-WAN Rules](sdwan-rules.md) | Create application-aware SD-WAN rules |
| [Performance SLA](performance-sla.md) | Monitor and enforce performance SLAs |
| [BGP & Routing](bgp-routing.md) | BGP configuration with SD-WAN |
| [Hub & Spoke (ADVPN)](hub-spoke-advpn.md) | ADVPN dynamic tunnel configuration |
| [Multi-Hub Deployments](multi-hub.md) | Multi-hub with regional failover |
| [Dual-Hub HA](dual-hub-ha.md) | High availability with dual hubs |
| [IPsec Overlays](ipsec-overlays.md) | IPsec overlay tunnel configuration |
| [SD-WAN with VXLAN](sdwan-vxlan.md) | VXLAN integration with SD-WAN |
| [FortiManager Integration](fortimanager.md) | Centralized management |
| [FortiAnalyzer Monitoring](fortianalyzer.md) | Analytics and monitoring |
| [Best Practices](best-practices.md) | Design and configuration best practices |
| [Troubleshooting](troubleshooting.md) | Common issues and diagnostic commands |

## Supported FortiOS Versions

This guide targets **FortiOS 7.4+** but most concepts apply to 7.0 and 7.2 as well.

!!! tip "Lab it first"
    Always test SD-WAN configurations in a lab before deploying to production. FortiGate-VM is available for free evaluation and works great in VMware, KVM, or cloud environments.
