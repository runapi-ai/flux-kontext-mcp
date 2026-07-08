<h1 align="center">RunAPI Flux Kontext MCP Server</h1>

<p align="center">
  <strong>Flux Kontext API access for AI agents: create image generation tasks, poll results, and check pricing through one focused MCP server.</strong>
</p>

<p align="center">
  <sub>Works with Claude Code, Codex, Cursor, Windsurf, VS Code, Roo Code, and any MCP-compatible host.</sub>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@runapi.ai/flux-kontext-mcp"><img src="https://img.shields.io/npm/v/%40runapi.ai/flux-kontext-mcp?style=flat-square&color=blue" alt="npm version"></a>
  <a href="https://github.com/runapi-ai/flux-kontext-mcp"><img src="https://img.shields.io/badge/GitHub-runapi--ai%2Fflux--kontext--mcp-24292f?style=flat-square" alt="GitHub repository"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue?style=flat-square" alt="Apache-2.0 license"></a>
  <img src="https://img.shields.io/badge/Type-MCP_Server-blue?style=flat-square" alt="MCP Server">
  <img src="https://img.shields.io/badge/Models-2-16a34a?style=flat-square" alt="2 models">
</p>

<p align="center">
  <a href="#install">Install</a> |
  <a href="#tools">Tools</a> |
  <a href="#models">Models</a> |
  <a href="#agent-prompts">Agent Prompts</a> |
  <a href="#configuration">Configuration</a> |
  <a href="#links">Links</a>
</p>

---

## Why This Package?

`@runapi.ai/flux-kontext-mcp` is a focused Model Context Protocol server for the **Flux Kontext** model line on RunAPI.
It gives MCP-compatible assistants direct access to 1 endpoint and 2 model variants without loading the full RunAPI catalog.

Use this per-model server when an agent should stay scoped to Flux Kontext. Use [`@runapi.ai/mcp`](https://github.com/runapi-ai/mcp) when one assistant should discover every RunAPI model line.

---

## Install

Add it to Claude Code:

```bash
claude mcp add flux-kontext -s user -- npx -y @runapi.ai/flux-kontext-mcp
```

Use project scope when the server should be shared with a repository:

```bash
claude mcp add flux-kontext -s project -- npx -y @runapi.ai/flux-kontext-mcp
```

Codex, Cursor, Windsurf, VS Code, Roo Code, and other MCP hosts can use the same stdio command:

```json
{
  "mcpServers": {
    "flux-kontext": {
      "command": "npx",
      "args": ["-y", "@runapi.ai/flux-kontext-mcp"]
    }
  }
}
```

`check_pricing` works before sign-in. For task creation and status polling, ask your assistant to call the `login` tool. It opens a browser login and saves credentials to `~/.config/runapi/config.json`, the same file used by `runapi login`.
Headless and CI hosts can still set `RUNAPI_API_KEY` before starting the MCP host.

Ready-made examples are in [`examples/`](examples/) for Claude, Cursor, Windsurf, VS Code, and Roo Code.

---

## Tools

| Tool | Auth | Purpose |
|---|---|---|
| `text_to_image` | Yes | Create a Flux Kontext text to image task and optionally wait for a terminal status. Returns the task id, status, output URLs, and pricing snapshot. |
| `get_task` | Yes | Fetch the current status and latest payload for an existing task. |
| `check_pricing` | No | Look up the current pricing snapshot for a Flux Kontext model and endpoint. |

---

## Models

Flux Kontext covers 2 model variants across 1 endpoint. Each tool accepts the models listed for it:

| Tool | Models |
|---|---|
| `text_to_image` | `flux-kontext-max`, `flux-kontext-pro` |

Model availability can change between releases. Use `check_pricing` or the [Flux Kontext model page](https://runapi.ai/models/flux-kontext) for the current catalog view.

---

## Agent Prompts

Ask your assistant in natural language; it can inspect pricing, create the task, and return the task id plus output URLs.

### Create a task

```text
Run a Flux Kontext text to image task with RunAPI.
```

The assistant can call `check_pricing`, then `text_to_image`, and return the task id, status, and output URLs.

### Submit without waiting

```text
Create the task but don't wait for it to finish.
```

The assistant calls the create tool with `wait: false` and returns the task id. Check on it later with `get_task`.

### Check pricing before creating

```text
Check current Flux Kontext pricing, then create the task if it matches my request.
```

The assistant calls `check_pricing` and can link to the [Flux Kontext model page](https://runapi.ai/models/flux-kontext) for the canonical catalog entry.

---

## Configuration

The server resolves auth in this order:

1. `RUNAPI_API_KEY` environment variable, useful for headless and CI hosts
2. `~/.config/runapi/config.json`, created by the MCP `login` tool or `runapi login`
3. No key, which still allows `check_pricing`

The config file is normally managed by login. A pre-provisioned headless config can use:

```json
{
  "apiKey": "your_runapi_key"
}
```

Do not commit real API keys.

---

## Links

| Resource | URL |
|---|---|
| Flux Kontext model page | [https://runapi.ai/models/flux-kontext](https://runapi.ai/models/flux-kontext) |
| npm package | [@runapi.ai/flux-kontext-mcp](https://www.npmjs.com/package/@runapi.ai/flux-kontext-mcp) |
| GitHub repository | [runapi-ai/flux-kontext-mcp](https://github.com/runapi-ai/flux-kontext-mcp) |
| RunAPI MCP overview | [runapi.ai/mcp](https://runapi.ai/mcp) |
| RunAPI docs | [runapi.ai/docs](https://runapi.ai/docs) |

---

## License

Licensed under the [Apache License, Version 2.0](LICENSE).
