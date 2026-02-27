# Optional MCP Servers

The main `mcp.json` configuration includes only MCP servers that work out-of-the-box without requiring manual user credentials. This document explains how to add optional servers that require additional setup.

## Why These Were Excluded from Default Config

The following servers require user-specific secrets or manual configuration:

- `postgres` — Requires database connection string
- `brave-search` — Requires API key
- `puppeteer` / `playwright` — Heavier browser automation (add only if needed)
- `slack` — Requires bot token; OpenClaw already has a native Slack channel (`src/slack/`) for message routing, so this MCP server is only needed if you want Copilot to query Slack workspaces directly during development
- `sqlite` — Requires a local database file path

## How to Add Optional Servers

To add any of these servers, edit `.github/copilot/mcp.json` and add the server configuration under `mcpServers`:

### Brave Search (Web Search)

```json
"brave-search": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-brave-search"],
  "env": {
    "BRAVE_API_KEY": "${COPILOT_MCP_BRAVE_API_KEY}"
  }
}
```

**Required environment variable:**
```bash
export COPILOT_MCP_BRAVE_API_KEY="your_brave_api_key_here"
```

Get an API key from: https://brave.com/search/api/

### PostgreSQL Database

```json
"postgres": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "mcp-server-postgres"],
  "env": {
    "POSTGRES_CONNECTION_STRING": "${COPILOT_MCP_POSTGRES_CONNECTION_STRING}"
  }
}
```

**Required environment variable:**
```bash
export COPILOT_MCP_POSTGRES_CONNECTION_STRING="postgresql://user:pass@host:port/db"
```

### SQLite (Local Database)

```json
"sqlite": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sqlite", "--db-path", "path/to/your.db"]
}
```

### Puppeteer (Browser Automation)

```json
"puppeteer": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@anthropic/server-puppeteer"],
  "tools": ["puppeteer_navigate", "puppeteer_screenshot", "puppeteer_click", "puppeteer_fill", "puppeteer_select", "puppeteer_hover", "puppeteer_evaluate"]
}
```

### Playwright (Alternative to Puppeteer)

```json
"playwright": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@anthropic/server-playwright"]
}
```

**Note:** Playwright is heavier than Puppeteer. Use one or the other, not both.

### Slack Integration

```json
"slack": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-slack"],
  "env": {
    "SLACK_BOT_TOKEN": "${COPILOT_MCP_SLACK_BOT_TOKEN}",
    "SLACK_TEAM_ID": "${COPILOT_MCP_SLACK_TEAM_ID}"
  }
}
```

### AWS Cloud Services

```json
"aws": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "mcp-server-aws"],
  "env": {
    "AWS_ACCESS_KEY_ID": "${COPILOT_MCP_AWS_ACCESS_KEY_ID}",
    "AWS_SECRET_ACCESS_KEY": "${COPILOT_MCP_AWS_SECRET_ACCESS_KEY}",
    "AWS_REGION": "${COPILOT_MCP_AWS_REGION}"
  }
}
```

### Sentry Error Tracking

```json
"sentry": {
  "type": "stdio",
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-sentry"],
  "env": {
    "SENTRY_DSN": "${COPILOT_MCP_SENTRY_DSN}",
    "SENTRY_AUTH_TOKEN": "${COPILOT_MCP_SENTRY_AUTH_TOKEN}"
  }
}
```

## Environment Variable Management

Add environment variables to your local environment (never commit these):

```bash
# Required for GitHub integration
GITHUB_TOKEN=ghp_your_token_here

# Optional MCP servers
COPILOT_MCP_BRAVE_API_KEY=your_brave_key_here
COPILOT_MCP_POSTGRES_CONNECTION_STRING=postgresql://user:pass@host:port/db

# OpenClaw-specific (for local development)
OPENCLAW_CONFIG_DIR=~/.openclaw
```

## Verifying Server Configuration

After adding a server, verify it works:

1. Restart your Copilot session
2. Check the logs for any errors
3. Test the server's functionality with a simple command

## Getting Help

If you encounter issues with optional servers:
1. Check that all required environment variables are set
2. Verify credentials are valid
3. Ensure required tools are installed (`npx -y <package>` auto-installs)
4. Review the MCP server's documentation on npmjs.com

---

**Note:** The default `mcp.json` is optimized for immediate use without manual configuration. Only add optional servers when you specifically need their functionality.
