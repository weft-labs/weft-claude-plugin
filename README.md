# weft-claude-plugin

The official Weft plugin for Claude Code. Search the agent web and pay any
x402/MPP endpoint from your Weft wallet — without leaving the conversation.

## What it does

| Component | Invocation | Purpose |
|---|---|---|
| Skill `weft` | model-invoked | Teaches Claude the balance → search → paid-fetch loop, receipt reading, and error handling |
| Command `/weft:balance` | `/weft:balance` | Wallet balance + spending-policy snapshot |
| Command `/weft:find-api` | `/weft:find-api <need>` | Discover paid endpoints for a task, then fetch with your approval |
| MCP connector | bundled | Points at the hosted Weft MCP server (`https://weft.network/mcp`) |

## Install

From a marketplace (once listed in the Anthropic community marketplace):

```
/plugin install weft@claude-plugins-community
```

Or directly from this repo:

```
/plugin marketplace add weft-labs/weft-claude-plugin
/plugin install weft@weft-labs
```

## First run

The plugin bundles the Weft MCP server. On first tool use, Claude redirects
you to Weft to sign in once — no keys to paste. The connection grant appears
under **Settings → Connections** and is revocable at any time.

New to Weft? Create an account at [weft.network](https://weft.network), then
retry the tool and sign in through the first-use browser flow. A new account
gets a buyer wallet with no promotional balance, free credit, or subsidy, so
fund it before the first paid fetch. API reference:
[weft.network/docs](https://weft.network/docs).

## Cost discipline

Paid fetches spend real USDC from your wallet, always inside your spending
policy. The skill enforces a balance check before paid actions, tight
`max_cost_usd` ceilings, and explicit user approval before the first paid
call in `/weft:find-api`.

## License

MIT — see [LICENSE](LICENSE).
