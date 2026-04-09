---
title: Zero-Touch Provisioning
description: Automated site deployment using ZTP in SD-WAN — how devices self-configure upon connection.
tags:
  - ztp
  - provisioning
  - automation
---

# :material-auto-fix: Zero-Touch Provisioning

Zero-Touch Provisioning (ZTP) allows new SD-WAN edge devices to automatically download their configuration and join the overlay network without any manual intervention at the site.

## How ZTP Works

1. **Pre-staging** -- Administrator defines the site configuration in the orchestrator
2. **Shipping** -- Device is shipped directly to the site (no pre-configuration needed)
3. **Power on** -- Device boots and contacts the cloud/orchestrator via DHCP + internet
4. **Authentication** -- Device authenticates using serial number or certificate
5. **Configuration download** -- Full configuration is pulled from the orchestrator
6. **Tunnel formation** -- IPsec tunnels are established to hub/other sites
7. **Operational** -- Site is live and managed centrally

## Benefits

| Benefit | Impact |
|---------|--------|
| No on-site IT needed | Ship to non-technical staff |
| Minutes vs weeks | Rapid site deployment |
| Consistent configs | Eliminates human error |
| Scalable | Deploy hundreds of sites simultaneously |

## Security Considerations

- Ensure ZTP uses certificate-based or serial-number authentication
- Use HTTPS for all orchestrator communications
- Validate device identity before pushing configurations
- Consider network admission control for initial DHCP access

!!! example "Fortinet ZTP"
    FortiGate devices support ZTP via FortiDeploy (Fortinet's cloud provisioning service). Pre-register the device serial number, assign a FortiManager, and the device auto-configures on first boot.
