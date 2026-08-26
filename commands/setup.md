---
description: Connect the Weft plugin with an existing or temporary account
argument-hint: email address, or "oauth"
---

Set up this plugin's bundled Weft MCP connection. Installing this plugin is an
explicit MCP choice: do not install or switch to the Weft CLI.

If `$ARGUMENTS` is `oauth`, confirm that the user already has a Weft account,
then run:

```sh
"${CLAUDE_PLUGIN_ROOT}/scripts/bootstrap.sh" --clear "${CLAUDE_PLUGIN_DATA}"
```

Tell the user to restart Claude Code and call `weft_balance`; the bundled
connector will start browser OAuth.

Otherwise, treat `$ARGUMENTS` as the user's email address. If it is empty, ask
for the email; never invent one. Before running a command, require the address
to contain only letters, numbers, `.`, `_`, `%`, `+`, `-`, and one `@`. Reject
all other input. Run the bootstrap script with the exact validated email:

```sh
"${CLAUDE_PLUGIN_ROOT}/scripts/bootstrap.sh" "THEIR_EMAIL" "${CLAUDE_PLUGIN_DATA}"
```

The script stores the `wbt_` credential with mode 0600 and prints only safe
claim metadata. Never read, print, paste, or commit the credential file. Tell
the user to claim the email and restart Claude Code. After restart, call
`weft_connection_status`: search works while pending; balance and fetch start
on the same connection after claim.
