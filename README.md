# MCP Workshop — Attendee Materials

Welcome! This repo contains everything you need to follow along with the **two-day MCP workshop**.

- **Day 1** — Theory + plugging in two existing MCP servers (one remote, one local)
- **Day 2** — Plugging in two more MCPs and then **vibe-coding your own** by prompting Claude Code

You don't need to read or build anything in advance. Just complete the [Preflight](#preflight) checklist below before Day 1 starts.

---

## Preflight

Verify these on your laptop **before Day 1**. Everything else is done in the session.

- [ ] [Node.js](https://nodejs.org/) v18 or newer — check with `node --version`
- [ ] `npm --version` and `npx --version` both work
- [ ] [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed — check with `claude --version`
- [ ] You know where Claude Code reads your local `.claude/settings.json`
- [ ] You can restart Claude Code after editing settings
- [ ] You can run a simple Node script from your terminal
- [ ] You have a writable folder to keep workshop projects in
- [ ] **You've downloaded [`dad-jokes-mcp.zip`](./dad-jokes-mcp.zip) from this repo** (used on Day 1 — extract anywhere, **do not run npm**)

If any of these fails, fix it before Day 1 — we won't have time to troubleshoot installs in the session.

---

## Day 1 — Hands-On Snippets

You'll add two MCP servers to your `.claude/settings.json` during Day 1. Both end up side-by-side. The snippets below are what you'll paste — you don't need to type them in advance.

### 1. Remote MCP — DeepWiki (just a URL)

```json
{
  "mcpServers": {
    "deepwiki": {
      "type": "http",
      "url": "https://mcp.deepwiki.com/mcp"
    }
  }
}
```

Restart Claude Code, then try:

- *"Using deepwiki, summarize what the modelcontextprotocol/typescript-sdk repo does."*
- *"Using deepwiki, what does the README of facebook/react say about strict mode?"*

### 2. Local MCP — Dad Jokes (pre-built — just a path)

1. Extract [`dad-jokes-mcp.zip`](./dad-jokes-mcp.zip) anywhere on disk (e.g. `C:\Users\you\dad-jokes-mcp\` or `~/dad-jokes-mcp/`).
2. Note the **absolute path** to `build/index.js` inside the extracted folder.
3. Add `dad-jokes` to your `.claude/settings.json` alongside `deepwiki`:

```json
{
  "mcpServers": {
    "deepwiki": {
      "type": "http",
      "url": "https://mcp.deepwiki.com/mcp"
    },
    "dad-jokes": {
      "command": "node",
      "args": ["C:\\Users\\your-name\\dad-jokes-mcp\\build\\index.js"]
    }
  }
}
```

- macOS / Linux path example: `/Users/your-name/dad-jokes-mcp/build/index.js`
- On Windows, use **forward slashes** or **double-backslashes** in JSON paths.

Restart Claude Code, then try:

- *"Tell me a dad joke."*
- *"Find me a joke about coffee."*

> **Don't run `npm install` or `npm run build` on the zip** — it's already built. Day 2 is when you'll build one yourself.

---

## Day 2 — Specs You Can Pick From

Day 2's main exercise is **vibe coding** — you'll prompt Claude Code to build a small MCP server, then register and use it live.

Pick **one** of these specs going in (or change your mind on the day):

- [`day2-specs/pick-A-dad-jokes.md`](./day2-specs/pick-A-dad-jokes.md) — calls an external API
- [`day2-specs/pick-B-sticky-notes.md`](./day2-specs/pick-B-sticky-notes.md) — persistent state in a local JSON file

Both are time-boxed to ~30 minutes. Goal: **one working MCP per person**, not breadth.

---

## Resources

- [MCP Specification](https://modelcontextprotocol.io/)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Official MCP server directory](https://github.com/modelcontextprotocol/servers)
