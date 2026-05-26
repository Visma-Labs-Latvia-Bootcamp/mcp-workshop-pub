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
- [ ] You know how to create a `.mcp.json` file in a folder and open Claude Code there
- [ ] You can restart Claude Code after editing the config
- [ ] You can run a simple Node script from your terminal
- [ ] You have a writable folder to keep workshop projects in
- [ ] **You've downloaded [`dad-jokes-mcp.zip`](./dad-jokes-mcp.zip) from this repo** (used on Day 1 — extract anywhere, **do not run npm**)

If any of these fails, fix it before Day 1 — we won't have time to troubleshoot installs in the session.

---

## Where MCP config lives in Claude Code

Claude Code supports MCP servers at **three scopes**. Worth knowing now so the rest of the workshop makes sense:

| Scope | Where it's stored | Who sees it |
|---|---|---|
| **Project** | `.mcp.json` at the project root | Shared with your team via git. Claude Code prompts you to approve servers the first time you open the project. |
| **Local** *(default of `claude mcp add`)* | A per-project entry in `~/.claude.json` | Just you, just this project. Good for personal dev or secrets you don't want in git. |
| **User** | A user-level entry in `~/.claude.json` | Just you, but available across **all** your projects. Good for personal utility MCPs you want everywhere. |

You can add at any scope via the CLI: `claude mcp add --scope project|local|user`. Precedence when the same name appears in multiple scopes is **local → project → user**.

**For this workshop we use the project scope (`.mcp.json`)**: same config for everyone, easy to inspect on disk, and the approval prompt is a nice teaching moment about MCP security.

> Want one of these MCPs available across **all** your projects after the workshop? Add it at user scope: `claude mcp add --scope user dad-jokes node "C:/path/to/dad-jokes-mcp/build/index.js"`.

---

## Day 1 — Hands-On Snippets

During Day 1 you'll create a `.mcp.json` in a workshop folder and open Claude Code there. Both MCP servers (DeepWiki + Dad Jokes) end up in that same file side-by-side. The snippets below are what you'll paste — you don't need to type them in advance.

> Pick (or create) any folder you want to be your "workshop folder", e.g. `C:\workshop\mcp-day1\` or `~/workshop/mcp-day1/`. `.mcp.json` goes in that folder's root. Claude Code will prompt you to approve the servers the first time you open the project.

### 1. Remote MCP — DeepWiki (just a URL)

Create `.mcp.json` in your workshop folder with:

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

Restart Claude Code (and approve the server when prompted), then try:

- *"Using deepwiki, summarize what the modelcontextprotocol/typescript-sdk repo does."*
- *"Using deepwiki, what does the README of facebook/react say about strict mode?"*

### 2. Local MCP — Dad Jokes (pre-built — just a path)

1. Extract [`dad-jokes-mcp.zip`](./dad-jokes-mcp.zip) anywhere on disk (e.g. `C:\Users\you\dad-jokes-mcp\` or `~/dad-jokes-mcp/`). It does **not** need to live inside your workshop folder.
2. Note the **absolute path** to `build/index.js` inside the extracted folder.
3. Add `dad-jokes` to the same `.mcp.json` you created above, alongside `deepwiki`:

```json
{
  "mcpServers": {
    "deepwiki": {
      "type": "http",
      "url": "https://mcp.deepwiki.com/mcp"
    },
    "dad-jokes": {
      "type": "stdio",
      "command": "node",
      "args": ["C:/Users/your-name/dad-jokes-mcp/build/index.js"]
    }
  }
}
```

- macOS / Linux path example: `/Users/your-name/dad-jokes-mcp/build/index.js`
- On Windows, use **forward slashes** or **double-backslashes** in JSON paths.

Restart Claude Code (approve the new server when prompted), then try:

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
