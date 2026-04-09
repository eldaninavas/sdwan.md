---
title: IPsec & Encryption
description: IPsec tunnel encryption, key management, and cryptographic best practices in SD-WAN.
tags:
  - ipsec
  - encryption
  - vpn
---

# :material-lock: IPsec & Encryption

All SD-WAN overlay tunnels are encrypted using IPsec, ensuring data confidentiality and integrity across untrusted transports like broadband and LTE.

## IPsec in SD-WAN

### Phase 1 (IKE)

Establishes a secure channel for negotiating encryption parameters:

- **IKEv2** preferred over IKEv1 (faster, more secure)
- Mutual authentication using pre-shared keys (PSK) or certificates
- Diffie-Hellman key exchange for perfect forward secrecy

### Phase 2 (IPsec SA)

Negotiates the actual data encryption parameters:

- **ESP** (Encapsulating Security Payload) for encryption + authentication
- Encryption algorithms: AES-256-GCM (recommended), AES-256-CBC
- Hash algorithms: SHA-256, SHA-384, SHA-512

## Recommended Settings

| Parameter | Recommended Value |
|-----------|-------------------|
| IKE Version | IKEv2 |
| Encryption | AES-256-GCM |
| Hash | SHA-256 |
| DH Group | 14 (2048-bit) or higher |
| PFS | Enable (DH Group 14+) |
| SA Lifetime | 28800s (Phase 1), 3600s (Phase 2) |
| DPD | Enable (10s interval) |

## Certificate-Based Authentication

For large deployments, use certificates instead of PSK:

- Centralized certificate authority (CA)
- Automatic certificate enrollment and renewal
- Stronger authentication than shared keys
- Easier to manage at scale

!!! warning "Avoid weak algorithms"
    Never use DES, 3DES, MD5, or DH Groups 1-2. These are considered cryptographically weak and should not be used in production SD-WAN deployments.
