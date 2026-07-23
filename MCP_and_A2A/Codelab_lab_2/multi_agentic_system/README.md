# Multi-Agent Image Scoring System

The core package for the Codelab 2 image generation and scoring pipeline. Contains the ADK agent, A2A server wrapper, and Agent Engine test client.

## Package Structure

```
multi_agentic_system/
├── pyproject.toml                     # Poetry project "image-scoring"
├── requirements.txt                   # Full dependency lockfile
├── image_scoring/                     # Core agent package
│   ├── agent.py                       # LoopAgent + SequentialAgent composition
│   ├── checker_agent.py               # Loop termination checker
│   ├── config.py                      # Environment-based configuration
│   ├── policy.json                    # Image content/composition rules
│   ├── deploy.py                      # Vertex AI Agent Engine deployment
│   ├── prompt.py                      # Checker agent system prompt
│   ├── sub_agents/
│   │   ├── prompt/                    # Prompt generation agent
│   │   ├── image/                     # Imagen 3 image generation agent
│   │   ├── scoring/                   # Image scoring agent
│   │   └── tools/                     # Shared tools (fetch_policy)
│   └── tools/
│       └── loop_condition_tool.py     # Loop counter and escalation logic
├── image_scoring_adk_a2a_server/      # Remote A2A client wrapper
│   ├── a2a_agent.py                   # RemoteA2aAgent pointing at agent card
│   └── remote_a2a/image_scoring/    # Mirror of image_scoring (A2A server copy)
└── testclient/
    └── remote_test.py                 # Agent Engine remote test client
```

## Sub-Agents

| Agent | Role |
|-------|------|
| **prompt_agent** | Builds Imagen3 prompts from news text using `policy.json` rules |
| **imagen_agent** | Generates images via Imagen 3 (`imagen-3.0-generate-002`) on Vertex AI |
| **scoring_agent** | Scores generated images against the content policy |
| **checker_agent** | Checks loop condition — stops when score exceeds threshold or max iterations reached |

## Tools

| Tool | Description |
|------|-------------|
| `fetch_policy_tool` | Reads `policy.json` content and composition rules |
| `image_generation_tool` | Calls Imagen 3 via `google.genai`; saves to GCS + ADK artifacts |
| `get_images_tool` | Loads generated image artifact from session |
| `set_score_tool` | Writes `total_score` to session state |
| `loop_condition_tool` | Increments loop counter; sets `escalate=True` when done |

## Setup

```bash
cd multi_agentic_system
poetry install
# or
pip install -r requirements.txt
```

## Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| `google-adk` | `==1.8.0` | Core agent framework |
| `google-cloud-aiplatform` | `^1.88.0` | Agent Engine deployment |
| `google-genai` | `^1.9.0` | Imagen 3 generation |
| `pandas`, `pillow`, `pydantic`, `python-dotenv` | various | Supporting libraries |

Python: `^3.10`

## Running Locally

```bash
gcloud auth application-default login

# ADK dev UI
adk web

# Direct run
adk run image_scoring
```

## Deploying to Agent Engine

```bash
cd image_scoring
poetry build
python deploy.py
```

Update `PROJECT_ID`, `LOCATION`, and `STAGING_BUCKET` in `deploy.py` before running.

## Testing a Deployed Instance

```bash
python -m testclient.remote_test
```

Edit hardcoded values in `remote_test.py`:
- `PROJECT_ID`, `LOCATION`, `STAGING_BUCKET`
- `REASONING_ENGINE_ID` (from deploy output)

## Related READMEs

- [image_scoring/](./image_scoring/README.md) — Core agent package details
- [image_scoring_adk_a2a_server/remote_a2a/image_scoring/](./image_scoring_adk_a2a_server/remote_a2a/image_scoring/README.md) — A2A server variant
