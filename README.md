# Flight Master Aviation Data MCP

Official MCP integration documentation for **Flight Master DAST (航班管家 DAST)**, providing aviation data capabilities for AI agents, coding assistants, and MCP-compatible applications.

Flight Master Aviation Data MCP provides access to flight status, route flights, onboard experience, delay risk prediction, airport weather, and flight trajectory data through the Model Context Protocol (MCP).

> This repository contains integration documentation only.  
> The production MCP server is remotely hosted and the server-side source code is not published in this repository.

## Official Links

- **MCP Product Page:** https://dast.133.cn/mcp/
- **DAST Platform:** https://dast.133.cn
- **Agent-readable Documentation:** https://dast.133.cn/mcp_assets/agent.md
- **Remote MCP Endpoint:** https://fly.huoli.com/mcp/dast_mcp

## MCP Endpoint

```text
https://fly.huoli.com/mcp/dast_mcp
```

| Item | Value |
|---|---|
| Protocol | Model Context Protocol (MCP) |
| Transport | Streamable HTTP |
| RPC | JSON-RPC 2.0 |
| Authentication | Bearer API Key |

## Authentication

An API Key is required to connect to the Flight Master Aviation Data MCP.

Include the API Key in the HTTP `Authorization` header:

```text
Authorization: Bearer <API_KEY>
```

To obtain an API Key:

1. Visit the [Flight Master DAST Platform](https://dast.133.cn)
2. Register or sign in
3. Open the MCP Console
4. Create an API Key
5. Use the key in your MCP client configuration

> **Security:** Never publish, commit, or share your real API Key in a public repository.

## Available Tools

The MCP server currently provides the following six tools:

| Tool | Capability |
|---|---|
| `dast_flight_dynamic` | Query real-time or historical flight status by flight number and date |
| `dast_flight_route` | Query flights between two airports for a specified date |
| `dast_flight_happy` | Query onboard experience data such as meals, Wi-Fi, seat information, entertainment, power supply, and baggage |
| `dast_delay_rate` | Query predicted flight delay and cancellation probabilities |
| `dast_future_weather` | Query hourly airport weather forecasts for the next 48 hours |
| `dast_flight_path` | Query real-time or historical flight trajectory, position, altitude, speed, and flight status |

For complete Tool schemas, parameters, response structures, routing rules, and usage guidance, see:

**https://dast.133.cn/mcp_assets/agent.md**

The live MCP `tools/list` response should be treated as the authoritative source if Tool definitions change.

## Claude Code

Add the remote MCP server with:

```bash
claude mcp add --transport http flight-master-dast https://fly.huoli.com/mcp/dast_mcp \
  --header "Authorization: Bearer <API_KEY>"
```

Then verify the connection:

```bash
claude mcp list
```

Replace `<API_KEY>` with the API Key created in the Flight Master DAST MCP Console.

## Cursor

Cursor supports remote MCP servers through `mcp.json`.

For a personal configuration available across projects, edit:

```text
~/.cursor/mcp.json
```

Example:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "url": "https://fly.huoli.com/mcp/dast_mcp",
      "headers": {
        "Authorization": "Bearer <API_KEY>"
      }
    }
  }
}
```

Save the configuration and restart or reconnect Cursor. The Flight Master DAST tools should then appear in the MCP tools list.

> Do not commit a configuration file containing a real API Key to a public repository.

## Cline

Open **MCP Servers → Configure MCP Servers** in Cline and add:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "type": "streamableHttp",
      "url": "https://fly.huoli.com/mcp/dast_mcp",
      "headers": {
        "Authorization": "Bearer <API_KEY>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Replace `<API_KEY>` with your Flight Master DAST API Key and reconnect the server.

## Agent-readable Documentation

A dedicated machine-readable document is available for AI agents and automated clients:

```text
https://dast.133.cn/mcp_assets/agent.md
```

It includes:

- MCP connection information
- Authentication requirements
- Tool selection guidance
- Tool schemas and parameters
- Date and airport rules
- Request and response structures
- Error handling
- Billing behavior
- Agent usage guidance

Agents integrating Flight Master Aviation Data MCP are encouraged to read this document before calling the service.

## Example Use Cases

Flight Master Aviation Data MCP can be used by AI agents to answer questions such as:

- What is the current status of CA1831?
- What flights operate from Beijing Capital Airport to Shanghai Hongqiao Airport today?
- Does this flight provide meals or Wi-Fi?
- What is the predicted delay risk for this flight?
- What will the weather be like at Hefei Xinqiao International Airport?
- Where is this flight currently located and what is its altitude?

## About Flight Master DAST

**Flight Master DAST** provides professional aviation data services and MCP capabilities for developers, AI agents, and aviation-related applications.

Visit the official MCP product page:

**https://dast.133.cn/mcp/**

For account registration, API Key management, usage information, and billing, visit:

**https://dast.133.cn**

## Repository Scope

This repository is the official public integration repository for Flight Master Aviation Data MCP.

It contains documentation and configuration examples only.

It does **not** contain:

- Production MCP server source code
- Internal aviation data processing systems
- Proprietary data pipelines
- Backend business logic

The production MCP service is operated remotely by Flight Master DAST.

---

**Flight Master DAST**  
Official Aviation Data MCP Service
