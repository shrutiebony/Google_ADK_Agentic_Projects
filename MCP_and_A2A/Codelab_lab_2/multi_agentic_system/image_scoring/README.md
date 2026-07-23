# Image Scoring Agent

The core ADK agent package for the multi-agent image generation and scoring pipeline. Composes a `LoopAgent` containing a `SequentialAgent` (prompt → generate → score) and a checker agent that controls loop termination.

## Agent Composition

```python
# agent.py
LoopAgent (image_scoring)
├── SequentialAgent (image_generation_scoring_agent)
│   ├── prompt_agent
│   ├── imagen_agent
│   └── scoring_agent
└── checker_agent
```

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | Composes `LoopAgent` + `SequentialAgent`; exports `root_agent` |
| `checker_agent.py` | Termination checker using `check_tool_condition` tool |
| `config.py` | Environment-based config (GCS, thresholds, models) |
| `policy.json` | Image content and composition rules for prompt generation and scoring |
| `deploy.py` | Vertex AI Agent Engine deployment script |
| `prompt.py` | Checker agent system prompt |

### Sub-Agents

| Path | Description |
|------|-------------|
| `sub_agents/prompt/prompt_agent.py` | Generates Imagen3 prompts from news text |
| `sub_agents/prompt/prompt.py` | Detailed Imagen3 prompt instructions |
| `sub_agents/image/imagen_agent.py` | Image generation orchestrator |
| `sub_agents/image/tools/image_generation_tool.py` | Calls Imagen 3 via `google.genai` |
| `sub_agents/scoring/scoring_agent.py` | Scores images against policy |
| `sub_agents/scoring/tools/get_images_tool.py` | Loads generated image artifact |
| `sub_agents/scoring/tools/set_score_tool.py` | Writes score to session state |
| `sub_agents/tools/fetch_policy_tool.py` | Reads `policy.json` |

### Tools

| Path | Description |
|------|-------------|
| `tools/loop_condition_tool.py` | Increments loop counter; sets `escalate=True` when threshold met |

## Loop Logic

1. Each iteration runs: prompt → generate → score
2. `check_tool_condition` reads `total_score` from session state
3. Loop stops when `total_score > SCORE_THRESHOLD` or iterations reach `MAX_ITERATIONS`
4. Each iteration gets a unique session ID via `set_session` callback

## Configuration

Set via environment variables (see `config.py`):

| Variable | Default | Description |
|----------|---------|-------------|
| `GCS_BUCKET_NAME` | — | Optional GCS bucket for generated images |
| `SCORE_THRESHOLD` | `45` | Minimum acceptable score |
| `MAX_ITERATIONS` | `1` | Max regeneration loops |
| `IMAGEN_MODEL` | `imagen-3.0-generate-002` | Image generation model |
| `GENAI_MODEL` | `gemini-2.0-flash` | LLM for prompt/scoring agents |

## Usage

```bash
# Local development
adk web
adk run image_scoring

# Deploy to Agent Engine
poetry build
python deploy.py
```

## Policy File

`policy.json` defines content and composition rules used by both the prompt agent (to guide generation) and the scoring agent (to evaluate results). Edit this file to change image quality criteria.

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Create Multi-Agents Codelab](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)
