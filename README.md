# Bookipi MCP server for ChatGPT and Claude

Bookipi connects to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev, so your AI agent can work with invoices, customers, proposals and contracts in your own Bookipi account. This repository provides a Python MCP stdio adapter for the hosted Cotlet.dev browser tools.

Unofficial, not affiliated with, endorsed by or sponsored by Bookipi.

## What can an AI agent do in Bookipi?

The [live Bookipi integration page](https://cotlet.dev/integrations/bookipi-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=bookipi) describes these workflows:

- Filter invoice lists and view archived invoices.
- Create invoice drafts with customer details and line items.
- Review customer correspondence and transaction history.
- Create and edit proposal drafts.
- Prepare contracts and website content for review.

Review amounts, recipients and document content before approving external sends, signatures or payment-related changes.

## How do I connect Bookipi to ChatGPT or Claude?

1. [Get access through Cotlet.dev](https://cotlet.dev/signup?app=bookipi&utm_source=github&utm_medium=readme&utm_campaign=bookipi).
2. Open `web.bookipi.com` in your Cotlet.dev Cloud Browser and sign in yourself.
3. In ChatGPT, add an MCP connector. In Claude, add a custom connector. Use `https://cotlet.dev/mcp/v1/connect` and sign in through Cotlet.dev.
4. Ask your agent to read invoice lists or prepare an unsent invoice draft.

## How do I connect Codex or Claude Code?

Create an API key in the Cotlet.dev console and set `COTLET_API_KEY` in your environment.

Codex configuration:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

Claude Code:

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## How do I run the local MCP adapter?

Use Python 3.10 or newer. The adapter uses the standard library.

```bash
git clone https://github.com/cotlet-connect/bookipi-mcp.git
cd bookipi-mcp
python3 bookipi_agent.py serve
```

For a client that starts local MCP servers, use an absolute path to the downloaded adapter:

```json
{"mcpServers":{"bookipi":{"command":"python3","args":["/absolute/path/bookipi-mcp/bookipi_agent.py","serve"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

The local adapter exposes seven browser helpers: `bookipi_guide`, `bookipi_status`, `bookipi_open`, `bookipi_read`, `bookipi_click`, `bookipi_type` and `bookipi_close`. These are browser helpers, distinct from the hosted workflow catalog on the integration page. Call the guide first. Without an API key, listing tools works and tool calls return setup instructions.

```bash
python3 bookipi_agent.py connect
python3 bookipi_agent.py guide
python3 bookipi_agent.py status
```

The adapter forwards calls to `https://cotlet.dev/mcp/v1`; it requires a Cotlet.dev account and a signed-in Bookipi Cloud Browser. At a sign-in page, sign in yourself. Close the browser session when finished.

## Does Bookipi have an official API or MCP server?

Bookipi publishes its own hosted MCP server at `https://mcp.bookipi.com/mcp` and a CLI for selected business operations. The live Cotlet.dev page reports vendor instructions for Claude, checked 12 November 2026. Its research did not find a general-purpose public REST API reference. Compare the official documented operations with your task and use Bookipi's own connection when it covers what you need.

## What should I ask first?

Start with a read, such as "Filter invoice lists for the period I select" or "Review this customer's transaction history". For a new invoice or proposal, ask for an unsent draft and review it before approving a send. Features depend on access in your own Bookipi account.

Source: [Bookipi on Cotlet.dev](https://cotlet.dev/integrations/bookipi-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=bookipi). Source page retrieved 20 November 2026.
