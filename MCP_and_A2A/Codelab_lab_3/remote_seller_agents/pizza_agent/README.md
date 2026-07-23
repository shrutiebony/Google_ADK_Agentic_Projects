# Pizza Seller Agent

A remote **pizza seller A2A server** built with **LangGraph** and **ChatVertexAI**. Handles menu queries and order creation, exposed as an A2A service for the Purchasing Concierge to call.

## What It Does

- Responds to menu requests with available pizza options and prices
- Creates pizza orders when requested
- Exposes an A2A agent card for discovery by the Purchasing Concierge

## Menu

| Item | Price (IDR) |
|------|-------------|
| Margherita | 100,000 |
| Pepperoni | 140,000 |
| Hawaiian | 110,000 |
| Veggie | 100,000 |
| BBQ Chicken | 130,000 |

## Architecture

```
A2AStarletteApplication (uvicorn)
  → PizzaSellerAgentExecutor
      → PizzaSellerAgent (LangGraph)
          → create_pizza_order tool
```

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | LangGraph agent, menu data, order creation tool |
| `agent_executor.py` | A2A `AgentExecutor` bridge |
| `__main__.py` | A2A server entry point (default port **10000**) |
| `Dockerfile` | Python 3.12 + uv; exposes port 8080 |
| `pyproject.toml` | Dependencies |
| `.env.example` | GCP and HOST_OVERRIDE template |

## Setup

```bash
cd remote_seller_agents/pizza_agent
uv sync
cp .env.example .env
# Edit .env with your GCP project details
```

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
HOST_OVERRIDE=                    # Set to public URL when deployed
```

## Usage

### Local

```bash
uv run . --host 0.0.0.0 --port 10000
```

Verify agent card: `http://localhost:10000/.well-known/agent.json`

### Cloud Run

```bash
gcloud run deploy pizza-agent \
  --source . \
  --region us-central1 \
  --allow-unauthenticated
```

After deployment, set `HOST_OVERRIDE` to the Cloud Run service URL so the agent card advertises the correct public endpoint.

The Dockerfile runs on port 8080:

```bash
uv run . --host 0.0.0.0 --port 8080
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Vertex AI project ID |
| `GOOGLE_CLOUD_LOCATION` | Vertex AI region |
| `HOST_OVERRIDE` | Public URL for agent card when behind Cloud Run proxy |

## Dependencies

Python `>=3.12`: `langgraph`, `langchain-google-vertexai`, `a2a-sdk>=0.2.16`, `uvicorn`, `pydantic`, `python-dotenv`

## Tech Stack

LangGraph, ChatVertexAI, A2A SDK, uvicorn, Cloud Run

## Port Assignment

The pizza agent runs on port **10000** locally; the burger agent uses **10001**. This matches the defaults in the Purchasing Concierge `.env.example`.
