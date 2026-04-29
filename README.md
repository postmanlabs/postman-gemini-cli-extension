# Postman Gemini CLI Extension

The Postman Gemini CLI extension connects Postman to your Gemini AI tools and workflows, giving the ability to access workspaces, manage collections and environments, evaluate APIs, and automate workflows through natural language interactions.

## Prerequisites

Requires Gemini CLI v0.4.0 or later.

## Installation

Install the Postman extension by running the following command in your terminal:

```bash
gemini extensions install https://github.com/postmanlabs/postman-gemini-cli-extension
```

## Authentication

This extension connects to Postman's hosted MCP server and authenticates via **OAuth** — no API key required. Gemini CLI will open a browser window to complete the Postman login flow on first use.

### API key (alternative)

If you prefer API key authentication, set your key as an environment variable:

```bash
export POSTMAN_API_KEY=your-api-key-here
```

Then configure the server in Gemini CLI's `settings.json` to read it from the environment:

```json
{
  "mcpServers": {
    "postman": {
      "url": "https://mcp.postman.com/minimal",
      "headers": {
        "Authorization": "Bearer $POSTMAN_API_KEY"
      }
    }
  }
}
```

Add the `export` line to your `~/.zshrc` or `~/.bashrc` to persist it across sessions. Get your API key at [postman.postman.co/settings/me/api-keys](https://postman.postman.co/settings/me/api-keys).

## Features

* **AI-powered Postman integration** — Leverages Model Context Protocol (MCP) to provide intelligent and context-aware Postman operations through natural language.
* **Essential API management** — Access to Postman's core tools for collections, workspaces, environments, specs, and more.
* **Seamless workflow integration** — Integrates directly into your development environment, making API management an accessible part of your workflow.
* **Enterprise-ready** — Designed with extensible architecture supporting Postman's Enterprise features and advanced collaboration workflows.

## Tool Configuration

This extension uses the **minimal** toolset — fast, focused access to core Postman operations.

To use a different mode, update the URL in your Gemini CLI extension settings:

| Mode | URL | Use when |
|------|-----|----------|
| Minimal (default) | `https://mcp.postman.com/minimal` | Collections, workspaces, environments, specs |
| Full | `https://mcp.postman.com/mcp` | 100+ tools, advanced collaboration, Enterprise |
| Code | `https://mcp.postman.com/code` | API search and client code generation |

## EU Region

If your Postman account is in the EU region, use the EU endpoint in your Gemini CLI extension settings:

Set your key as an environment variable first:

```bash
export POSTMAN_API_KEY=your-api-key-here
```

Then configure the EU endpoint in Gemini CLI's `settings.json`:

```json
{
  "mcpServers": {
    "postman": {
      "url": "https://mcp.eu.postman.com/minimal",
      "headers": {
        "Authorization": "Bearer $POSTMAN_API_KEY"
      }
    }
  }
}
```

> **Note**: OAuth is not supported for the EU region — API key authentication is required.

## Use Cases

* **Code synchronization** — Effortlessly keep your code up to date based on changes made to your [Postman Collections](https://learning.postman.com/docs/collections/collections-overview/) and [specs](https://learning.postman.com/docs/design-apis/specifications/overview/).
* **Collection management** — Create and tag collections, update collection and request documentation, add comments, or perform actions across multiple collections without leaving your editor.
* **Workspace and environment management** — Create workspaces and environments, plus manage your environment variables.
* **Automatic spec creation** — Create specs from your code and use them to generate collections.
