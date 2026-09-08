# Flight Master Aviation Data MCP

Official MCP integration repository for **Flight Master DAST (航班管家 DAST)**, providing professional aviation data capabilities for AI agents, coding assistants, and MCP-compatible applications.

**航班管家航空数据 MCP（Flight Master Aviation Data MCP）** provides access to flight status, route flights, onboard experience, delay risk prediction, airport weather, and flight trajectory data through the Model Context Protocol (MCP).

The service supports two MCP connection modes:

- **Remote MCP** via Streamable HTTP — recommended for most users
- **STDIO MCP** via the official npm package — for local MCP client integration

> This repository is the official public integration repository for Flight Master Aviation Data MCP.
> Production server-side source code and proprietary aviation data processing systems are not published in this repository.

---

## Official Links

- MCP Product Page: https://dast.133.cn/mcp/
- DAST Platform: https://dast.133.cn
- Agent-readable Documentation: https://dast.133.cn/mcp_assets/agent.md
- Remote MCP Endpoint: https://fly.huoli.com/mcp/dast_mcp
- npm STDIO Package: https://www.npmjs.com/package/@flightmaster/aviation-dast-mcp

---

## Connection Modes

Flight Master Aviation Data MCP supports both **Remote Streamable HTTP** and **STDIO** connections.

### Option 1 — Remote MCP via Streamable HTTP

Recommended for most users, hosted AI agents, and MCP clients that support remote MCP servers.

Endpoint:

```text
https://fly.huoli.com/mcp/dast_mcp
```

| Item | Value |
| --- | --- |
| Protocol | Model Context Protocol (MCP) |
| Transport | Streamable HTTP |
| RPC | JSON-RPC 2.0 |
| Authentication | Bearer API Key |

Authentication header:

```text
Authorization: Bearer <API_KEY>
```

No local MCP server installation is required.

---

### Option 2 — STDIO MCP via npm

For MCP clients that support local STDIO servers, use the official npm package:

```text
@flightmaster/aviation-dast-mcp
```

Run directly with:

```bash
npx -y @flightmaster/aviation-dast-mcp
```

The STDIO package requires the following environment variable:

```text
DAST_API_KEY=<API_KEY>
```

Example MCP configuration:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "command": "npx",
      "args": [
        "-y",
        "@flightmaster/aviation-dast-mcp"
      ],
      "env": {
        "DAST_API_KEY": "<API_KEY>"
      }
    }
  }
}
```

The package also supports the optional environment variable:

```text
DAST_MCP_URL=<CUSTOM_GATEWAY_URL>
```

`DAST_MCP_URL` should normally be omitted. It is only required when a custom Flight Master DAST MCP gateway URL needs to be used.

Official npm package:

https://www.npmjs.com/package/@flightmaster/aviation-dast-mcp

---

## Authentication

A Flight Master DAST API Key is required for both connection modes.

### Remote MCP

Include the API Key in the HTTP `Authorization` header:

```text
Authorization: Bearer <API_KEY>
```

### STDIO MCP

Provide the API Key through the required environment variable:

```text
DAST_API_KEY=<API_KEY>
```

### Obtain an API Key

1. Visit the Flight Master DAST Platform: https://dast.133.cn
2. Register or sign in
3. Open the MCP Console
4. Create an API Key
5. Use the key in your MCP client configuration

> **Security:** Never publish, commit, or share your real API Key in a public repository.

---

## Available Tools

The MCP server currently provides the following six tools:

| Tool | Capability |
| --- | --- |
| `dast_flight_dynamic` | Query real-time or historical flight status by flight number and date |
| `dast_flight_route` | Query flights between two airports for a specified date |
| `dast_flight_happy` | Query onboard experience data such as meals, Wi-Fi, seat information, entertainment, power supply, and baggage |
| `dast_delay_rate` | Query predicted flight delay and cancellation probabilities |
| `dast_future_weather` | Query hourly airport weather forecasts for the next 48 hours |
| `dast_flight_path` | Query real-time or historical flight trajectory, position, altitude, speed, and flight status |

For complete Tool schemas, parameters, response structures, routing rules, and usage guidance, see:

https://dast.133.cn/mcp_assets/agent.md

> The live MCP `tools/list` response should be treated as the authoritative source if Tool definitions change.

---

## Claude Code

### Remote MCP

Add the hosted Remote MCP server with:

```bash
claude mcp add --transport http flight-master-dast https://fly.huoli.com/mcp/dast_mcp \
  --header "Authorization: Bearer <API_KEY>"
