---
title: Multi-Cloud Access
description: SD-WAN for optimized connectivity to multiple cloud providers — AWS, Azure, GCP.
tags:
  - multi-cloud
  - aws
  - azure
  - use-case
---

# :material-cloud-sync: Multi-Cloud Access

SD-WAN provides optimized, secure connectivity from branches and data centers to workloads running across multiple cloud providers.

## Design Patterns

### Virtual Hub in Cloud

Deploy FortiGate-VM instances in each cloud provider's VPC/VNET:

- Acts as SD-WAN hub in the cloud
- Provides consistent security policy across clouds
- Enables cloud-to-cloud connectivity via SD-WAN overlay

### Cloud Interconnect + SD-WAN

Combine dedicated cloud connections with SD-WAN overlay:

- AWS Direct Connect, Azure ExpressRoute, GCP Interconnect
- SD-WAN manages failover between dedicated and internet paths
- Best for high-bandwidth, latency-sensitive workloads

## Benefits

- **Consistent security** across all clouds
- **Optimized routing** to the nearest cloud region
- **Simplified management** from a single orchestrator
- **Cost optimization** by steering traffic to the best-cost path
