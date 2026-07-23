# Google ADK Agentic Projects

A curated collection of end-to-end **agentic AI** projects built with the [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/), **Gemini**, **MCP (Model Context Protocol)**, and **A2A (Agent-to-Agent)** communication. Each folder is a self-contained learning or production-style implementation with source code, configuration, sample outputs, and walkthrough documentation.

---

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Projects at a Glance](#projects-at-a-glance)
- [Folder Details](#folder-details)
  - [Building_E2E_Google_ADK_Agent_application](#building_e2e_google_adk_agent_application)
  - [Build_Full_stack_E2E_Agentic_Multimodal_Application_with_Agentic_RAG](#build_full_stack_e2e_agentic_multimodal_application_with_agentic_rag)
  - [MCP_and_A2A](#mcp_and_a2a)
  - [Modern_AI_with_unsloth.ai](#modern_ai_with_unslothai)
  - [Eemrging_Agentic_AI_Architectures_paper](#eemrging_agentic_ai_architectures_paper)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Resources & References](#resources--references)

---

## Overview

This repository demonstrates how to design, build, and deploy intelligent agents that go beyond simple chatbots. The projects cover:

- **Single-agent workflows** — research, debugging, code review, and tool-augmented reasoning
- **Multi-agent systems** — orchestrated pipelines and agent-to-agent collaboration
- **Full-stack applications** — multimodal UIs, APIs, databases, and cloud deployment
- **Modern LLM training** — fine-tuning, LoRA, reinforcement learning, and continued pretraining

Implementations are based on Google Codelabs, Google Cloud samples, ADK reference patterns, and hands-on experimentation with production-oriented tooling.

---

## Repository Structure

```
Google_ADK_Agentic_Projects/
├── Building_E2E_Google_ADK_Agent_application/     # Four ADK agent implementations
├── Build_Full_stack_E2E_Agentic_Multimodal_Application_with_Agentic_RAG/  # Expense assistant (full stack)
├── MCP_and_A2A/                                   # Three Google Codelab projects
├── Modern_AI_with_unsloth.ai/                     # LLM training notebooks (Unsloth)
├── Eemrging_Agentic_AI_Architectures_paper/       # Research paper & presentation links
└── README.md
```

---

## Projects at a Glance

| Folder | Focus | Key Technologies |
|--------|-------|------------------|
| `Building_E2E_Google_ADK_Agent_application` | Four standalone ADK agents | ADK, Gemini, MCP, Gemini CLI |
| `Build_Full_stack_E2E_Agentic_Multimodal_Application_with_Agentic_RAG` | Multimodal expense tracker | ADK, Gemini 2.5, Firestore, FastAPI, Gradio, Cloud Run |
| `MCP_and_A2A` | Multi-agent codelabs (3 labs) | ADK, MCP, A2A, Agent Engine, Cloud Run |
| `Modern_AI_with_unsloth.ai` | LLM training workflows | Unsloth, LoRA, DPO, GRPO, CPT |
| `Eemrging_Agentic_AI_Architectures_paper` | Agent architecture research | Paper, article, slides, video |

---

## Folder Details

### Building_E2E_Google_ADK_Agent_application

End-to-end ADK agent implementations for real-world developer workflows. See [readme.md](./Building_E2E_Google_ADK_Agent_application/readme.md) for full details.

| Subfolder | Description | Key Files |
|-----------|-------------|-----------|
| **`1.Deep_Research_Agent_For_Lead_Generation`** | Automates multi-step company research for lead generation — search, enrichment, ranking, and structured CSV/JSON report output. | `research_agent.py`, `test_agent.py`, `requirements.txt` |
| **`2.Advanced_Tool_Agent_Using_Gemini_CLI`** | Exposes the Gemini CLI as an ADK tool so a reasoning agent can run command-line code generation and analysis tasks. | `tool_agent.py`, `quick_test.py`, `test_prompts.md` |
| **`3.MCP_Tools_Agent_For_Bug_Assistance`** | MCP-powered debugging assistant that analyzes code, detects bugs, suggests fixes, and can generate patches. | `bug_assistant.py`, `sample_buggy_code.py`, [README](./Building_E2E_Google_ADK_Agent_application/3.MCP_Tools_Agent_For_Bug_Assistance/README.md) |
| **`4.Production_Quality_Code_Review_Assistant`** | Production-grade code reviewer with sequential and loop-based sub-agent pipelines for analysis, style checking, testing, feedback synthesis, and automated fixing. | `agent.py`, `agent_engine_app.py`, `services.py`, `tools.py`, `config.py` |

**Concepts demonstrated:** ADK pipelines, tool registration, MCP integration, Gemini CLI tooling, multi-stage agent orchestration (`SequentialAgent`, `LoopAgent`), and Agent Engine deployment patterns.

---

### Build_Full_stack_E2E_Agentic_Multimodal_Application_with_Agentic_RAG

A complete **Personal Expense Assistant** that accepts receipt images, extracts structured expense data, stores embeddings in Firestore, and answers natural-language queries through a chat interface. See [README.md](./Build_Full_stack_E2E_Agentic_Multimodal_Application_with_Agentic_RAG/README.md).

| Component | File(s) | Role |
|-----------|---------|------|
| **Backend (FastAPI)** | `backend.py`, `schema.py`, `utils.py` | Serves the ADK agent, manages sessions and artifacts |
| **Frontend (Gradio)** | `frontend.py` | Chat UI with image upload and preview |
| **Agent logic** | `expense_manager_agent.zip` | ADK agent with multimodal receipt parsing and vector search |
| **Configuration** | `settings.py`, `settings.yaml` | Pydantic settings and YAML config |
| **Deployment** | `Dockerfile.txt`, `supervisord.conf` | Single-container Cloud Run deployment |
| **Logging** | `logger.py` | Application logging |

**Architecture:** User → Gradio UI → FastAPI → ADK Agent (Gemini 2.5) → Firestore (vector search) + Cloud Storage (receipt images).

**Features:** Multimodal receipt ingestion, automated metadata extraction, vector similarity search, conversation-aware follow-ups, and cloud-native deployment.

---

### MCP_and_A2A

Three Google Codelab implementations covering **ADK**, **MCP**, and **A2A** agent communication. See [README.md](./MCP_and_A2A/README.md).

#### `Codelab_lab_1/` — Currency Agent (MCP + A2A)

Builds a currency conversion agent using FastMCP, Google ADK, and the A2A protocol.

```
Codelab_lab_1/
├── README.md
└── currency_agentic_system/
    ├── agent.py          # ADK agent with MCP tool integration
    └── test_client.py    # A2A client test suite
```

- **MCP tool:** `get_exchange_rate` (Frankfurter API)
- **Model:** Gemini 2.5 Flash
- **Endpoints:** MCP server at `/mcp`, A2A agent card at `/.well-known/agent.json`

#### `Codelab_lab_2/` — Multi-Agent Image Scoring System

Creates and deploys a multi-agent image generation and scoring pipeline with A2A communication.

```
Codelab_lab_2/
├── README.md
├── Bicycle.png / Waterfall.png    # Sample images
└── multi_agentic_system/
    ├── image_scoring/               # Local image scoring agent
    │   ├── agent.py
    │   ├── checker_agent.py
    │   └── sub_agents/              # prompt, image, scoring sub-agents
    ├── image_scoring_adk_a2a_server/  # A2A server wrapper
    │   ├── a2a_agent.py
    │   └── remote_a2a/image_scoring/
    └── testclient/
        └── remote_test.py           # Remote A2A client tests
```

- **Sub-agents:** Prompt agent, Imagen agent (image generation), Scoring agent, Checker agent
- **Tools:** Image generation, policy fetching, score setting, loop conditions
- **Reference:** [Google Codelab – Create Multi-Agents with ADK & A2A](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)

#### `Codelab_lab_3/` — Purchasing Concierge (A2A Action Engine)

A purchasing concierge that delegates orders to remote burger and pizza seller agents over A2A.

```
Codelab_lab_3/
├── README.md
├── purchasing_concierge_ui.py       # Gradio web interface
├── deploy_to_agent_engine.py        # Vertex AI Agent Engine deployment
├── purchasing_concierge/
│   ├── agent.py
│   ├── purchasing_agent.py
│   └── remote_agent_connection.py   # A2A client connections
└── remote_seller_agents/
    ├── burger_agent/                # CrewAI-based A2A server (Cloud Run)
    └── pizza_agent/                 # LangGraph-based A2A server (Cloud Run)
```

- **Concierge:** Google ADK on Vertex AI Agent Engine
- **Burger Agent:** CrewAI framework
- **Pizza Agent:** LangGraph framework
- **UI:** Gradio chat interface

---

### Modern_AI_with_unsloth.ai

A series of Colab-ready notebooks exploring modern LLM training with [Unsloth.ai](https://unsloth.ai/). See [README.md](./Modern_AI_with_unsloth.ai/README.md).

| Notebook | Topic | Description |
|----------|-------|-------------|
| `1_Full_Finetuning_with_a_small_model.ipynb` | Supervised Fine-Tuning (SFT) | End-to-end full fine-tuning on small/quantized models (e.g., smolLM2 135M, Gemma 3 1B) |
| `2_LORA_parameter_efficient_Finetune.ipynb` | LoRA / PEFT | Parameter-efficient fine-tuning with memory and speed comparisons vs. full SFT |
| `3_ReinforcementLearning_Using_Dataset(DPO_RLHF).ipynb` | Preference RL (DPO/RLHF) | Training with preferred/rejected response pairs and reward tracking |
| `4_ReinforcementLearning_with_GRPO.ipynb` | GRPO Reasoning RL | Chain-of-thought reasoning optimization on problem-only datasets |
| `5_ContinuedPretraining_HindiLanguage.ipynb` | Continued Pretraining (CPT) | Extending model vocabulary and knowledge for new languages/domains |

**Models used:** smolLM2, Gemma 2/3, Llama 3, Mistral, Phi-3, Qwen2, TinyLlama (open weights).

---

### Eemrging_Agentic_AI_Architectures_paper

Research and educational resources on emerging agentic AI architectures — how agents plan, reason, use tools, and collaborate. See [readme.md](./Eemrging_Agentic_AI_Architectures_paper/readme.md).

| Resource | Link |
|----------|------|
| **Paper** | [arXiv:2404.11584](https://arxiv.org/pdf/2404.11584) |
| **Medium Article** | [Agents Are the Next Big Shift in AI](https://medium.com/@shrutiebony/agents-are-the-next-big-shift-in-ai-just-b229e9a2607d) |
| **Slide Deck** | [Google Slides](https://docs.google.com/presentation/d/1ZeZPXSdOqODIPVtWBIkPsfri_sfw9Mr1-kLX5P6PD-w/edit?usp=sharing) |
| **Video** | [YouTube Walkthrough](https://youtu.be/u56521d-Nug) |

---

## Technology Stack

| Layer | Technologies |
|-------|-------------|
| **Agent Framework** | Google ADK, Gemini 2.0/2.5 Flash & Pro |
| **Protocols** | MCP (Model Context Protocol), A2A (Agent-to-Agent) |
| **Cloud & Deployment** | Google Cloud Run, Vertex AI Agent Engine, Cloud Storage |
| **Data** | Firestore (vector search), AlloyDB |
| **Backend** | FastAPI, Uvicorn, Python 3.10+ |
| **Frontend** | Gradio, Streamlit |
| **LLM Training** | Unsloth, LoRA, DPO, GRPO |
| **Other Frameworks** | CrewAI, LangGraph, FastMCP |

---

## Prerequisites

- **Python 3.10+** (3.12 recommended for MCP/A2A labs)
- **Google Cloud Project** with billing enabled (for Vertex AI, Cloud Run, Firestore)
- **API access:** Gemini API or Vertex AI credentials
- **Optional:** `gcloud` CLI, Docker, `uv` package manager (Codelab 3)
- **Optional:** GPU/Colab for Unsloth notebooks

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/shrutiebony/Google_ADK_Agentic_Projects.git
cd Google_ADK_Agentic_Projects
```

### 2. Choose a project

Navigate to the folder that matches your interest. Each project has its own `README.md` (or `readme.md`) with setup instructions.

### 3. Set up a virtual environment

```bash
python -m venv venv
# Windows
venv\Scripts\activate
# macOS/Linux
source venv/bin/activate
```

### 4. Install dependencies

Most Python projects include a `requirements.txt` or `pyproject.toml`. Example:

```bash
cd Building_E2E_Google_ADK_Agent_application/1.Deep_Research_Agent_For_Lead_Generation
pip install -r requirements.txt
```

### 5. Configure environment variables

Copy `.env.example` to `.env` where provided, and set:

- `GOOGLE_CLOUD_PROJECT`
- `GOOGLE_CLOUD_LOCATION`
- Gemini / Vertex AI credentials

For Google Cloud authentication:

```bash
gcloud auth application-default login
```

### 6. Run locally or deploy

- **Local:** Run the main Python script or start the ADK dev UI
- **Cloud:** Use included Dockerfiles, `deploy_to_agent_engine.py`, or Cloud Run deploy scripts

---

## Resources & References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [Agent-to-Agent (A2A) Protocol](https://google.github.io/A2A/)
- [Google Cloud – Tools Make an Agent](https://cloud.google.com/blog/topics/developers-practitioners/tools-make-an-agent-from-zero-to-assistant-with-adk)
- [Unsloth Documentation](https://docs.unsloth.ai/)

---

## License

Individual projects may reference Google Codelab starter code and third-party libraries. Refer to each subfolder's documentation for specific licensing and attribution details.
