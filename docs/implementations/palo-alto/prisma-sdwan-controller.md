---
title: "Palo Alto: Prisma SD-WAN Controller"
description: Prisma SD-WAN controller and portal management — sites, policies, and automation.
tags:
  - palo-alto
  - prisma-sdwan
  - controller
  - portal
---

# :material-cloud-cog: Prisma SD-WAN Controller

The Prisma SD-WAN Controller is a cloud-native management platform for all ION devices. It provides policy management, monitoring, and full API access.

## Portal Overview

The Prisma SD-WAN portal provides:

- **Dashboard** -- Global view of all sites, health, and alerts
- **Sites** -- Manage individual site configurations
- **Policies** -- Global and site-specific policies
- **Analytics** -- Application performance and WAN metrics
- **Inventory** -- Device management and ZTP

## Site Management

### Site Hierarchy

```
Tenant
├── Region: Americas
│   ├── Site: HQ-NYC (Hub)
│   ├── Site: Branch-LA (Spoke)
│   └── Site: Branch-CHI (Spoke)
├── Region: EMEA
│   ├── Site: Hub-LON (Hub)
│   └── Site: Branch-FRA (Spoke)
└── Region: APAC
    ├── Site: Hub-SIN (Hub)
    └── Site: Branch-TKY (Spoke)
```

### Site Configuration (API)

```python
import prisma_sase

sdk = prisma_sase.API()
sdk.interactive.login()

# List all sites
sites = sdk.get.sites()
for site in sites.json()['items']:
    print(f"{site['name']}: {site['admin_state']} ({site['element_cluster_role']})")

# Update site policy
site_update = {
    "policy_set_id": "<new-policy-set-id>",
    "admin_state": "active"
}
sdk.put.sites(site_id, site_update)
```

## Policy Management

### Policy Set Structure

```
Global Policy Set
├── Network Policy Rules (traffic steering)
├── QoS Policy Rules (bandwidth allocation)
├── NAT Policy Rules
├── Security Policy Rules
└── Path Policy Rules (SLA constraints)

Site-Specific Overrides
├── Local network rules
└── Site-specific QoS
```

### Bulk Operations (API)

```python
# Apply policy to all spoke sites
spoke_sites = [s for s in sites.json()['items'] 
               if s['element_cluster_role'] == 'SPOKE']

for site in spoke_sites:
    update = {"policy_set_id": "<standard-spoke-policy>"}
    sdk.put.sites(site['id'], update)
    print(f"Updated policy for {site['name']}")
```

## Terraform Support

Prisma SD-WAN has a Terraform provider for infrastructure-as-code:

```hcl
resource "prismasdwan_site" "branch" {
  name                  = "Branch-NYC-001"
  element_cluster_role  = "SPOKE"
  admin_state           = "active"
  policy_set_id         = prismasdwan_policy_set.spoke.id
  
  address {
    city    = "New York"
    state   = "NY"
    country = "US"
  }
}

resource "prismasdwan_wan_interface" "wan1" {
  site_id    = prismasdwan_site.branch.id
  element_id = prismasdwan_element.ion.id
  name       = "WAN 1"
  type       = "wan"
  label_id   = prismasdwan_wan_label.internet.id
  
  bw_config {
    uplink_bw   = 100
    downlink_bw = 100
  }
}
```

!!! info "API-first design"
    Every operation in the Prisma SD-WAN portal can be done via API. The portal is essentially a GUI wrapper around the API. This makes full automation achievable from day one.
