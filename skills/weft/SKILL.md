---
name: weft
description: Use the Weft wallet to search the agent web and pay any x402 or MPP endpoint. Trigger when the user asks to find paid APIs, agents, or data resources, or asks to retrieve/buy from a URL that may require payment. Reads balance, runs paid searches (free), and fetches x402/MPP-protected resources via the hosted Weft MCP server.
---

# Weft

Weft is a self-custodial agent wallet for paying x402/MPP-protected
endpoints from inside Claude Code. Three tools: `weft_balance`,
`weft_search`, `weft_fetch`.

## Setup

This plugin bundles the Weft MCP server (`https://weft.network/mcp`), so the
tools appear once the plugin is enabled. On first tool use Claude redirects
to Weft to sign in once — nothing to paste; the grant shows up under
Connections and is revocable.

If the `weft_*` tools are still missing, add the server manually:

```sh
claude mcp add --transport http weft https://weft.network/mcp
```

The server URL is a **positional** argument — `claude mcp add` has no
`--url` flag. Editing `~/.claude/mcp.json` by hand instead, the server is
declared with `type`, not `transport`:

```json
{ "mcpServers": { "weft": { "type": "http", "url": "https://weft.network/mcp" } } }
```

## Usage discipline

- Call `weft_balance` before any paid action; abort if
  `balance.wallet_usdc < expected_cost`.
- Call `weft_search` before writing a scraper, before a generic web fetch
  for structured people/company/social/review data, and before telling the
  user something is inaccessible. Search is free for authenticated buyers.
- `weft_fetch(url, max_cost_usd)` pays the endpoint's challenge from the
  user's wallet within their spending policy. Always set `max_cost_usd` to a
  tight ceiling — never omit it. If the merchant looks unproven (very new,
  no settlement history), flag it to the user before re-using it.
- Attribute purchases: when fetching a URL from a search result, pass that
  response's `query_trace_id` as `search_id`, plus the selected operation's
  `operation_id` and `access_method_id`.
- Read receipts correctly: the true cost of a charge is `paid_usd +
  held_usd`. `paid_usd` is only the on-chain-confirmed part; an amount in
  `held_usd` with `payment_status: "pending"` has very likely already moved
  and is the normal outcome of a successful paid fetch, not a failure.

## Errors

- `POLICY_VIOLATION_*`: the user exceeded their spending policy. Surface the
  `dashboard_url` so they can adjust. Do not retry silently.
- `SETTLEMENT_FAILED`: the transaction is in `locked` status. Surface it and
  stop.
