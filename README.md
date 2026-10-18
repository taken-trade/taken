<div align="center">

# taken.

**Trading workflows, connected to your AI.**

Browser automation and developer tools for supported trading-platform workflows.

[Open Taken](https://app.taken.trade) · [Quick Start](docs/QUICKSTART.md) · [API & MCP](docs/INTEGRATIONS.md) · [Documentation](https://taken.trade/docs) · [Security](SECURITY.md)

</div>

---

Taken gives traders and developers a way to connect AI agents to browser workflows, manage supported account connections, and follow tasks from instruction to result. Users can watch progress, pause automation, take over manually, or cancel a task.

Trading platforms are often built around people navigating websites and repeating steps across accounts. Taken is building a common interface for agents to work with those workflows while users retain control.

> [!NOTE]
> Taken is under active development. Trading and payment actions are currently blocked; no API key or MCP tool can place, approve, or execute trades. Supported workflows and limits depend on your plan and configuration.

## Get started

### Use the hosted app

Open [app.taken.trade](https://app.taken.trade), sign in, and try an available browser workflow. No local installation is required for the hosted app.

A simple first task:

> Open example.com and tell me the page title.

### Connect an application

Create a scoped API key in **Taken → profile menu → Developer Settings**. Store it in your local environment or secret manager as `TAKEN_API_KEY`. For this read-only example, choose the `sessions:read` permission.

```bash
curl --fail-with-body --silent --show-error \
  'https://taken.trade/api/public/v1/browser-sessions' \
  -H "Authorization: Bearer ${TAKEN_API_KEY:?Set TAKEN_API_KEY in your environment}"
```

This lists existing sessions; it does not start a browser task. See the [Quick Start](docs/QUICKSTART.md) for task creation, polling, and errors.

### Connect an AI agent with MCP

Taken provides a remote MCP endpoint for clients that support **Streamable HTTP and a custom Authorization header**:

```text
https://taken.trade/api/public/mcp
Authorization: Bearer <your Taken API key>
```

OAuth is not currently available. Clients that require OAuth for remote MCP connections cannot connect through that flow yet. See [API & MCP integration](docs/INTEGRATIONS.md) for requirements and tool names.

## What you can build

| Capability | Use |
| --- | --- |
| Browser tasks | Submit natural-language instructions for supported sites and retrieve progress and results. |
| Session visibility | Inspect your browser sessions and their current state. |
| Connected platforms | Read supported platform connection status; manage connections in the app. |
| Order drafts | Parse an order description for review, without approval or submission. |
| User controls | Watch, pause, take over manually, and cancel tasks in the app. |
| Chrome companion | Use supported local-browser workflows through Taken's browser integration. |

## How it works

```text
You / your AI agent
        |
        v
Taken app, REST API, or MCP
        |
        v
Authentication + scopes + limits
        |
        v
Supported browser workflow  <-- User controls in Taken
        |
        v
Progress + result
```

An API key acts as its owner and is limited by its scopes. REST and MCP use the same account and subscription limits as the app. Tasks run in bounded steps; polling a running task advances it unless `advance=false` is specified.

## Developer resources

| Resource | Start here |
| --- | --- |
| First API request | [Quick Start](docs/QUICKSTART.md) |
| REST API and MCP tools | [Integration guide](docs/INTEGRATIONS.md) |
| Complete API reference | [Hosted reference](https://taken.trade/docs/api-reference) |
| Machine-readable API description | [OpenAPI](https://taken.trade/api/public/v1/openapi.json) |
| Security and credential handling | [Security policy](SECURITY.md) |
| Documentation improvements | [Contributing](CONTRIBUTING.md) |

## Project status

This repository contains Taken's product information, integration guides, and request examples. The hosted application's source code, production configuration, and internal development history are maintained privately. This repository does not provide a self-hosted application or an installable SDK.

Development focuses on supported browser workflows, developer integrations, and user controls. Live execution, OAuth support, and webhook availability should be checked against the current [documentation](https://taken.trade/docs); this overview does not promise a release date.

## Contact

For product questions, integrations, partnerships, or investment discussions: [contact@taken.trade](mailto:contact@taken.trade).

Report vulnerabilities privately using [SECURITY.md](SECURITY.md).

© 2026 Taken. All rights reserved.
