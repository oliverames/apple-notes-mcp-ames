## 2026-05-27 - Codex marketplace runtime fork

**What changed**: Added and refined Codex marketplace packaging for the Ames fork, then changed both Claude and Codex MCP manifests so runtime execution uses `npx -y github:oliverames/apple-notes-mcp-ames` instead of the upstream npm package.

**Decisions made**: The fork is the runtime source of truth for Codex. Codex plugin packaging uses a root `codex/.mcp.json` referenced from `.codex-plugin/plugin.json`, because that shape passes the local Codex plugin validator and still installs cleanly through the GitHub marketplace.

**Left off at**: `main` was pushed at `ca8ebd3`, Codex marketplace `apple-notes-mcp-ames` was upgraded from GitHub, `codex mcp list` showed `apple-notes` enabled with the Ames GitHub runtime, and a direct MCP stdio probe returned 23 tools.

**Open questions**: Apple Notes tools were not hot-loaded into the active Codex conversation after install; verify in a fresh Codex session that ToolSearch exposes the connector tools.

---
