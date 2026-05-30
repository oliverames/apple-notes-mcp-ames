## 2026-05-30 - Apple Notes marketplace metadata refresh

**What changed**: Merged upstream Apple Notes MCP `1.4.2` into the Ames fork, preserving the fork-specific GitHub runtime command. Added Codex marketplace version metadata, the real macOS Notes app icon at `codex/assets/icon.png`, a first-party `codex/assets/screenshot.svg` marketplace preview, and Codex `composerIcon`, `logo`, and `screenshots` display fields. Filled out the Claude plugin and marketplace display metadata (`displayName`, homepage, repository, license, keywords). Added Codex install instructions to the README and added `scripts/sync-plugin-version.mjs` so future `npm version` runs update both Claude and Codex marketplace manifests.

**Decisions made**: Switched Claude Code's protected user-level plugin settings to the public Ames fork after Oliver approved the protected config update. Claude and Codex now both use the `apple-notes-mcp-ames` marketplace. Patch-bumped the package and plugin metadata to `1.4.3` so existing installs pick up the new screenshot asset through the normal marketplace update path.

**Left off at**: Apple Notes is ready to ship as `1.4.3`. Codex marketplace snapshot should be upgraded from GitHub, Claude marketplace settings now point at `oliverames/apple-notes-mcp-ames`, and the installed Codex cache should include the new icon/display metadata after refresh.

**Open questions**: None.

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
