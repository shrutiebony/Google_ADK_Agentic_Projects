# Purchasing Concierge Agent

The orchestrator agent for the Codelab 3 purchasing concierge demo. Built with **Google ADK**, it discovers remote seller agents via A2A agent cards and delegates menu queries and order placement.

## What It Does

1. On first request, resolves A2A agent cards from configured seller URLs
2. Uses the `send_task(agent_name, task)` tool to send A2A messages to burger or pizza sellers
3. Maintains session state (`active_agent`, `session_id`, `contextId`) for multi-turn conversations
4. Merges seller responses into natural language for the user

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | Wires `PurchasingAgent` with seller URLs from env; exports `root_agent` |
| `purchasing_agent.py` | Core orchestrator — agent card discovery, `send_task` tool, session state |
| `remote_agent_connection.py` | A2A client wrapper with httpx event-loop workaround |
| `.env.example` | Environment variable template |

## Agent Details

- **Model:** `gemini-2.5-flash-lite`
- **Tools:** `send_task` — sends A2A messages to remote seller agents
- **Callback:** `before_agent_callback` — discovers agent cards on first request

## Setup

```bash
cp .env.example .env
# Edit .env with your seller agent URLs and GCP settings
```

```env
PIZZA_SELLER_AGENT_URL=http://localhost:10000
BURGER_SELLER_AGENT_URL=http://localhost:10001
GOOGLE_GENAI_USE_VERTEXAI=TRUE
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
```

## Local Development

Run seller agents locally first (see their READMEs), then:

```bash
# Deploy or run the concierge via ADK
adk run purchasing_concierge

# Or deploy to Agent Engine
cd ..
python deploy_to_agent_engine.py
```

## Deployment

Deployed to **Vertex AI Agent Engine** via the parent `deploy_to_agent_engine.py` script. Requires:

- `GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`
- `STAGING_BUCKET` — GCS bucket for staging
- Seller agent URLs pointing to deployed Cloud Run services

After deployment, set `AGENT_ENGINE_RESOURCE_NAME` for the Gradio UI.

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PIZZA_SELLER_AGENT_URL` | Pizza A2A server base URL |
| `BURGER_SELLER_AGENT_URL` | Burger A2A server base URL |
| `GOOGLE_GENAI_USE_VERTEXAI` | Use Vertex AI for Gemini |
| `GOOGLE_CLOUD_PROJECT` | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | GCP region |
| `STAGING_BUCKET` | GCS staging bucket (for Agent Engine deploy) |
| `AGENT_ENGINE_RESOURCE_NAME` | Deployed resource name (for Gradio UI) |

## Dependencies

```
google-cloud-aiplatform[adk,agent-engines]
a2a-sdk==0.2.16
google-adk
httpx
python-dotenv
```

## Tech Stack

Google ADK, Gemini 2.5 Flash Lite, A2A SDK, Vertex AI Agent Engine
