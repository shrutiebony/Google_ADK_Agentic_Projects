# Advanced Tool Agent Using Gemini CLI

A function-calling agent that exposes Gemini CLI capabilities as tools. The agent uses Gemini's native function calling to decide when to generate code or analyze existing code, then returns a synthesized response.

## What It Does

The agent registers two tools with Gemini:

- **`generate_code_with_cli`** — Generates code from a natural-language prompt
- **`analyze_code_with_cli`** — Analyzes code for security, performance, and style issues

The model selects the appropriate tool, executes it, and incorporates the result into a final answer.

> **Note:** The CLI execution layer is simulated via a Python subprocess stub. In production, this would invoke the real `gemini` CLI (`gemini chat --model {model} "{prompt}"`).

## Key Files

| File | Description |
|------|-------------|
| `tool_agent.py` | Main app — `GeminiCLITool`, `AdvancedToolAgent`, CLI entry point |
| `quick_test.py` | Runs three automated test cases |
| `test_prompts.md` | Test prompts, expected behavior, and troubleshooting |
| `requirements.txt` | Python dependencies |

## Requirements

```
google-genai>=0.2.0
google-generativeai>=0.3.0
google-cloud-aiplatform>=1.38.0
python-dotenv>=1.0.0
pydantic>=2.0.0
```

## Setup

```bash
pip install -r requirements.txt
```

Create a `.env` file:

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
MODEL_NAME=gemini-2.5-flash-exp
```

```bash
gcloud auth application-default login
```

## Usage

**Demo mode** (runs three automated requests):

```bash
python tool_agent.py
```

**Single request**:

```bash
python tool_agent.py --request "Generate a Python REST API with authentication"
```

**Interactive chat**:

```bash
python tool_agent.py --interactive
```

**Quick test suite**:

```bash
python quick_test.py
```

## Workflow

```
User request
    ↓
Gemini generate_content(tools=[generate_code, analyze_code])
    ↓
Function call selected → GeminiCLITool.execute()
    ↓
Follow-up generate_content with FunctionResponse
    ↓
Final text response
```

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Yes | — | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | No | `us-central1` | Vertex AI region |
| `MODEL_NAME` | No | `gemini-2.5-flash-exp` | Model for agent reasoning |

## Tech Stack

Python 3.10+, `google-genai` (Vertex AI function calling), `argparse`
