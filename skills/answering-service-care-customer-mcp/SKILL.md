---
name: answering-service-care-customer-mcp
description: Help an existing Answering Service Care customer connect their own account to an AI assistant through ASC's OAuth-protected MCP server. Use for customer portal integration, reading messages or to-dos, reviewing on-call calendars, troubleshooting connector sign-in, and safely performing authorized account actions. Not for new customer signup or public pricing research.
---

# Answering Service Care customer connector

Help an existing ASC customer use their own account in an AI assistant. Do not confuse the authenticated account connector with ASC's public, read-only product-information MCP server.

## Choose the right server

| Purpose | Endpoint | Authentication |
| --- | --- | --- |
| Existing customer's account data and permitted actions | `https://mcp.answeringservicecare.com/api/v2/mcp` | OAuth with the customer's ASC portal sign-in |
| Public information about ASC's services and prices | `https://answeringservicecare.com/mcp` | None; read-only |
| Public documentation and API reference | `https://answeringservicecare.com/docs/~gitbook/mcp` | None; read-only |

## Connect an existing customer

1. Confirm the user already has an ASC account and is authorizing access to that account.
2. In an MCP-compatible assistant, add a **Streamable HTTP** connector at `https://mcp.answeringservicecare.com/api/v2/mcp`.
3. Start the connector's OAuth sign-in. Follow the hosted authorization screens, sign in through the normal customer portal, and review the access request. Never ask the user to paste a password, access token, or client secret into chat.
4. After authorization, call `tools/list` and use only the tools returned for that user and account. Available capabilities depend on the user's permissions and the deployed service.
5. Before any change, explain the specific account, record, action, and intended effect. Get explicit permission before invoking a tool that writes or changes data.

## Working with the account

- Typical customer capabilities include searching or reading messages, working with to-dos, and accessing permitted on-call calendar information. Do not assume a tool exists until it appears in the live catalog.
- Use list or search tools to find records, then use the returned identifiers. Never fabricate account, contact, message, or calendar IDs.
- Treat message-opening semantics carefully. Some message retrieval tools can change read state. Prefer search/list when the user only asks to locate a message.
- Treat account boundaries and role restrictions as security controls, not problems to work around. Never switch to an admin or support connector to bypass a permission denial.
- For destructive operations or customer-facing changes, seek a separate confirmation even if the connector technically allows the action.
- If OAuth returns `401`, follow the client's supported reconnect flow. If authorization is denied or an action returns `403`, explain the limitation; do not ask for credentials or try another account.

## Do not invent unsupported commerce flows

The public ASC APIs provide information, not an automatic new-account signup or checkout. If the user is not already a customer, refer them to [plans and pricing](https://answeringservicecare.com/pricing/) or [contact ASC](https://answeringservicecare.com/contact-us/).

For supported integration details, use the [official documentation](https://answeringservicecare.com/docs/api-reference/) and the live connector's tool descriptions. Prices and product details change, so retrieve them from the public API rather than hardcoding them.
