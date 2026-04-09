---
title: AI Integrations
description: Integrate SDWAN.md with Claude, ChatGPT, and other AI assistants
tags:
  - community
  - integrations
  - ai
  - mcp
---

# :material-robot: AI Integrations

SDWAN.md is designed to be AI-friendly. Use our knowledge base as context for your favorite AI assistant to get accurate, up-to-date answers about SD-WAN.

## Option 1: `llms.txt` (Any AI)

We provide standardized files that any LLM can consume:

| File | Description | Use Case |
|------|-------------|----------|
| [`/llms.txt`](https://sdwan.md/llms.txt) | Index of all pages with links | Quick reference, link discovery |
| [`/llms-full.txt`](https://sdwan.md/llms-full.txt) | Full content of all pages | Complete knowledge base in one file |

### How to Use

Simply paste one of these URLs in your conversation with any AI:

```
Read https://sdwan.md/llms-full.txt and use it as context.
Then answer: How do I configure ADVPN hub-and-spoke with FortiGate?
```

Works with **Claude**, **ChatGPT**, **Gemini**, and any AI that can fetch URLs.

---

## Option 2: Claude Project

Upload the wiki as a **Knowledge Base** in a Claude Project for persistent context.

### Steps

1. Go to [claude.ai](https://claude.ai) → **Projects** → **New Project**
2. Name it "SD-WAN Expert" (or whatever you prefer)
3. Download the docs from [GitHub](https://github.com/eldaninavas/sdwan.md)
4. Upload all `.md` files from `docs/` as **Knowledge**
5. Set the project instructions:

```
You are an SD-WAN networking expert with deep knowledge of Fortinet
FortiGate SD-WAN. Use the uploaded knowledge base to answer questions
about SD-WAN architecture, configurations, troubleshooting, and
best practices. Always cite the relevant page when answering.
```

Now every conversation in that project has full SD-WAN knowledge.

---

## Option 3: MCP Server (Claude Code / Claude Desktop)

The **MCP (Model Context Protocol)** server gives Claude direct tool access to search and read the wiki.

### Available Tools

| Tool | Description |
|------|-------------|
| `search_sdwan` | Search the knowledge base by keyword |
| `list_sdwan_pages` | List all available wiki pages |
| `read_sdwan_page` | Read a specific page by its path |

### Setup for Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "sdwan": {
      "command": "npx",
      "args": ["sdwan-mcp-server"]
    }
  }
}
```

### Setup for Claude Code

```bash
claude mcp add sdwan npx sdwan-mcp-server
```

### What You Can Ask

Once the MCP server is connected, Claude can directly query the knowledge base:

- *"How do I configure SD-WAN health checks on FortiGate?"*
- *"Show me the ADVPN hub configuration for dual-hub HA"*
- *"What's the difference between SLA mode and lowest-cost SLA?"*
- *"How do I troubleshoot SD-WAN path selection issues?"*

### Source Code

The MCP server source is in the [`mcp-server/`](https://github.com/eldaninavas/sdwan.md/tree/main/mcp-server) directory of the repository.

---

## For Developers

### Build Your Own Integration

The content is available in multiple formats:

| Format | URL | Best For |
|--------|-----|----------|
| Raw Markdown | [GitHub `docs/`](https://github.com/eldaninavas/sdwan.md/tree/main/docs) | Custom parsers, RAG pipelines |
| `llms.txt` | `https://sdwan.md/llms.txt` | LLM index discovery |
| `llms-full.txt` | `https://sdwan.md/llms-full.txt` | Single-file ingestion |
| MCP Server | `npx sdwan-mcp-server` | Claude integrations |
| Website | `https://sdwan.md` | Human browsing |

### RAG Pipeline Example

```python
# Example: Build a RAG pipeline with the wiki content
import requests

# Fetch all content
response = requests.get("https://sdwan.md/llms-full.txt")
content = response.text

# Split into chunks, embed, and store in your vector DB
# Then query with your LLM of choice
```

!!! tip "All content is MIT licensed"
    You're free to use, modify, and redistribute the content for any purpose, including commercial AI applications.
