# SDWAN.md

> The open-source knowledge base for **Software-Defined Wide Area Networking (SD-WAN)**.

[![Deploy](https://github.com/eldaninavas/sdwan.md/actions/workflows/deploy.yml/badge.svg)](https://github.com/eldaninavas/sdwan.md/actions/workflows/deploy.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-26a69a.svg)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](docs/community/contributing.md)

## About

[SDWAN.md](https://sdwan.md) is a community-driven wiki covering everything about SD-WAN: fundamentals, application-aware routing, security, vendor implementations (Fortinet first), and real-world use cases.

## What's Inside

- **Topics** — SD-WAN architecture, overlay/underlay, dynamic path selection, QoS, SASE, ZTP, and more
- **Use Cases** — Branch connectivity, multi-cloud, SaaS optimization, retail, healthcare, MPLS migration
- **Implementations** — Fortinet FortiGate SD-WAN with full CLI configurations (16 pages covering basics to advanced ADVPN multi-hub)
- **Community** — Contributing guide, AI integrations (llms.txt, Claude Project, MCP)

## Local Development

```bash
# Clone
git clone https://github.com/eldaninavas/sdwan.md.git
cd sdwan.md

# Install dependencies
pip install -r requirements.txt

# Serve locally (with live reload)
mkdocs serve

# Build static site
mkdocs build
```

## Contributing

We welcome contributions! See our [Contributing Guide](docs/community/contributing.md) for details.

**Quick edit:** Every page on the site has an edit button (pencil icon) that takes you directly to the file on GitHub.

## Structure

```
docs/
  ├── topics/              # Core SD-WAN concepts (18 pages)
  ├── use-cases/           # Real-world applications (9 pages)
  ├── implementations/
  │   └── fortinet/        # FortiGate SD-WAN guides (16 pages)
  └── community/           # Contributing, integrations, legal
```

## License

MIT License — see [LICENSE](LICENSE) for details.
