# Building End-to-End Google ADK Agent Applications

Four standalone intelligent agent projects built with the Google Gemini ecosystem. Each subfolder demonstrates a different agent pattern — from prompt-driven research pipelines to full Google ADK multi-agent orchestration.

## Projects

| # | Folder | Description |
|---|--------|-------------|
| 1 | [Deep Research Agent for Lead Generation](./1.Deep_Research_Agent_For_Lead_Generation/) | Multi-step B2B lead research with CSV/JSON export |
| 2 | [Advanced Tool Agent Using Gemini CLI](./2.Advanced_Tool_Agent_Using_Gemini_CLI/) | Function-calling agent with Gemini CLI tools |
| 3 | [MCP Tools Agent for Bug Assistance](./3.MCP_Tools_Agent_For_Bug_Assistance/) | Code analysis and bug detection assistant |
| 4 | [Production Quality Code Review Assistant](./4.Production_Quality_Code_Review_Assistant/) | Multi-agent code review and fix pipeline (ADK) |

## Tech Stack

- **Google Gemini** via Vertex AI (`google-genai` SDK)
- **Google ADK** (project 4 only)
- **Python 3.10+**
- **MCP-style tool declarations** (projects 2–3)

## Shared Prerequisites

1. A Google Cloud project with **Vertex AI API** enabled
2. Application Default Credentials:
   ```bash
   gcloud auth application-default login
   ```
3. A Python virtual environment:
   ```bash
   python -m venv venv
   venv\Scripts\activate        # Windows
   source venv/bin/activate     # macOS/Linux
   ```

## Shared Environment Variables

Most projects use these variables (set in a `.env` file in each subfolder):

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Yes | — | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | No | `us-central1` | Vertex AI region |
| `MODEL_NAME` | No | `gemini-2.5-flash-exp` | Gemini model ID |

## Getting Started

Each subfolder is self-contained with its own source code and `requirements.txt`. Navigate into the project you want to run and follow its README:

```bash
cd 1.Deep_Research_Agent_For_Lead_Generation
pip install -r requirements.txt
python research_agent.py
```

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│  Projects 1–3: google-genai + Vertex AI function calling   │
│  Project 4:    google-adk multi-agent pipelines              │
└─────────────────────────────────────────────────────────────┘
         │                    │                    │
         ▼                    ▼                    ▼
   Lead Research        Tool Agent           Bug Assistant
   (prompt pipeline)    (CLI tools)          (MCP-style tools)
                                                    │
                                                    ▼
                                          Code Review Assistant
                                          (SequentialAgent +
                                           LoopAgent pipelines)
```

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Tools Make an Agent (Google Cloud Blog)](https://cloud.google.com/blog/topics/developers-practitioners/tools-make-an-agent-from-zero-to-assistant-with-adk)
- [Vertex AI Gemini](https://cloud.google.com/vertex-ai/generative-ai/docs)
