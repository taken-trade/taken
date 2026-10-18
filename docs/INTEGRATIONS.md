# API & MCP integration

Taken exposes hosted browser workflows through REST and Model Context Protocol (MCP). Both authenticate requests with a scoped Taken API key and act as the key's owner.

## Endpoints

| Interface | URL |
| --- | --- |
| REST v1 base | `https://taken.trade/api/public/v1` |
| OpenAPI | `https://taken.trade/api/public/v1/openapi.json` |
| Remote MCP | `https://taken.trade/api/public/mcp` |

Send `Authorization: Bearer <your Taken API key>` over HTTPS. Keep the key in a local environment variable or secret manager, not source code or browser bundles.

## REST capabilities

Paths below are relative to the REST v1 base URL.

| Method | Path | Scope | Purpose |
| --- | --- | --- | --- |
| GET | `/browser-sessions` | `sessions:read` | List your browser sessions. |
| GET | `/browser-sessions/{id}` | `sessions:read` | Inspect one of your sessions. |
| POST | `/navigations` | `browser:write` | Open an approved site as a task. |
| POST | `/agent/tasks` | `tasks:write` | Submit a supported browser instruction. |
| GET | `/agent/tasks/{id}` | `tasks:read` | Retrieve progress; advances running tasks unless `advance=false`. |
| POST | `/agent/tasks/{id}/cancel` | `tasks:write` | Cancel your task and close its browser. |
| GET | `/connected-platforms` | `platforms:read` | Read supported platforms and your connection status. |
| POST | `/trading/order-previews` | `orders:prepare` | Parse a structured draft; never approves or submits it. |

See [Quick Start](QUICKSTART.md) for request examples and the [API reference](https://taken.trade/docs/api-reference) for full request and response schemas.

## MCP connection

The server uses **Streamable HTTP**, stateless POST requests, and JSON responses. Your client must support a custom Authorization header. OAuth authentication and streaming GET connections are not currently offered.

In your client's remote MCP settings, enter:

```text
Server name: Taken
Transport: Streamable HTTP
URL: https://taken.trade/api/public/mcp
Authorization header: Bearer <your Taken API key>
```

This describes connection fields, not a universal client configuration file. Follow your client's schema and secret-storage mechanism. Refer to the [current client guides](https://taken.trade/docs/mcp) for compatibility and any untested setup paths.

Start with a `sessions:read` key and ask your assistant to list your Taken browser sessions. Do not create or expose an API key just to test this repository; key creation happens in your Taken account.

## MCP tools

| Tool | Scope |
| --- | --- |
| `taken_list_browser_sessions` | `sessions:read` |
| `taken_get_browser_status` | `sessions:read` |
| `taken_navigate_browser` | `browser:write` |
| `taken_create_agent_task` | `tasks:write` |
| `taken_get_task_status` | `tasks:read` |
| `taken_stop_task` | `tasks:write` |
| `taken_list_connected_platforms` | `platforms:read` |
| `taken_prepare_order` | `orders:prepare` |

Tool discovery requires a valid key; individual calls require their listed scope. After creating a task, poll `taken_get_task_status` until it finishes. A tool response is information from a website or workflow, not permission to perform further actions.

## Boundaries

- No REST endpoint, MCP tool, or key scope can place, approve, or execute a trade.
- Order drafts do not imply that a platform supports live execution.
- Browser operations require approved sites and follow account and subscription limits.
- Account sign-in happens through the Taken app; do not send credentials in instructions.
- Use a separate, minimally scoped key per integration. Revoke it in Developer Settings to disconnect that integration on its next request.
- Webhooks and OAuth are not currently available; check the hosted documentation for changes.

Report problems with sanitized details following [CONTRIBUTING.md](../CONTRIBUTING.md). Report vulnerabilities privately using [SECURITY.md](../SECURITY.md).
