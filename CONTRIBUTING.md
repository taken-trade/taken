# Contributing

Thanks for helping improve Taken's public documentation and integration guides.

This repository contains product information and request examples. The hosted application's implementation and operational infrastructure are maintained privately.

## Useful contributions

- Fix unclear explanations, broken links, or incorrect request examples.
- Report differences between the documented API and observed behavior.
- Share a reproducible integration issue using a test account and sanitized data.
- Suggest documentation or workflows that would help developers get started.

For product questions, partnerships, or investment discussions, contact [contact@taken.trade](mailto:contact@taken.trade).

## Report an issue

Include the affected guide or endpoint, what you expected, what happened, and minimal reproduction steps. For an API problem, include the HTTP status, error code, and `x-request-id` when available. Mention the client and version for MCP problems.

Remove Authorization headers, API keys, passwords, cookies, personal information, account records, and private session output before posting. Use a test account and example.com where possible.

**Suspected vulnerabilities go through [SECURITY.md](SECURITY.md), not a public issue.**

## Propose a documentation change

Keep the change focused. Link to the current hosted documentation when correcting endpoint behavior. Mark untested client setup explicitly, and distinguish released capabilities from plans.

Before submitting:

- Check Markdown rendering and relative links.
- Use placeholders or local environment variables for credentials.
- Verify endpoint names, scopes, and request shapes against the [API reference](https://taken.trade/docs/api-reference).
- Do not add private application code, production configuration, internal plans, or customer data.
- Do not describe live execution, client compatibility, or security guarantees as available without verification.

Documentation examples should not execute financial actions or automatically run against a user's account during checks. Any live task test must be intentional and account for subscription usage.
