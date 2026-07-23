# Burger Seller Agent

A remote **burger seller A2A server** built with **CrewAI** and **Vertex AI Gemini**. Handles menu queries and order creation, exposed as an A2A service for the Purchasing Concierge to call.

## What It Does

- Responds to menu requests with available burger options and prices
- Creates burger orders when requested
- Exposes an A2A agent card for discovery by the Purchasing Concierge

## Menu

| Item | Price (IDR) |
|------|-------------|
| Classic Cheeseburger | 85,000 |
| Double Cheeseburger | 110,000 |
| Spicy Chicken Burger | 80,000 |
| Spicy Cajun Burger | 85,000 |

## Architecture

```
A2AStarletteApplication (uvicorn)
  → BurgerSellerAgentExecutor
      → BurgerSellerAgent (CrewAI Crew)
          → create_burger_order tool
```

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | CrewAI agent, menu data, `create_burger_order` tool |
| `agent_executor.py` | A2A `AgentExecutor` bridge |
| `__main__.py` | A2A server entry point (default port **10001**) |
| `Dockerfile` | Python 3.12 + uv; exposes port 8080 |
| `pyproject.toml` | Dependencies |
| `.env.example` | GCP and HOST_OVERRIDE template |

## Setup

```bash
cd remote_seller_agents/burger_agent
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
uv run . --host 0.0.0.0 --port 10001
```

Verify agent card: `http://localhost:10001/.well-known/agent.json`

### Cloud Run

```bash
gcloud run deploy burger-agent \
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

Python `>=3.12`: `crewai`, `a2a-sdk>=0.2.16`, `click`, `httpx`, `pydantic`, `python-dotenv`

## Tech Stack

CrewAI, Vertex AI Gemini, A2A SDK, uvicorn, Cloud Run
