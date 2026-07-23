# Codelab 3 — Purchasing Concierge (A2A Action Engine)

A **Purchasing Concierge** AI agent that communicates with remote **Burger** and **Pizza Seller Agents** using the **Agent-to-Agent (A2A)** protocol on Google Cloud. Demonstrates how agents built with different frameworks can collaborate seamlessly.

## What It Does

The user chats with a purchasing concierge (via Gradio UI). The concierge discovers remote seller agents, delegates menu queries and order placement, and merges responses into a natural conversation.

**Example:**

> **User:** "Show me burger and pizza menu."
> **Agent:** Displays menus from both seller agents.
>
> **User:** "I want to order one BBQ chicken pizza and one spicy cajun burger."
> **Agent:** Sends A2A tasks to both sellers and confirms both orders.

## Architecture

```
User → Gradio UI (purchasing_concierge_ui.py)
         → Vertex AI Agent Engine (PurchasingAgent / gemini-2.5-flash-lite)
              → send_task tool → A2A → pizza_seller_agent (LangGraph, :10000)
                                   → burger_seller_agent (CrewAI, :10001)
```

## Components

| Component | Technology | Role |
|-----------|-----------|------|
| **Purchasing Concierge** | Google ADK + Vertex AI Agent Engine | Orchestrator and A2A client |
| **Burger Seller Agent** | CrewAI | A2A server for burger orders |
| **Pizza Seller Agent** | LangGraph | A2A server for pizza orders |
| **Frontend UI** | Gradio | Chat-based user interface |
| **Hosting** | Cloud Run + Agent Engine | Serverless infrastructure |

## Folder Structure

```
Codelab_lab_3/
├── purchasing_concierge/              # Orchestrator agent
│   ├── agent.py                       # Wires PurchasingAgent with seller URLs
│   ├── purchasing_agent.py            # Core orchestrator logic
│   └── remote_agent_connection.py     # A2A client wrapper
├── remote_seller_agents/
│   ├── burger_agent/                  # CrewAI A2A server
│   └── pizza_agent/                   # LangGraph A2A server
├── purchasing_concierge_ui.py         # Gradio chat UI
└── deploy_to_agent_engine.py          # Agent Engine deployment script
```

## End-to-End Workflow

### 1. GCP Setup

Enable billing, Vertex AI, and Cloud Run. Authenticate:

```bash
gcloud auth application-default login
```

### 2. Deploy Seller Agents to Cloud Run

```bash
cd remote_seller_agents/pizza_agent
gcloud run deploy pizza-agent --source . --region us-central1 --allow-unauthenticated

cd ../burger_agent
gcloud run deploy burger-agent --source . --region us-central1 --allow-unauthenticated
```

Set `HOST_OVERRIDE` on each Cloud Run service to its public URL.

### 3. Verify Agent Cards

Visit `/.well-known/agent.json` on each Cloud Run service URL.

### 4. Configure the Concierge

Copy and edit the environment file:

```bash
cd purchasing_concierge
cp .env.example .env
```

```env
PIZZA_SELLER_AGENT_URL=https://pizza-agent-xxxxx.run.app
BURGER_SELLER_AGENT_URL=https://burger-agent-xxxxx.run.app
GOOGLE_GENAI_USE_VERTEXAI=TRUE
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
STAGING_BUCKET=gs://your-staging-bucket
```

### 5. Deploy the Concierge

```bash
cd Codelab_lab_3
python deploy_to_agent_engine.py
# Note the printed resource_name
```

### 6. Launch the Gradio UI

```bash
export AGENT_ENGINE_RESOURCE_NAME=<resource from step 5>
python purchasing_concierge_ui.py
# UI at http://0.0.0.0:8080
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PIZZA_SELLER_AGENT_URL` | Pizza A2A server base URL |
| `BURGER_SELLER_AGENT_URL` | Burger A2A server base URL |
| `GOOGLE_GENAI_USE_VERTEXAI` | Use Vertex for Gemini |
| `GOOGLE_CLOUD_PROJECT` | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | GCP region |
| `STAGING_BUCKET` | GCS staging bucket for Agent Engine |
| `AGENT_ENGINE_RESOURCE_NAME` | Deployed Agent Engine resource (for UI) |

## Cleanup

To prevent unwanted costs, delete Cloud Run services and Agent Engine deployments when finished, or delete the entire GCP project.

## Related READMEs

- [purchasing_concierge/](./purchasing_concierge/README.md) — Orchestrator agent details
- [burger_agent/](./remote_seller_agents/burger_agent/README.md) — CrewAI burger seller
- [pizza_agent/](./remote_seller_agents/pizza_agent/README.md) — LangGraph pizza seller
