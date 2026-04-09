---
title: Multi-Cloud Connectivity
description: Connecting SD-WAN to AWS, Azure, GCP, and other cloud providers for hybrid and multi-cloud architectures.
tags:
  - multi-cloud
  - aws
  - azure
  - gcp
---

# :material-cloud-outline: Multi-Cloud Connectivity

SD-WAN extends the enterprise WAN into public cloud providers, enabling optimized connectivity to IaaS and SaaS workloads.

## Cloud On-Ramp

Cloud on-ramp provides optimized paths from SD-WAN branches to cloud workloads:

- **Direct cloud peering** -- SD-WAN edge connects directly to cloud provider networks
- **Virtual hub in cloud** -- Deploy SD-WAN virtual appliances in AWS/Azure/GCP VPCs
- **Cloud interconnect** -- Use Express Route (Azure), Direct Connect (AWS), or Cloud Interconnect (GCP)

## Deployment Models

### Virtual SD-WAN Hub in Cloud

Deploy a virtual SD-WAN appliance (e.g., FortiGate-VM) in the cloud:

- Acts as a hub for branches accessing cloud workloads
- Provides full security stack in the cloud
- Enables cloud-to-cloud connectivity

### SaaS Optimization

Direct internet breakout for SaaS applications:

- Microsoft 365, Salesforce, Zoom, etc.
- Break out locally at each branch
- SD-WAN steers SaaS traffic to the nearest cloud edge

## Supported Cloud Providers

| Provider | SD-WAN Integration | Virtual Appliance |
|----------|--------------------|-------------------|
| AWS | Transit Gateway, VPC | FortiGate-VM, vEdge |
| Azure | Virtual WAN, VNET | FortiGate-VM, NVA |
| GCP | Cloud Router, VPC | FortiGate-VM |
| Oracle Cloud | FastConnect | FortiGate-VM |

!!! tip "Fortinet cloud integration"
    FortiGate-VM is available in all major cloud marketplaces and can be managed centrally via FortiManager alongside physical FortiGates.
