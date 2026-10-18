# Quick Start

Use Taken's hosted API to inspect sessions or run a supported browser task. You need a Taken account and a scoped API key. Browser tasks require the appropriate subscription and consume your account's browser time and AI allowance.

## 1. Create a scoped key

In the [Taken app](https://app.taken.trade), open the profile menu → **Developer Settings → Create API Key**. Select only the permissions needed:

- `sessions:read` to list browser sessions.
- `tasks:write` to create or cancel browser tasks.
- `tasks:read` to retrieve task progress and results.

The secret is shown once. Store it in a secret manager or the `TAKEN_API_KEY` environment variable on your own machine. Never commit the value, put it in a URL, or include it in frontend code or an issue.

## 2. Make a read-only request

With `TAKEN_API_KEY` set in your local environment:

```bash
curl --fail-with-body --silent --show-error \
  'https://taken.trade/api/public/v1/browser-sessions' \
  -H "Authorization: Bearer ${TAKEN_API_KEY:?Set TAKEN_API_KEY in your environment}"
```

A successful response has a `data` field. An empty list means no matching sessions; listing sessions does not create one.

## 3. Create a browser task

This request starts work and uses your subscription allowance:

```bash
curl --fail-with-body --silent --show-error \
  'https://taken.trade/api/public/v1/agent/tasks' \
  -H "Authorization: Bearer ${TAKEN_API_KEY:?Set TAKEN_API_KEY in your environment}" \
  -H 'Content-Type: application/json' \
  --data '{"instruction":"Open example.com and tell me the page title"}'
```

Save the returned `data.id` locally as `TAKEN_TASK_ID`. Do not treat task acceptance as completion.

## 4. Follow the task

```bash
curl --fail-with-body --silent --show-error \
  "https://taken.trade/api/public/v1/agent/tasks/${TAKEN_TASK_ID:?Set TAKEN_TASK_ID to the returned data.id}" \
  -H "Authorization: Bearer ${TAKEN_API_KEY:?Set TAKEN_API_KEY in your environment}"
```

Each GET advances a running task by one bounded step and may take about 45 seconds. Repeat until `data.done` is true, then check `data.status` and `data.result`. A finished task may have failed, been cancelled, or been blocked. Paused and manual-control tasks do not advance until the appropriate action is taken in the app. Use `?advance=false` to inspect without advancing.

Do not blindly retry task creation after a network timeout: the first request may have been accepted. Check the app before creating another task.

## Common errors

| Status | What to check |
| --- | --- |
| 400 | Request JSON, parameters, and instruction length. |
| 401 | Missing, expired, revoked, or invalid API key. |
| 403 | Key is missing the endpoint's scope. |
| 404 | Resource is unavailable to the key's owner. |
| 422 | Approved-site rules, subscription, task limits, or a busy window. |
| 429 | Request rate limit; slow down before retrying. |

Error responses use `error.code` and `error.message`. For support, include the response's `x-request-id`, the endpoint, and a sanitized description. Exclude Authorization headers, account records, session data, and personal information.

## Limits and next steps

REST and MCP share request limits, while browser usage and AI allowances are shared with the app. No API key or tool can approve or execute a trade. Do not put trading passwords or one-time codes into task instructions.

Continue with [API & MCP integration](INTEGRATIONS.md), the [hosted API reference](https://taken.trade/docs/api-reference), and [rate limits](https://taken.trade/docs/rate-limits).
