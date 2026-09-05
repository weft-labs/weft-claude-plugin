# weft-claude-plugin

Official Weft plugin for Claude Code. Read [README](README.md) for installation,
setup, validation and releases.

- `.claude-plugin/` owns plugin metadata; `commands/` owns plugin-specific
  workflows; `.mcp.json` owns the bundled MCP connection.
- `skills/weft/` is a byte-identical upstream mirror pinned by `SKILLS_REF`.
  Change the canonical skills repo first; do not patch the vendored mirror.
- Run `bash tests/plugin_test.sh` for plugin behavior changes. README owns the
  strict plugin validation and release procedure.
- Keep temporary credentials in private plugin data, never tracked files.
- Do not change customer payment approval or spending limits during doc cleanup.

## Workflow and context

Use an isolated Git worktree for changes. Product changes use
PRs based on `main`; native stack layers target their parent. Keep configured
hooks enabled and required checks passing. Patrick owns the merge gate.

Start with [README.md](README.md) for setup, usage, and documentation.
