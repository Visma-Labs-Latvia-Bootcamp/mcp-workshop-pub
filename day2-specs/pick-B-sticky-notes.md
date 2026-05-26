# Pick B — Sticky Notes MCP (persistent state, multiple tools)

> Day 2 vibe-coding spec. You'll **prompt Claude Code to build this for you** — you're not following a step-by-step recipe.

## What you're building

A small MCP server that does CRUD on notes, persisted to a local `notes.json` file in the project folder. No external APIs.

## Tools

| Tool | Input | Behavior |
|---|---|---|
| `add-note` | `title: string`, `content: string` | Append a note with a generated id and `createdAt` timestamp to `notes.json`. |
| `list-notes` | — | Return all notes from `notes.json` (or an empty list if none yet). |
| `search-notes` | `query: string` | Return notes whose title or content contains the query (case-insensitive). |
| `delete-note` | `id: string` | Remove the note with that id from `notes.json`. |

## Tech to use

- **Language:** TypeScript
- **SDK:** `@modelcontextprotocol/sdk`
- **Schema:** `zod@3`
- **Transport:** `stdio`
- **Module type:** ESM (`"type": "module"` in `package.json`)
- **Build:** `tsc`, compiling `src/` → `build/`
- **Storage:** plain `notes.json` in the project root (no database)

## Starter prompt

Open Claude Code in an empty folder and paste this:

> Build me an MCP server in TypeScript with these tools: `add-note(title, content)`, `list-notes()`, `search-notes(query)`, `delete-note(id)`. Persist notes to a local `notes.json` file in the project root. Use `@modelcontextprotocol/sdk`, stdio transport, and `zod@3`. Set up `package.json` with `"type": "module"` and a `build` script. Create a minimal `tsconfig.json` that compiles `src/` to `build/`. Don't run the build yet — just write the files.

## Likely follow-up prompts

- *"Now run `npm install` and `npm run build`. Fix any errors."*
- *"Notes need an `id` and a `createdAt` timestamp — use `crypto.randomUUID()` for the id."*
- *"Make `search-notes` case-insensitive and match against both title and content."*
- *"The tool descriptions are too vague — rewrite them so an AI client knows exactly when to use each one."*

## Register your server

Once it builds, ask Claude Code:

> Help me register this MCP in my `.claude/settings.json`. Use the absolute path to `build/index.js`.

The entry should look like:

```json
{
  "mcpServers": {
    "sticky-notes": {
      "command": "node",
      "args": ["<absolute-path-to>/sticky-notes-mcp/build/index.js"]
    }
  }
}
```

Then restart Claude Code.

## Test it

- *"Save a note titled 'Workshop' with content 'I built my first MCP today'."*
- *"What notes do I have?"*
- *"Search my notes for 'workshop'."*
- *"Delete the workshop note."*

## What "done" looks like

- Claude Code lists `sticky-notes` tools after restart
- Adding a note creates / updates `notes.json` in the project folder
- Listing returns what you've added; searching filters correctly; deleting removes the right note
- You can describe in one sentence what each piece of the server does

## If you get stuck

- Try **MCP Inspector** to confirm the server itself works (separate from Claude Code):
  ```bash
  npx @modelcontextprotocol/inspector node build/index.js
  ```
- Ask Claude Code to explain whichever piece is confusing — that's part of the exercise.
- Flag the presenter; they have a reference solution to screen-share.
