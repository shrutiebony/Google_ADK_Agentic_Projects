# Codelab 1 — Currency Agent (MCP + A2A)

An intelligent currency conversion agent built with **Google ADK**, **FastMCP**, and the **A2A Protocol**. It fetches real-time exchange rates via an MCP tool and exposes itself as an A2A server for agent-to-agent communication.

## What It Does

Answers questions like *"How much is 100 USD in CAD?"* by:

1. Connecting to an external **FastMCP server** that exposes a `get_exchange_rate` tool (Frankfurter API)
2. Using **Gemini 2.5 Flash** via ADK to reason about currency conversion queries
3. Exposing the agent as an **A2A server** so other agents can call it

## Architecture

```
[External FastMCP Server]          [currency_agentic_system]
  get_exchange_rate tool    ←──   MCPToolset (Streamable HTTP)
  http://127.0.0.1:8080/mcp         LlmAgent (gemini-2.5-flash)
                                         │
                                         ▼
                                   to_a2a() → A2A server :10000
                                         │
                                         ▼
                                   test_client.py (A2AClient)
```

## Key Files

| File | Description |
|------|-------------|
| `currency_agentic_system/agent.py` | Defines `root_agent` (LlmAgent) and `a2a_app = to_a2a(...)` |
| `currency_agentic_system/test_client.py` | A2A client — single-turn and multi-turn tests |
| `currency_agentic_system/__init__.py` | Package init |

## Agent Details

- **Model:** `gemini-2.5-flash`
- **MCP connection:** `StreamableHTTPConnectionParams` → `MCP_SERVER_URL`
- **A2A port:** `A2A_PORT` (default `10000`)
- **Exports:** `root_agent`, `a2a_app`

## Prerequisites

- External **FastMCP MCP server** exposing `get_exchange_rate` on port 8080 (from the Google Codelab; server source is not included in this repo)
- `google-adk`, `a2a-sdk`, `python-dotenv`

## Setup

```bash
pip install google-adk a2a-sdk python-dotenv httpx
```

Create a `.env` file:

```env
MCP_SERVER_URL=http://127.0.0.1:8080/mcp
A2A_PORT=10000
```

## Usage

### 1. Start the MCP server

Run the external FastMCP server from the codelab (port 8080).

### 2. Start the A2A agent

```bash
cd Codelab_lab_1
uvicorn currency_agentic_system.agent:a2a_app --host 0.0.0.0 --port 10000
```

### 3. Verify the agent card

```
http://127.0.0.1:10000/.well-known/agent.json
```

### 4. Run tests

```bash
python -m currency_agentic_system.test_client
```

The test client sends currency queries and multi-turn follow-ups (e.g., "in GBP").

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `MCP_SERVER_URL` | `http://127.0.0.1:8080/mcp` | MCP tool server endpoint |
| `A2A_PORT` | `10000` | A2A server port |
| `AGENT_URL` | `http://localhost:10000` | Test client target URL |

Gemini/Vertex credentials via `GOOGLE_API_KEY` or Application Default Credentials.

## Endpoints

| Endpoint | URL |
|----------|-----|
| MCP Server | `http://127.0.0.1:8080/mcp` |
| A2A Agent Card | `http://127.0.0.1:10000/.well-known/agent.json` |

## Concepts Demonstrated

- Model Context Protocol (MCP) tool integration
- Google ADK and Gemini API usage
- Agent-to-Agent (A2A) Protocol
- Agent interoperability across services

## Tech Stack

Python 3.12, FastMCP, Google ADK, Gemini 2.5 Flash, A2A Python SDK
