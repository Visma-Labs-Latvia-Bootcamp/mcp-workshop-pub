# Pick A — Dad Jokes MCP (calls an external API)

> Day 2 vibe-coding spec. You'll **prompt Claude Code to build this for you** — you're not following a step-by-step recipe.

## What you're building

A small MCP server that exposes one or two tools backed by the free [icanhazdadjoke.com](https://icanhazdadjoke.com/) API (no API key needed).

## Tools

| Tool | Input | Behavior |
|---|---|---|
| `random-joke` | — | Calls `GET https://icanhazdadjoke.com/` with header `Accept: application/json` and returns the `joke` field. |
| `search-jokes` *(optional second tool)* | `query: string` | Calls `GET https://icanhazdadjoke.com/search?term=<query>` with the same header and returns up to 5 jokes. |

## Tech to use

- **Language:** TypeScript
- **SDK:** `@modelcontextprotocol/sdk`
- **Schema:** `zod@3`
- **Transport:** `stdio`
- **Module type:** ESM (`"type": "module"` in `package.json`)
- **Build:** `tsc`, compiling `src/` → `build/`

## Starter prompt

Open Claude Code in an empty folder and paste this:

> Build me an MCP server in TypeScript that exposes one tool called `random-joke`. The tool calls `https://icanhazdadjoke.com/` with the header `Accept: application/json` and returns the `joke` field from the response. Use the `@modelcontextprotocol/sdk` package, stdio transport, and `zod@3` for the input schema. Set up `package.json` with `"type": "module"` and a `build` script using `tsc`. Create a minimal `tsconfig.json` that compiles `src/` to `build/`. Don't run the build yet — just write the files.

## Likely follow-up prompts

- *"Now run `npm install` and `npm run build`. Fix any errors."*
- *"Add a second tool called `search-jokes` that takes a `query` string and calls `https://icanhazdadjoke.com/search?term=<query>` and returns up to 5 jokes."*
- *"The tool description is too vague — rewrite it so an AI client knows exactly when to use it."*

## Register your server

Once it builds, ask Claude Code:

> Help me register this MCP in my `.claude/settings.json`. Use the absolute path to `build/index.js`.

The entry should look like:

```json
{
  "mcpServers": {
    "dad-jokes": {
      "command": "node",
      "args": ["<absolute-path-to>/dad-jokes-mcp/build/index.js"]
    }
  }
}
```

Then restart Claude Code.

## Test it

- *"Tell me a dad joke."*
- *"Find me a joke about coffee."*
- *"Search for jokes about computers."*

## What "done" looks like

- Claude Code lists `dad-jokes` tools after restart
- `random-joke` returns a string with a joke
- (Optional) `search-jokes` returns multiple jokes for a query
- You can describe in one sentence what each piece of the server does

## If you get stuck

- Try **MCP Inspector** to confirm the server itself works (separate from Claude Code):
  ```bash
  npx @modelcontextprotocol/inspector node build/index.js
  ```
- Ask Claude Code to explain whichever piece is confusing — that's part of the exercise.
- Flag the presenter; they have a reference solution to screen-share.
