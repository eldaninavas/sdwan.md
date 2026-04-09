---
title: "Palo Alto: VPN Tunnels"
description: IPsec VPN and SD-WAN tunnel configuration for Palo Alto SD-WAN platforms.
tags:
  - palo-alto
  - vpn
  - ipsec
  - tunnels
---

# :material-tunnel: VPN Tunnels

SD-WAN tunnels provide encrypted connectivity between sites. Both Palo Alto platforms automate tunnel creation but differ in approach.

## PAN-OS SD-WAN VPN

### VPN Cluster Configuration

PAN-OS SD-WAN automates tunnel creation through **VPN Clusters** in Panorama:

```
Panorama > SD-WAN > VPN Clusters
  Name: Corporate-SDWAN
  Type: Hub-and-Spoke
  
  Hub:
    Device Group: DC-Firewalls
    Hub Interface: ethernet1/1 (ISP1), ethernet1/2 (ISP2)
    
  Branches:
    Device Group: Branch-Firewalls
    Branch Interface: ethernet1/1 (WAN1), ethernet1/2 (WAN2)
    
  Crypto Settings:
    IKE Crypto Profile: Suite-B-GCM-256
    IPsec Crypto Profile: Suite-B-GCM-256
    IKE Version: IKEv2
    DH Group: 19 (256-bit ECDH)
    Authentication: Certificate
```

### IKE Crypto Profile

```
Network > IKE Crypto Profile
  Name: SD-WAN-IKE
  DH Group: 19 (256-bit ECDH)
  Authentication: SHA-256
  Encryption: AES-256-GCM
  Key Lifetime: 8 hours
```

### IPsec Crypto Profile

```
Network > IPsec Crypto Profile
  Name: SD-WAN-IPsec
  Protocol: ESP
  Encryption: AES-256-GCM
  DH Group: 19
  Lifetime: 1 hour
```

### Tunnel Monitoring

```
> show vpn ipsec-sa

GwID  TnID  Name                        State   Gateway          Enc       Hash   DH
---- ----- ------                       ------  --------         -----     ----   --
1     1     sdwan_branch1_isp1          active  203.0.113.10     aes256gcm sha256 19
1     2     sdwan_branch1_isp2          active  198.51.100.10    aes256gcm sha256 19
2     1     sdwan_branch2_isp1          active  192.0.2.10       aes256gcm sha256 19
```

## Prisma SD-WAN VPN

### VPN Configuration (API)

Prisma SD-WAN creates tunnels on-demand based on policy:

```python
# Configure VPN link between sites
vpn_link = {
    "name": "HQ-to-Branch1",
    "type": "ipsec",
    "admin_state": "enabled",
    "source_site_id": "<hq-site-id>",
    "target_site_id": "<branch1-site-id>",
    "ipsec_profile": {
        "ike_version": "v2",
        "encryption": "aes-256-gcm",
        "dh_group": "group14",
        "authentication": "psk",
        "lifetime_seconds": 3600
    }
}

sdk.post.vpnlinks(vpn_link)
```

### Anynet (Mesh VPN)

Prisma SD-WAN's **Anynet** technology automatically builds optimal VPN meshes:

- Automatically creates tunnels between sites that need to communicate
- Tears down idle tunnels to reduce overhead
- Supports both hub-spoke and dynamic mesh
- No manual tunnel configuration per site pair

## Comparison

| Feature | PAN-OS SD-WAN | Prisma SD-WAN |
|---------|--------------|---------------|
| Tunnel creation | Automated via VPN Cluster | Automated via Anynet |
| Crypto | Suite-B supported | AES-256-GCM |
| Authentication | Certificate or PSK | PSK or certificate |
| Dynamic mesh | Via VPN cluster config | Automatic (Anynet) |
| Tunnel monitoring | CLI + Panorama | Portal + API |

!!! warning "Certificate auth for production"
    Always use certificate-based authentication for production SD-WAN deployments. PSK is acceptable for lab/testing but doesn't scale securely for large deployments.
