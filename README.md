# Torque360 MCP server for ChatGPT and Claude

Torque360 publishes no public API documentation or official MCP server, according to the Cotlet.dev page checked 17 November 2026. Cotlet.dev connects ChatGPT, Claude and other AI agents to your own signed-in Torque360 Cloud Browser for shop reports and setup. The page describes 16 hosted workflows: 10 read reports and 6 write tools whose changes ask for confirmation by default.

## What you can ask

> Close the day with one summary.

> Compare billed and clocked hours per mechanic.

> Check timesheets before payroll.

> Reconcile Torque Pay and PayPal payments.

> Prepare tax collected and profit for the month.

> Build an inspection template.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude

Settings > Connectors > Add custom connector: `https://cotlet.dev/mcp/v1/connect`. Sign in to Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Torque360 tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"torque360":{"command":"uvx","args":["--from","git+https://github.com/cotlet-connect/torque360-mcp","torque360-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t torque360-mcp .
docker run -i --rm -e COTLET_API_KEY torque360-mcp
```

Tools: `torque360_guide`, `torque360_status`, `torque360_open`, `torque360_read`, `torque360_click`, `torque360_type`, `torque360_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From this repository:

```bash
pip install git+https://github.com/cotlet-connect/torque360-mcp
torque360-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 torque360_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 torque360_agent.py status    # is Torque360 ready in your Cotlet.dev account
python3 torque360_agent.py guide     # Torque360 screens and sign-in link
python3 torque360_agent.py read      # open Torque360, read the page, close the browser
python3 torque360_agent.py tools     # list Cotlet.dev tools
python3 torque360_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Torque360 users

**Does Torque360 have an API or MCP server?**

The Cotlet.dev page checked 17 November 2026 found no public Torque360 API documentation, official MCP server or official ChatGPT or Claude connector. Torque360 lists packaged integrations, including QuickBooks, CARFAX, PartsTech, Nexpart and PayPal.

**How do I connect Torque360 to ChatGPT?**

Sign up at https://cotlet.dev/signup?app=torque360, enable Torque360 and sign in at portal.torque360.co in your Cotlet.dev Cloud Browser. Add https://cotlet.dev/mcp/v1/connect in ChatGPT and authenticate with OAuth.

**Can I use Torque360 with Claude?**

Add https://cotlet.dev/mcp/v1/connect as a custom connector in Claude and sign in to Cotlet.dev.

**Can these tools write repair orders or take payments?**

The page describes reports, inspection and document templates, pricing and tax setup. Estimates and repair orders remain with your service advisor. Payment reports only read transactions.

**Is this an official Torque360 server?**

No. Cotlet.dev is independent and is not affiliated with Torque360. Your Torque360 plan and user permissions determine what you can access.

## Live integration page

https://cotlet.dev/integrations/torque360-api-mcp-server

---

More: https://cotlet.dev/integrations/torque360-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=torque360

Start free: https://cotlet.dev/signup?app=torque360&utm_source=github&utm_medium=readme&utm_campaign=torque360
