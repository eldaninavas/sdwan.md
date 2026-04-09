---
title: Remote Work & Hybrid
description: Extending SD-WAN to home offices and remote workers with SASE and ZTNA.
tags:
  - remote-work
  - hybrid
  - ztna
  - use-case
---

# :material-home-account: Remote Work & Hybrid

SD-WAN extends to support remote and hybrid workers through SASE (Secure Access Service Edge) and ZTNA (Zero Trust Network Access).

## Approaches

### SD-WAN Appliance at Home

- Deploy a small SD-WAN appliance (e.g., FortiGate 40F/60F) at home offices
- Full SD-WAN overlay back to corporate
- Same security policies as branch offices
- Best for power users or executives

### SASE / Cloud-Based SD-WAN

- Agent-based or agentless ZTNA
- Traffic routed through nearest SASE PoP
- No hardware required at the home
- Best for general remote workers

### VPN Fallback

- Traditional SSL-VPN or IPsec client
- Lowest cost, highest friction
- Best as a backup option

## Comparison

| Feature | Home SD-WAN | SASE/ZTNA | VPN |
|---------|------------|-----------|-----|
| Security level | Full NGFW | Cloud-based | Basic |
| User experience | Best | Good | Acceptable |
| Cost per user | High | Medium | Low |
| IT overhead | Medium | Low | Low |
| Application access | Full network | Per-app | Full network |
