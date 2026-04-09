---
title: Contributing
description: How to contribute to SDWAN.md — style guide, submission process, and content guidelines.
tags:
  - contributing
  - community
---

# :material-pencil: Contributing

Thank you for your interest in contributing to SDWAN.md! This guide explains how to submit content.

## Quick Start

1. Fork the [repository](https://github.com/eldaninavas/sdwan.md)
2. Create a branch for your changes
3. Write or edit content in the `docs/` folder
4. Submit a Pull Request

## Content Guidelines

- Write in clear, concise English
- Use code blocks for all CLI commands and configurations
- Include vendor and FortiOS version when relevant
- Add diagrams (Mermaid) where they help explain concepts
- Use admonitions (tip, warning, info) for callouts
- Add tags to page frontmatter

## Page Structure

Every page should have:

```yaml
---
title: Page Title
description: One-line description for SEO
tags:
  - relevant
  - tags
---
```

## Style Guide

- Use ATX-style headers (`#`, `##`, `###`)
- One sentence per line (easier diffs)
- Use Material for MkDocs admonitions for callouts
- Prefer Mermaid diagrams over images where possible
- Reference other wiki pages with relative links

## Code of Conduct

Be respectful, constructive, and inclusive. We're all here to learn.
