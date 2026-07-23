# Personal Expense Assistant

A full-stack multimodal expense-tracking application powered by **Google ADK**, **Gemini 2.5**, and GCP services. Users upload receipt images, extract structured expense data, store embeddings in Firestore, and query their expense history through a conversational chat interface.

## What It Does

- Upload receipt images and extract store name, date, line items, totals, and categories
- Store metadata and vector embeddings in **Firestore**
- Save receipt images in **Google Cloud Storage**
- Query expenses via natural-language chat with contextual follow-ups
- Deploy as a single container on **Cloud Run** (FastAPI + Gradio via Supervisord)

## Architecture

```
User (Gradio UI :8080)
    │  POST /chat (text + base64 images)
    ▼
FastAPI Backend (:8081)
    │  ADK Runner → expense_manager_agent
    │  InMemorySessionService + GcsArtifactService
    ▼
Gemini 2.5 (Vertex AI) + ADK tools
    │  Receipt parsing, Firestore CRUD, vector search
    ▼
GCP: Firestore (metadata + embeddings) + GCS (receipt images)
```

## Key Files

| File | Description |
|------|-------------|
| `backend.py` | FastAPI app, ADK `Runner`, `/chat` endpoint |
| `frontend.py` | Gradio multimodal chat UI |
| `schema.py` | `ChatRequest`, `ChatResponse`, `ImageData` Pydantic models |
| `utils.py` | GCS artifact upload/download, response parsing, ADK content formatting |
| `settings.py` / `settings.yaml` | Pydantic Settings (env vars override YAML) |
| `logger.py` | Structured JSON logging for GCP |
| `pyproject.toml` | Dependencies (`google-adk==1.18.0`, Gradio, FastAPI, etc.) |
| `Dockerfile.txt` | Python 3.12 + `uv sync`, Supervisord entrypoint |
| `supervisord.conf` | Runs backend (:8081) and frontend (:8080) together |
| `expense_manager_agent.zip` | ADK agent package (tools, prompts, Firestore handlers) |

## Tech Stack

| Component | Technology |
|-----------|-----------|
| LLM | Gemini 2.5 Flash (Vertex AI) |
| Agent Platform | Google ADK v1.18 |
| Database | Firestore (Native Mode + Vector Search) |
| File Storage | Google Cloud Storage |
| Backend | FastAPI + Uvicorn |
| Frontend | Gradio 5.x |
| Deployment | Cloud Run + Docker + Supervisord |
| Configuration | Pydantic Settings + YAML |

## Prerequisites

- GCP project with **Vertex AI**, **Firestore**, and **Cloud Storage** enabled
- Firestore database created (Native Mode)
- GCS bucket for receipt images and artifacts
- `gcloud auth application-default login`

## Setup

### 1. Extract the agent package

```bash
unzip expense_manager_agent.zip
```

The `expense_manager_agent/` package must be present for `backend.py` to import `root_agent`.

### 2. Configure settings

Edit `settings.yaml` or set environment variables:

| Variable | Description |
|----------|-------------|
| `GCLOUD_PROJECT_ID` | GCP project ID |
| `GCLOUD_LOCATION` | Vertex AI region (default: `us-central1`) |
| `STORAGE_BUCKET_NAME` | GCS bucket for artifacts |
| `DB_COLLECTION_NAME` | Firestore collection for receipts |
| `BACKEND_URL` | Backend URL for frontend (default: `http://localhost:8081/chat`) |

### 3. Install dependencies

```bash
uv sync
```

## Usage

**Local development** (run in separate terminals):

```bash
uv run backend.py    # port 8081
uv run frontend.py     # port 8080
```

Open `http://localhost:8080` to interact with the chat UI.

**Docker** (rename `Dockerfile.txt` to `Dockerfile`):

```bash
docker build -t expense-assistant .
docker run -p 8080:8080 \
  -e GCLOUD_PROJECT_ID=your-project-id \
  -e STORAGE_BUCKET_NAME=your-bucket \
  expense-assistant
```

**Cloud Run:** Build and push the container image, then deploy with the required GCP environment variables. Supervisord runs both services in a single container.

## Request Flow

1. Frontend encodes uploaded images as base64 in a `ChatRequest`
2. Backend stores images as GCS artifacts (SHA-256 hash IDs) and formats ADK `Content`
3. ADK agent runs asynchronously; response is parsed for thinking process and final answer
4. Attachment hash IDs are resolved back to base64 images for the UI

## Features

- Multimodal receipt ingestion (image → structured fields)
- Automated storage of extracted data and receipt images
- Vector-based similarity search for contextual expense queries
- Metadata filters (stores, dates, categories, totals)
- Conversation-aware reasoning
- Cloud-native single-container deployment

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Vertex AI Gemini](https://cloud.google.com/vertex-ai/generative-ai/docs)
- [Firestore Vector Search](https://cloud.google.com/firestore/docs/vector-search)
