# MCP Tools Agent for Bug Assistance

An intelligent debugging assistant that analyzes source code using Gemini function calling with MCP-style tool declarations. It detects bugs, assigns severity levels, suggests fixes, and exports structured JSON reports.

Based on: [Tools Make an Agent from Zero to Assistant with ADK](https://cloud.google.com/blog/topics/developers-practitioners/tools-make-an-agent-from-zero-to-assistant-with-adk)

## What It Does

1. Reads source files (`.py`, `.js`, `.java`)
2. Sends code to Gemini with registered analysis tools
3. Executes tool calls locally (syntax analysis, bug pattern detection, fix generation)
4. Produces a natural-language analysis and a JSON bug report

## Tools

| Tool | Description |
|------|-------------|
| `analyze_code_syntax` | Parses Python code with `ast` for syntax and structural issues |
| `detect_common_bugs` | Pattern matching for exec/eval, password logging, missing except blocks, TODOs |
| `generate_fix` | Template-based fix suggestions for identified bugs |

> **Note:** Tools are implemented as Gemini `FunctionDeclaration`s executed locally — not via a separate MCP server process.

## Key Files

| File | Description |
|------|-------------|
| `bug_assistant.py` | Main app — `MCPBugAssistant` class and CLI |
| `sample_buggy_code.py` | Intentionally buggy sample for testing |
| `test_simple.py` | Creates a temp file, runs analysis, prints results |
| `demo_script.md` | Video demo walkthrough script |
| `bug_report.json` | Pre-generated sample report |

## Requirements

```
google-genai>=0.2.0
google-cloud-aiplatform>=1.38.0
python-dotenv>=1.0.0
```

## Setup

```bash
pip install google-genai google-cloud-aiplatform python-dotenv
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

**Demo** (creates and analyzes a sample file):

```bash
python bug_assistant.py
```

**Analyze a specific file**:

```bash
python bug_assistant.py --file sample_buggy_code.py
```

**Analyze a directory** (all `*.py` files recursively):

```bash
python bug_assistant.py --directory ./src
```

**Custom output path**:

```bash
python bug_assistant.py --file sample_buggy_code.py --output outputs/my_report.json
```

**Simple test**:

```bash
python test_simple.py
```

## Workflow

```
Source file(s)
    ↓
analyze_file() — read file, detect language
    ↓
Gemini generate_content with tools
    ↓
Execute function calls → aggregate results
    ↓
Follow-up Gemini call → natural-language analysis
    ↓
generate_report() → JSON to outputs/bug_report.json
```

## Output

Reports are saved as JSON with bug descriptions, severity levels, line numbers, and fix suggestions. A console summary is printed after each run.

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Yes | — | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | No | `us-central1` | Vertex AI region |
| `MODEL_NAME` | No | `gemini-2.5-flash-exp` | Gemini model |

## Video Walkthrough

[YouTube: MCP Bug Assistant Demo](https://youtu.be/4bjq91TVp-s)

## Tech Stack

Python 3.10+, `google-genai`, `ast`, `pathlib`, `argparse`
