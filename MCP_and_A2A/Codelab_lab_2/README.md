# Codelab 2 — Multi-Agent Image Scoring System

A multi-agent image generation and scoring pipeline built with **Google ADK** and the **A2A Protocol**. Generates Imagen3 prompts from news text, creates images, scores them against a content policy, and loops until the score threshold is met or max iterations are reached.

Based on: [Google Codelab – Create Multi-Agents with ADK & A2A](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)

## What It Does

Given a text prompt (e.g., "Create image of a cat"):

1. **Prompt Agent** — Builds an Imagen3 prompt from news text and `policy.json` rules
2. **Imagen Agent** — Generates an image via Imagen 3 on Vertex AI
3. **Scoring Agent** — Scores the image against the content policy
4. **Checker Agent** — Decides whether to loop again or stop

The system can run locally via ADK, deploy to **Vertex AI Agent Engine**, or expose as a **remote A2A agent**.

## Architecture

```
LoopAgent (image_scoring)
├── SequentialAgent (image_generation_scoring_agent)
│   ├── prompt_agent      → builds Imagen prompt from policy.json
│   ├── imagen_agent      → generates image via Imagen 3
│   └── scoring_agent     → scores image; sets total_score
└── checker_agent         → check_tool_condition → escalate when done
```

## Folder Structure

```
Codelab_lab_2/
├── Bicycle.png, Waterfall.png         # Sample images
└── multi_agentic_system/
    ├── image_scoring/                 # Core agent package
    ├── image_scoring_adk_a2a_server/  # Remote A2A client wrapper
    └── testclient/                    # Agent Engine remote test
```

See [multi_agentic_system/README.md](./multi_agentic_system/README.md) for detailed package documentation.

## Key Components

| Component | Location | Description |
|-----------|----------|-------------|
| Core agent | `multi_agentic_system/image_scoring/` | Full agent pipeline with sub-agents and tools |
| A2A server | `multi_agentic_system/image_scoring_adk_a2a_server/` | Wraps agent as remote A2A service |
| Test client | `multi_agentic_system/testclient/` | Tests deployed Agent Engine instance |

## Loop Logic

- `MAX_ITERATIONS` (default: 1) and `SCORE_THRESHOLD` (default: 45)
- `check_tool_condition` reads `total_score` from session state
- Sets `escalate=True` when score exceeds threshold or max iterations reached

## Prerequisites

- GCP project with Vertex AI enabled
- Python 3.10+
- Poetry or pip

## Quick Start

```bash
cd multi_agentic_system
poetry install    # or: pip install -r requirements.txt
gcloud auth application-default login

# Set environment variables (see image_scoring/config.py)
export GCS_BUCKET_NAME=your-bucket    # optional
export SCORE_THRESHOLD=45
export MAX_ITERATIONS=1

# Run via ADK dev UI
adk web

# Or run directly
adk run image_scoring
```

## Deployment to Agent Engine

```bash
cd multi_agentic_system/image_scoring
poetry build    # creates dist/image_scoring-0.1.0-py3-none-any.whl
python deploy.py
```

Edit hardcoded values in `deploy.py` before deploying:
- `PROJECT_ID`
- `LOCATION`
- `STAGING_BUCKET`

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `GCS_BUCKET_NAME` | — | Optional GCS upload for generated images |
| `SCORE_THRESHOLD` | `45` | Minimum acceptable score |
| `MAX_ITERATIONS` | `1` | Max regeneration loops |
| `IMAGEN_MODEL` | `imagen-3.0-generate-002` | Image generation model |
| `GENAI_MODEL` | `gemini-2.0-flash` | LLM for prompt/scoring agents |

## Video Walkthrough

[YouTube: Multi-Agent ADK & A2A Demo](https://youtu.be/oYPckTgBT0w)

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Create Multi-Agents Codelab](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)
- [Instavibe ADK Multi-Agents](https://codelabs.developers.google.com/instavibe-adk-multi-agents/instructions)
