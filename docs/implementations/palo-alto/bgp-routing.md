---
title: "Palo Alto: BGP & Routing"
description: BGP configuration with SD-WAN overlays on Palo Alto platforms.
tags:
  - palo-alto
  - bgp
  - routing
---

# :material-routes: BGP & Routing

BGP is commonly used in SD-WAN deployments for route exchange between hubs, branches, and cloud environments.

## PAN-OS BGP with SD-WAN

### Virtual Router BGP Configuration

```
Network > Virtual Routers > default > BGP
  Enable: Yes
  Router ID: 10.255.0.1
  AS Number: 65000
  
  Peer Group: SD-WAN-Hubs
    Type: iBGP
    Peer AS: 65000
    Enable: Yes
    
    Peer: Hub-Primary
      Peer Address: 10.255.0.1
      Local Address: 10.255.0.100 (tunnel interface)
      Enable: Yes
      
    Peer: Hub-Secondary
      Peer Address: 10.255.0.2
      Local Address: 10.255.0.100
      Enable: Yes
```

### Route Redistribution

```
Network > Virtual Routers > default > BGP > Redistribution
  Redistribute:
    Connected Routes: Yes
      Route Map: CONNECTED-TO-BGP
    Static Routes: Yes
      Route Map: STATIC-TO-BGP
```

### Route Filtering with SD-WAN

```
Objects > Route Maps
  Name: SDWAN-ROUTE-MAP
  Sequence 10:
    Match: Prefix List "Branch-Subnets"
    Action: Permit
    Set: Community 65000:100, Local Preference 200
  Sequence 20:
    Match: any
    Action: Deny
```

## Prisma SD-WAN Routing

### BGP on ION Devices (API)

```python
bgp_config = {
    "router_id": "10.255.0.100",
    "local_as_num": 65000,
    "admin_state": "enabled",
    "peers": [
        {
            "name": "Hub-BGP",
            "peer_ip": "10.255.0.1",
            "remote_as_num": 65000,
            "route_reflector_client": False,
            "admin_state": "enabled",
            "hold_time": 90,
            "keepalive_time": 30,
            "address_families": [
                {
                    "afi": "ipv4",
                    "safi": "unicast",
                    "default_originate": False,
                    "soft_reconfiguration": True
                }
            ]
        }
    ],
    "networks": [
        {"prefix": "192.168.100.0/24"}
    ]
}

sdk.put.bgpconfigs(site_id, element_id, bgp_config)
```

### Route Advertisements

Prisma SD-WAN automatically advertises connected subnets and can be configured to advertise:

- Connected routes
- Static routes
- Routes learned from LAN-side BGP peers
- Summarized prefixes

## Verification

=== "PAN-OS"

    ```
    > show routing protocol bgp summary
    > show routing protocol bgp peer
    > show routing protocol bgp rib-out
    > show routing protocol bgp loc-rib
    ```

=== "Prisma SD-WAN"

    ```python
    # Check BGP status
    bgp_status = sdk.get.bgpconfigs(site_id, element_id)
    peers = sdk.get.bgppeers(site_id, element_id)
    for peer in peers.json()['items']:
        print(f"{peer['name']}: {peer['peer_state']}")
    ```

!!! tip "iBGP with route reflectors"
    For hub-and-spoke SD-WAN, configure the hub as a BGP route reflector. This eliminates the need for a full mesh of iBGP sessions between all branches.
