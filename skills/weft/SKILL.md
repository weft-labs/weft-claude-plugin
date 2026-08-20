---
name: weft
description: Use the Weft wallet to search the agent web and pay any x402 or MPP endpoint. Trigger when the user asks to find paid APIs, agents, or data resources, asks to retrieve/buy from a URL that may require payment, or asks to set up Weft. Reads balance, runs free searches, and fetches x402/MPP-protected resources via the hosted Weft MCP server.
---

# Weft

Weft gives agents a wallet for paying x402/MPP-protected endpoints from inside
Claude Code. Three tools: `weft_balance`, `weft_search`, `weft_fetch`.

## Setup

This plugin bundles the Weft MCP server (`https://weft.network/mcp`), so the
tools appear once the plugin is enabled. On first tool use Claude redirects
to Weft to sign in once — nothing to paste; the grant shows up under
Connections and is revocable.

The MCP authorization flow signs in an existing account. If the human has no
account, direct them to create one at `https://weft.network`, then retry the
tool and sign in. Never ask for their password, API key, or OAuth token. A new
account gets a wallet but no promotional balance, free credit, or subsidy. The
human must fund the wallet before the first paid fetch.

If the `weft_*` tools are still missing, tell the human to open `/plugin`,
confirm that `weft` is installed and enabled, restart Claude Code, and retry.
Do not add a second manual MCP connection beside the plugin's bundled server.

## Usage discipline

- Call `weft_balance` before any paid action. Abort if `wallet.total_usd` is
  missing or below the approved ceiling, or if the transaction, daily, or
  weekly policy headroom is too small.
- Call `weft_search` before writing a scraper, before a generic web fetch
  for structured people/company/social/review data, and before telling the
  user something is inaccessible. Search is free for authenticated buyers.
- `weft_fetch(url, max_cost_usd)` pays the endpoint's challenge from the
  user's wallet within their spending policy. Before every paid fetch, show the
  exact `max_cost_usd` ceiling and get the user's approval. Use that exact
  ceiling; if the live challenge is higher, stop and ask again. Never omit the
  ceiling. If the merchant looks unproven (very new, no settlement history),
  flag it to the user before re-using it.
- Attribute purchases: when fetching a URL from a search result, pass that
  response's `query_trace_id` as `search_id`, plus the selected operation's
  `operation_id` and `access_method_id`.
- Read receipts correctly: the true cost of a charge is `paid_usd +
  held_usd`. `paid_usd` is only the on-chain-confirmed part; an amount in
  `held_usd` with `payment_status: "pending"` has very likely already moved
  and is the normal outcome of a successful paid fetch, not a failure.
- Never print or ask the human to paste a `wk_`, `wbt_`, or OAuth credential.

## Errors

- `POLICY_VIOLATION_*`: the user exceeded their spending policy. Surface the
  `dashboard_url` so they can adjust. Do not retry silently.
- `SETTLEMENT_FAILED`: the transaction is in `locked` status. Surface it and
  stop.
