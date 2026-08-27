---
name: sparkyfitness-connect-mcp-agent
description: Connect an AI agent to a SparkyFitness instance over MCP and use its 56 health tools safely, including the admin tools that bypass row-level security.
api: SparkyFitness MCP Server
generated: '2026-08-27'
method: generated
source: https://codewithcj.github.io/SparkyFitness/features/mcp-server + mcp/sparkyfitness-mcp.yml
operations:
  - POST /mcp
mcp_tools:
  - sparky_manage_food
  - sparky_manage_exercise
  - sparky_manage_checkin
  - sparky_manage_goals
  - sparky_get_report
  - sparky_daily_checkin_wizard
---

# Connect an agent to SparkyFitness over MCP

SparkyFitness runs its MCP server **in-process inside the API server**. There is
no separate service to install, no npm package to run, and no vendor-hosted
endpoint: your endpoint is your own deployment.

## Endpoint

- Production: `https://<your-host>/mcp` — the production nginx config proxies
  `/mcp` to the server.
- Local dev: `http://localhost:8080/mcp` through the Vite dev proxy, or
  `http://localhost:3010/mcp` straight at the server port.

Transport is stateless streamable HTTP. A fresh MCP server and transport are
built per request, so nothing is carried between calls.

## Authenticate

Generate an API key in **Settings -> Developer & Integrations -> API Key
Management** and send it as a bearer token:

```
Authorization: Bearer <API_KEY>
```

HTTP-capable clients (Cursor, Open WebUI's "MCP Streamable HTTP" integration,
and others) point at the URL directly. stdio-only clients such as the classic
Claude Desktop config need a bridge — the docs use the third-party `mcp-remote`
package, with the key in an `env` block and the no-space
`Authorization:${AUTH_HEADER}` header form that works around clients mangling
spaces in header arguments. Add `--allow-http` only for a plain-HTTP local dev
server.

## Tool surface

56 tools. Two profiles, chosen by the user in their AI service settings:

- **`full`** (default) — every chat-visible tool.
- **`core`** — food, exercise, check-in and goals only. Intended for small local
  models with no prompt cache, which select more reliably from a smaller
  surface. **MCP honours this setting verbatim**, unlike the in-app chat path,
  because MCP clients pay the whole tool-list cost in their own context window
  on every call.

Marquee tools: `sparky_manage_food`, `sparky_manage_exercise`,
`sparky_manage_checkin`, `sparky_manage_goals`, `sparky_get_report`,
`sparky_daily_checkin_wizard`, `sparky_detect_patterns`,
`sparky_get_30_day_trends`. The full list with descriptions is in
`mcp/sparkyfitness-mcp.yml`; `mcp/sparkyfitness-tool-crosswalk.yml` binds each
one to the REST operations behind it.

## Rules

- **Tools honour the user's unit preferences.** lbs/kg and kcal/kJ conversion
  happens for you here and does **not** happen on the REST API. If you mix the
  two surfaces in one flow, you will mix unit systems.
- **Row-level security scopes every normal tool** to the user the API key
  authenticates. MCP deliberately scopes to the authenticated actor and not to
  a family-sharing delegation cookie, so an agent cannot be walked sideways into
  another family member's data.
- **Five admin tools bypass row-level security**: `sparky_execute_read_only_sql`,
  `sparky_query_table`, `sparky_inspect_schema`, `sparky_get_db_stats`,
  `sparky_get_user_info`. They run on the owner pool. They require BOTH
  `DEV_TOOLS_ENABLED=true` and an admin caller, and are gated at registration so
  a non-admin never sees them in `tools/list`. Leave them disabled unless you
  are actively debugging, and never point a general-purpose agent at an instance
  that has them on.
- **Send the negotiated protocol version.** The server clamps a well-formed
  version that post-dates its SDK down to its own rather than returning 400 —
  a courtesy for clients that send their newest version instead of the
  negotiated one. An unrecognised *older* version or a non-date value still
  gets a 400.
- **Omit optional fields; do not send `null`.** The server strips nulls from
  `tools/call` arguments before validation precisely because LLM clients send
  `"start_date": null`, which would otherwise fail Zod `.optional()` with MCP
  error `-32602`. Do not rely on that if you can simply omit the key.
- **Rate limit:** 100 requests per minute per API key by default, `429` with
  `Retry-After` on exhaustion. Every MCP call counts.
- **Privacy:** if the instance is self-hosted, the data does not leave the
  operator's infrastructure until the agent sends it to its own model provider.
  Say so before doing anything that ships health data outward.
