---
title: "Palo Alto: Best Practices"
description: Design and configuration best practices for Palo Alto SD-WAN deployments.
tags:
  - palo-alto
  - best-practices
  - design
---

# :material-star: Best Practices

Proven design and configuration best practices for Palo Alto SD-WAN in production.

## Design Best Practices

1. **Choose the right product** -- PAN-OS SD-WAN for existing Palo Alto customers; Prisma SD-WAN for greenfield or overlay-only
2. **Dual WAN at every site** -- Minimum two diverse ISPs
3. **Hub redundancy** -- Dual hub for any deployment with > 10 sites
4. **Enable spoke-to-spoke** -- On-demand direct tunnels for inter-branch traffic
5. **Template everything** -- Never configure sites individually

## PAN-OS SD-WAN Best Practices

1. **Use Panorama template variables** -- Site-specific values as variables, not hard-coded
2. **Separate device groups** by site size (small/medium/large branch)
3. **Apply full security profiles** to all SD-WAN policy rules
4. **Use App-ID** instead of port-based rules for traffic distribution
5. **Enable SSL decryption** for SaaS traffic going direct-to-internet
6. **Size firewalls with SD-WAN overhead** -- IPsec encryption consumes CPU

## Prisma SD-WAN Best Practices

1. **Use the API** for all repeatable operations
2. **Terraform for IaC** -- Manage site configs as code
3. **Global policy set** for common rules; site overrides only when necessary
4. **Leverage ADEM** for proactive performance monitoring
5. **Connect to Prisma Access** for cloud-delivered security on internet-bound traffic
6. **Right-size ION devices** -- Match throughput to actual WAN bandwidth

## Security Best Practices

| Practice | PAN-OS | Prisma SD-WAN |
|----------|--------|---------------|
| Encryption | Suite-B (AES-256-GCM, ECDH) | AES-256-GCM |
| Authentication | Certificate-based | Certificate or PSK |
| Segmentation | Zone-based + Security Policy | Network context policies |
| Internet security | On-box NGFW | Prisma Access (cloud) |
| Zero Trust | GlobalProtect + ZTNA | Prisma Access ZTNA |

## Operational Best Practices

1. **Staged rollouts** -- Pilot 5-10 sites, validate, then expand
2. **Monitor SLA continuously** -- Set alerts for SLA violations
3. **Regular failover testing** -- Simulate link failures monthly
4. **Keep software current** -- SD-WAN features improve significantly between releases
5. **Document your design** -- Topology, IP plan, policy matrix, tunnel matrix
6. **Backup before changes** -- Panorama config export or API config dump

!!! warning "SD-WAN license"
    PAN-OS SD-WAN requires an additional license per firewall. Ensure licensing is in place before deploying the SD-WAN plugin. Prisma SD-WAN is subscription-based per ION device.
