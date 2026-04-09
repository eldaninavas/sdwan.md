---
title: "Fortinet: Best Practices"
description: FortiGate SD-WAN design and configuration best practices for production deployments.
tags:
  - fortinet
  - best-practices
  - design
---

# :material-star: Best Practices

Proven design and configuration best practices for FortiGate SD-WAN in production environments.

## Design Best Practices

1. **Use ADVPN** for any deployment with more than 5 spokes -- avoids full mesh tunnel complexity
2. **Dual WAN at every site** -- minimum two ISPs for redundancy
3. **LTE as tertiary backup** -- last-resort connectivity
4. **Separate overlay zones** from underlay zones in SD-WAN
5. **Use FortiManager** for any deployment with more than 3 sites

## Configuration Best Practices

1. **Always use IKEv2** -- faster, more secure, supports MOBIKE
2. **AES-256-GCM** for encryption -- hardware accelerated on all FortiGates
3. **DH Group 14+** -- minimum 2048-bit key exchange
4. **`net-device enable`** on all overlay tunnels
5. **`add-route disable`** on tunnels -- let BGP handle routing
6. **Health check interval 500ms** for broadband, 1000ms for LTE

## SLA Design

| Application Tier | Health Check Interval | Failtime | SLA Latency | SLA Loss |
|------------------|-----------------------|----------|-------------|----------|
| Real-Time | 500ms | 3 | 50ms | 0% |
| Interactive | 500ms | 3 | 150ms | 1% |
| Bulk | 1000ms | 5 | 300ms | 5% |
| Best Effort | 1000ms | 5 | N/A | N/A |

## Security Best Practices

1. **Full UTM on direct internet breakout** -- NGFW, IPS, web filter
2. **VRF segmentation** for PCI/HIPAA compliance
3. **Certificate-based auth** for large deployments
4. **Disable weak ciphers** -- no DES, 3DES, MD5, DH1-2

## Operational Best Practices

1. **Staged rollouts** -- pilot 5-10 sites before full deployment
2. **Backup configs** before every change
3. **Monitor SLA continuously** via FortiAnalyzer
4. **Document everything** -- topology, IP plan, tunnel matrix
5. **Test failover regularly** -- simulate link failures monthly
