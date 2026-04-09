---
title: Healthcare Networks
description: SD-WAN for healthcare — HIPAA compliance, telehealth, medical IoT, and clinical applications.
tags:
  - healthcare
  - hipaa
  - telehealth
  - use-case
---

# :material-hospital-building: Healthcare Networks

Healthcare organizations use SD-WAN to connect clinics, hospitals, and remote facilities while maintaining HIPAA compliance and supporting critical clinical applications.

## Key Requirements

- **HIPAA compliance** -- End-to-end encryption for PHI
- **Application priority** -- EMR/EHR systems always get priority
- **Telehealth** -- Low-latency video for remote consultations
- **Medical IoT** -- Secure connectivity for medical devices
- **Segmentation** -- Isolate clinical, administrative, and patient networks

## Critical Applications

| Application | SLA Requirement | Path |
|-------------|-----------------|------|
| EMR/EHR (Epic, Cerner) | Low latency, zero loss | Best path, guaranteed |
| PACS (Medical Imaging) | High bandwidth | Dedicated bandwidth |
| Telehealth (Video) | Low latency, low jitter | Priority path |
| VoIP | Real-time SLA | Priority path |
| Guest WiFi | Best effort | Local internet |

!!! warning "HIPAA encryption"
    All traffic containing PHI must be encrypted in transit. SD-WAN IPsec tunnels satisfy this requirement, but ensure encryption is enabled on ALL overlay tunnels without exception.
