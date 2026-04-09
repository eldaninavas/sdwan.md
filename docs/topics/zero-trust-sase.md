---
title: Zero Trust & SASE
description: Secure Access Service Edge (SASE) framework and Zero Trust Network Access (ZTNA) in SD-WAN.
tags:
  - sase
  - ztna
  - zero-trust
---

# :material-shield-check: Zero Trust & SASE

SASE (Secure Access Service Edge) converges SD-WAN and security into a single cloud-delivered service, while Zero Trust Network Access (ZTNA) ensures that trust is never implicit.

## What is SASE?

SASE combines:

- **SD-WAN** -- Optimized, policy-based WAN connectivity
- **SWG** -- Secure Web Gateway
- **CASB** -- Cloud Access Security Broker
- **ZTNA** -- Zero Trust Network Access
- **FWaaS** -- Firewall as a Service

## Zero Trust Principles

1. **Never trust, always verify** -- Authenticate and authorize every user and device
2. **Least privilege access** -- Grant minimum necessary permissions
3. **Assume breach** -- Design security as if the network is already compromised
4. **Continuous validation** -- Re-evaluate trust continuously, not just at login

## ZTNA vs VPN

| Feature | Traditional VPN | ZTNA |
|---------|----------------|------|
| Access model | Network-level (full access) | Application-level (per-app) |
| Trust model | Trust after authentication | Continuous verification |
| Attack surface | Broad (entire network) | Minimal (specific apps) |
| User experience | VPN client required | Transparent, agent or agentless |

!!! tip "Fortinet SASE"
    Fortinet offers FortiSASE, which integrates FortiGate SD-WAN with cloud-delivered security services. This provides a unified solution for both on-premises and remote users.
