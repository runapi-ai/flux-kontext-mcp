# @runapi.ai/flux-kontext-mcp

RunAPI MCP server for the **Flux Kontext** model line. Create image tasks,
poll their status, and check pricing through a single RunAPI API key.

## Tools

- `text_to_image` — create a Flux Kontext task and (optionally) poll until it
  reaches a terminal status. Returns the task id, status, output URLs, and a
  price snapshot. Models: `flux-kontext-max`, `flux-kontext-pro`.
- `get_task` — fetch the current status and latest payload for a task.
- `check_pricing` — look up pricing for a model in this line.

## Configuration

Set a RunAPI API key via the `RUNAPI_API_KEY` environment variable, or write
it to `~/.config/runapi/config.json`:

```bash
mkdir -p ~/.config/runapi
echo '{"api_key":"YOUR_KEY"}' > ~/.config/runapi/config.json
```

Get an API key at https://runapi.ai. Pricing is listed at
https://runapi.ai/pricing.

## Usage

Run the server over stdio:

```bash
npx -y @runapi.ai/flux-kontext-mcp
```

Add it to an MCP client (see `examples/` for per-client configs):

```json
{
  "mcpServers": {
    "flux-kontext": {
      "command": "npx",
      "args": ["-y", "@runapi.ai/flux-kontext-mcp"],
      "env": { "RUNAPI_API_KEY": "${RUNAPI_API_KEY}" }
    }
  }
}
```

## License

Apache-2.0