```

Then verify the connection:

```bash
claude mcp list
```

Replace `<API_KEY>` with the API Key created in the Flight Master DAST MCP Console.

### STDIO

Claude Code and other STDIO-compatible MCP clients can also use the official npm package.

Generic STDIO configuration:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "command": "npx",
      "args": [
        "-y",
        "@flightmaster/aviation-dast-mcp"
      ],
      "env": {
        "DAST_API_KEY": "<API_KEY>"
      }
    }
  }
}
```

---

## Cursor

Cursor supports both Remote MCP and STDIO MCP configurations.

### Remote MCP — Recommended

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

### STDIO

Alternatively, configure the official npm package:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "command": "npx",
      "args": [
        "-y",
        "@flightmaster/aviation-dast-mcp"
      ],
      "env": {
        "DAST_API_KEY": "<API_KEY>"
      }
    }
  }
}
```

> Do not commit a configuration file containing a real API Key to a public repository.

---

## Cline

Cline supports both Remote MCP and STDIO MCP configurations.

### Remote MCP — Recommended

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

### STDIO

Alternatively:

```json
{
  "mcpServers": {
    "flight-master-dast": {
      "command": "npx",
      "args": [
        "-y",
        "@flightmaster/aviation-dast-mcp"
      ],
      "env": {
        "DAST_API_KEY": "<API_KEY>"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

---

## Generic MCP Client Configuration

### Remote Streamable HTTP

Use:

```text
URL:
https://fly.huoli.com/mcp/dast_mcp
```

Header:

```text
Authorization: Bearer <API_KEY>
```

### STDIO

Use:

```json
{
  "command": "npx",
  "args": [
    "-y",
    "@flightmaster/aviation-dast-mcp"
  ],
  "env": {
    "DAST_API_KEY": "<API_KEY>"
  }
}
```

---

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

---

## Example Use Cases

Flight Master Aviation Data MCP can be used by AI agents to answer questions such as:

- What is the current status of CA1831?
- What flights operate from Beijing Capital Airport to Shanghai Hongqiao Airport today?
- Does this flight provide meals or Wi-Fi?
- What is the predicted delay risk for this flight?
- What will the weather be like at Hefei Xinqiao International Airport?
- Where is this flight currently located and what is its altitude?

---

## About Flight Master DAST

**Flight Master DAST（航班管家 DAST）** provides professional aviation data services and MCP capabilities for developers, AI agents, coding assistants, and aviation-related applications.

Flight Master Aviation Data MCP connects AI applications with professional aviation data through the open Model Context Protocol ecosystem.

Official MCP Product Page:

https://dast.133.cn/mcp/

For account registration, API Key management, usage information, and billing:

https://dast.133.cn

---

## Repository Scope

This repository is the official public integration repository for **Flight Master Aviation Data MCP**.

It contains:

- MCP integration documentation
- Remote MCP connection examples
- STDIO npm integration examples
- Authentication guidance
- Client configuration examples
- MCP ecosystem metadata

It does not contain:

- Production MCP server source code
- Internal aviation data processing systems
- Proprietary data pipelines
- Backend business logic

The production aviation data service and Remote MCP infrastructure are operated by **Flight Master DAST**.

---

**Flight Master DAST**  
**航班管家 DAST**  
Official Aviation Data MCP Service
