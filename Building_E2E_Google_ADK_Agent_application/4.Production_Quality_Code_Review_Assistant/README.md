# Production Quality Code Review Assistant

A multi-agent code review system built with the **Google ADK** framework. It orchestrates sequential and loop-based agent pipelines to analyze Python code, check style, run tests, synthesize feedback, and optionally attempt automated fixes.

## What It Does

The system defines three orchestration layers:

1. **Review Pipeline** (`SequentialAgent`) — code analysis → style check → test run → feedback synthesis
2. **Fix Pipeline** (`SequentialAgent` + `LoopAgent`) — code fixer → fix test runner → fix validator (up to 3 iterations) → fix synthesizer
3. **Root Agent** (`CodeReviewAssistant`) — delegates to review or fix pipelines based on user input

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | Root agent and pipeline orchestration (`SequentialAgent`, `LoopAgent`) |
| `agent_engine_app.py` | Wraps `root_agent` in `vertexai.agent_engines.AdkApp` for Agent Engine deployment |
| `config.py` | Pydantic `AgentConfig` — models, grading weights, GCP settings |
| `constants.py` | `StateKeys` — shared state key definitions across agents and tools |
| `tools.py` | ADK async tools: AST analysis, PEP 8 checking, grading/fix report tools |
| `services.py` | Artifact and session service factories (GCS, in-memory, Cloud SQL) |
| `requirements.txt` | Full pinned dependency lockfile |

## Architecture

```
User submits Python code
    ↓
CodeReviewAssistant (root_agent)
    ↓
CodeReviewPipeline (SequentialAgent)
    ├── code_analyzer_agent       → analyze_code_structure (AST)
    ├── style_checker_agent       → check_code_style (pycodestyle)
    ├── test_runner_agent         → generates/runs tests
    └── feedback_synthesizer_agent → final feedback
    ↓
CodeFixPipeline (SequentialAgent)
    └── FixAttemptLoop (LoopAgent, max 3 iterations)
        ├── code_fixer_agent
        ├── fix_test_runner_agent
        └── fix_validator_agent
    └── fix_synthesizer_agent → final fix report
```

## Requirements

Key packages from `requirements.txt`:

```
google-adk==1.14.1
google-genai==1.38.0
google-cloud-aiplatform[adk,agent-engines]>=1.111
pydantic==2.11.9
pycodestyle==2.11.1
fastapi==0.117.1
python-dotenv==1.1.1
```

## Setup

```bash
pip install -r requirements.txt
gcloud auth application-default login
```

## Project Status

> **Incomplete:** `agent.py` imports sub-agents from `code_review_assistant.sub_agents.*`, but those modules are not included in this folder. The expected package structure is:

```
code_review_assistant/
├── sub_agents/
│   ├── review_pipeline/
│   │   ├── code_analyzer.py
│   │   ├── style_checker.py
│   │   ├── test_runner.py
│   │   └── feedback_synthesizer.py
│   └── fix_pipeline/
│       ├── code_fixer.py
│       ├── fix_test_runner.py
│       ├── fix_validator.py
│       └── fix_synthesizer.py
```

The folder must be restructured or installed as an editable package named `code_review_assistant` for imports to resolve.

## Intended Usage

Once sub-agents are in place:

```bash
adk run code_review_assistant
```

**Agent Engine deployment** (via `agent_engine_app.py`):

```python
from vertexai import agent_engines
from code_review_assistant.agent import root_agent

app = agent_engines.AdkApp(agent=root_agent, enable_tracing=True)
```

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Auto-detected | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | `us-central1` | GCP region |
| `GOOGLE_GENAI_USE_VERTEXAI` | `true` | Use Vertex AI vs AI Studio |
| `WORKER_MODEL` | `gemini-2.5-flash` | Fast analysis model |
| `CRITIC_MODEL` | `gemini-2.5-pro` | Advanced feedback model |
| `ARTIFACT_BUCKET` | — | GCS bucket for reports (else in-memory) |
| `STAGING_BUCKET` | — | GCS staging for Agent Engine |
| `PASSING_SCORE_THRESHOLD` | `0.8` | Grade pass threshold |
| `STYLE_WEIGHT` | `0.3` | Grading weight |
| `TEST_WEIGHT` | `0.5` | Grading weight |
| `STRUCTURE_WEIGHT` | `0.2` | Grading weight |
| `MAX_GRADING_ATTEMPTS` | `3` | Max grading retries |

## Tools

Defined in `tools.py`:

- `analyze_code_structure` — AST-based Python analysis
- `check_code_style` — PEP 8 via `pycodestyle`
- `search_past_feedback` — Retrieves prior review feedback
- `update_grading_progress` / `save_grading_report` — Grading workflow
- `validate_fixed_style` / `compile_fix_report` / `save_fix_report` — Fix workflow
- `exit_fix_loop` — Signals loop termination

## Tech Stack

Python 3.10+, Google ADK, Vertex AI Agent Engine, GCS, Cloud SQL (optional), `pycodestyle`, `pydantic-settings`
