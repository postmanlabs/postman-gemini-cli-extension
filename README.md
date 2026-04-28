# Postman Gemini CLI Extension

The Postman Gemini CLI extension connects Postman to your Gemini AI tools and workflows, giving the ability to access workspaces, manage collections and environments, evaluate APIs, and automate workflows through natural language interactions.

## Prerequisites

Requires Gemini CLI v0.4.0 or later.

## Installation

Install the Postman extension by running the following command in your terminal:

```bash
gemini extensions install https://github.com/postmanlabs/postman-gemini-cli-extension
```

## Features

* **AI-powered Postman integration** — Leverages Model Context Protocol (MCP) to provide intelligent and context-aware Postman operations through natural language.
* **Essential API management** — Access to Postman's core tools for collections, workspaces, environments, specs, and more.
* **Seamless workflow integration** — Integrates directly into your development environment, making API management an accessible part of your workflow.
* **Enterprise-ready** — Designed with extensible architecture supporting Postman's Enterprise features and advanced collaboration workflows.

## Tool Configuration

This extension uses the **minimal** toolset by default — the recommended configuration for most users, providing fast, focused access to core Postman operations.

| Mode | Tools | Use when |
|------|-------|----------|
| `--minimal` (default) | Essential tools | Collections, workspaces, environments, specs |
| `--full` | 100+ tools | Advanced collaboration, Enterprise features |
| `--code` | Code tools | API search and client code generation |

To switch modes, update the `args` in your Gemini CLI extension settings.

## EU Region

If your Postman account is in the EU region, add the `--region eu` flag to the args in your Gemini CLI extension settings:

```json
"args": ["-y", "@postman/postman-mcp-server@latest", "--minimal", "--region", "eu"]
```

## Use Cases

* **Code synchronization** — Effortlessly keep your code up to date based on changes made to your [Postman Collections](https://learning.postman.com/docs/collections/collections-overview/) and [specs](https://learning.postman.com/docs/design-apis/specifications/overview/).
* **Collection management** — Create and tag collections, update collection and request documentation, add comments, or perform actions across multiple collections without leaving your editor.
* **Workspace and environment management** — Create workspaces and environments, plus manage your environment variables.
* **Automatic spec creation** — Create specs from your code and use them to generate collections.
