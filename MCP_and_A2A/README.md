# MCP and A2A Codelabs

Three Google Codelab implementations demonstrating **Google ADK**, **MCP (Model Context Protocol)**, and **A2A (Agent-to-Agent)** communication for building, deploying, and orchestrating multi-agent systems.

## Codelabs

| Lab | Folder | Description |
|-----|--------|-------------|
| 1 | [Codelab_lab_1](./Codelab_lab_1/) | Currency Agent — MCP tool integration + A2A server |
| 2 | [Codelab_lab_2](./Codelab_lab_2/) | Multi-Agent Image Scoring — ADK pipeline + A2A + Agent Engine |
| 3 | [Codelab_lab_3](./Codelab_lab_3/) | Purchasing Concierge — A2A orchestration across Cloud Run agents |

## Repository Structure

```
MCP_and_A2A/
├── Codelab_lab_1/
│   └── currency_agentic_system/     # ADK agent consuming MCP tools via A2A
├── Codelab_lab_2/
│   └── multi_agentic_system/        # Image generation + scoring pipeline
│       ├── image_scoring/           # Core agent package
│       ├── image_scoring_adk_a2a_server/  # Remote A2A wrapper
│       └── testclient/              # Agent Engine test client
└── Codelab_lab_3/
    ├── purchasing_concierge/        # Orchestrator agent
    ├── remote_seller_agents/
    │   ├── burger_agent/            # CrewAI A2A server
    │   └── pizza_agent/             # LangGraph A2A server
    ├── purchasing_concierge_ui.py   # Gradio chat UI
    └── deploy_to_agent_engine.py    # Agent Engine deployment
```

## Concepts Demonstrated

- **ADK (Agent Development Kit)** — Creating and managing agents with sub-agent pipelines
- **MCP (Model Context Protocol)** — Exposing external tools to agents via standardized protocol
- **A2A (Agent-to-Agent)** — Secure, structured message exchange between independent agents
- **Agent Engine** — Cloud deployment and execution of ADK agents on Vertex AI
- **Multi-framework interoperability** — CrewAI, LangGraph, and ADK agents communicating via A2A

## Shared Prerequisites

- Python 3.10+ (3.12 for Codelab 3 seller agents)
- Google Cloud project with billing enabled
- Vertex AI API enabled
- Application Default Credentials:
  ```bash
  gcloud auth application-default login
  ```

## Getting Started

Each codelab is self-contained. Navigate to the lab folder and follow its README:

```bash
cd Codelab_lab_1
# Follow Codelab_lab_1/README.md
```

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [A2A Protocol](https://google.github.io/A2A/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [Create Multi-Agents with ADK & A2A Codelab](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)
- [Vertex AI Agent Engine](https://cloud.google.com/vertex-ai/generative-ai/docs/agent-engine/overview)
