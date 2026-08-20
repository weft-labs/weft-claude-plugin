---
description: Find paid APIs and data endpoints for a task via Weft search
argument-hint: what you need (e.g. "work emails for YC AI founders")
---

Search the agent web for paid endpoints matching: $ARGUMENTS

1. Call `weft_search` with a precise query derived from the request above.
   Narrow with structured `filters` when the user named a budget, resource
   type, or payment protocol.
2. Present the top results as a table: name, price per call, type, protocol,
   and one line on what it returns.
3. Recommend one result and say why. Quote its exact price.
4. Ask the user before paying. On approval, call `weft_fetch` on the result
   URL with a tight `max_cost_usd` (the quoted price plus a small margin,
   never more), passing `search_id`, `operation_id`, and `access_method_id`
   from the search response for attribution.
5. Summarize what came back and what it cost (`paid_usd + held_usd`).

Never guess endpoint paths — only fetch URLs returned by `weft_search`. If no
result fits, say so instead of falling back to scraping.
