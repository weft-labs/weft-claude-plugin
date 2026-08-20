---
description: Check the Weft wallet balance and spending policy
---

Check the user's Weft wallet now with `weft_balance`, then report:

- Available USDC (and promo credit, if any)
- Spending-policy caps and spent-today / spent-week totals
- Headroom: how many typical paid fetches the remaining balance covers

If the balance is low relative to a likely upcoming spend, say so and point
at the dashboard URL for top-ups. If the `weft_*` tools are not available,
the Weft connector is not connected yet — tell the user to run `/plugin` and
check that the weft plugin is installed and enabled, then sign in on first
tool use.
