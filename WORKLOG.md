## 2026-05-30 - Apple Notes marketplace metadata refresh

**What changed**: Merged upstream Apple Notes MCP `1.4.2` into the Ames fork, preserving the fork-specific GitHub runtime command. Added Codex marketplace version metadata, the real macOS Notes app icon at `codex/assets/icon.png`, and Codex `composerIcon`, `logo`, and `screenshots` display fields. Filled out the Claude plugin and marketplace display metadata (`displayName`, homepage, repository, license, keywords). Added Codex install instructions to the README and added `scripts/sync-plugin-version.mjs` so future `npm version` runs update both Claude and Codex marketplace manifests.

**Decisions made**: Kept Claude Code configured on the upstream `apple-notes-mcp` marketplace rather than switching user-level Claude settings to this fork during wrap-up. Codex remains on the public `apple-notes-mcp-ames` marketplace and now sees `apple-notes@apple-notes-mcp-ames` at `1.4.2`.

**Left off at**: Committed and pushed merge commit `7f64719` (`feat: refresh Apple Notes marketplace metadata`). Codex marketplace snapshot was upgraded from GitHub, and the installed Codex cache includes the new icon/display metadata.

**Open questions**: Consider whether Claude Code should also switch from upstream `apple-notes-mcp` to `apple-notes-mcp-ames`; this touches protected user-level plugin settings and was intentionally not changed here.

**Verification**:
- `claude plugin validate .` passed for the marketplace.
- `npm run typecheck`, `npm test` (327 passed), `npm run build`, `npm run lint`, and `node --check scripts/sync-plugin-version.mjs` passed.
- The pre-push hook reran typecheck and tests successfully.
- Custom Codex metadata scan passed for source, upgraded marketplace snapshot, and installed cache.
- `codex mcp list` shows `apple-notes` using `github:oliverames/apple-notes-mcp-ames`.

---

## 2026-05-27 - Codex marketplace runtime fork

**What changed**: Added and refined Codex marketplace packaging for the Ames fork, then changed both Claude and Codex MCP manifests so runtime execution uses `npx -y github:oliverames/apple-notes-mcp-ames` instead of the upstream npm package.

**Decisions made**: The fork is the runtime source of truth for Codex. Codex plugin packaging uses a root `codex/.mcp.json` referenced from `.codex-plugin/plugin.json`, because that shape passes the local Codex plugin validator and still installs cleanly through the GitHub marketplace.

**Left off at**: `main` was pushed at `ca8ebd3`, Codex marketplace `apple-notes-mcp-ames` was upgraded from GitHub, `codex mcp list` showed `apple-notes` enabled with the Ames GitHub runtime, and a direct MCP stdio probe returned 23 tools.

**Open questions**: Apple Notes tools were not hot-loaded into the active Codex conversation after install; verify in a fresh Codex session that ToolSearch exposes the connector tools.

---
