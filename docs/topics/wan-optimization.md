---
title: WAN Optimization
description: TCP optimization, data deduplication, compression, and caching techniques in SD-WAN.
tags:
  - wan-optimization
  - tcp
  - compression
---

# :material-speedometer-slow: WAN Optimization

WAN optimization techniques reduce bandwidth consumption and improve application performance over WAN links.

## Key Techniques

### TCP Optimization

- **TCP acceleration** -- Proxy TCP connections to reduce round-trip overhead
- **Window scaling** -- Optimize TCP window sizes for high-latency links
- **Selective acknowledgment** -- Reduce retransmissions on lossy links

### Data Deduplication

- Eliminate redundant data across the WAN
- Byte-level deduplication identifies repeated patterns
- Especially effective for repeated file transfers and backups

### Compression

- Real-time compression of WAN traffic
- Reduces bandwidth consumption by 40-60% for compressible data
- Most effective for text-based protocols (HTTP, CIFS, email)

### Protocol Optimization

- **CIFS/SMB acceleration** -- Optimize Windows file sharing
- **HTTP optimization** -- Caching, prefetching, and connection reuse
- **SSL/TLS optimization** -- Reduce handshake overhead

## SD-WAN vs Traditional WAN Optimization

| Feature | Traditional WAN Opt | SD-WAN |
|---------|--------------------|---------| 
| TCP optimization | Yes | Some vendors |
| Deduplication | Yes | Limited |
| Compression | Yes | Some vendors |
| Application routing | No | Yes |
| Path selection | No | Yes |
| Encryption | Separate | Built-in |

!!! note "Not all SD-WAN vendors include WAN optimization"
    WAN optimization is a separate feature from SD-WAN. Some vendors (like Fortinet) include basic WAN optimization in the SD-WAN appliance, while others require a separate appliance or license.
