# Image Scoring A2A Server

A copy of the image scoring agent configured to run as an **A2A server**, enabling other ADK agents to call it remotely via the Agent-to-Agent protocol.

## Purpose

This package mirrors the core `image_scoring/` agent but is set up for A2A exposure. The parent orchestrator in `image_scoring_adk_a2a_server/a2a_agent.py` uses a `RemoteA2aAgent` to connect to this server.

## Relationship to `image_scoring/`

This directory is a near-identical copy of `../image_scoring/`. When making changes to agent logic, update both copies or consolidate them into a shared package.

## A2A Endpoints

Defined in `agent.json`:

| Endpoint | URL |
|----------|-----|
| Agent URL | `http://localhost:8001/a2a/image_scoring` |
| Agent Card | `http://localhost:8001/a2a/image_scoring/.well-known/agent.json` |

**Skills:** `generate_and_score_image`

**Output modes:** `image/png`, `text/plain`

## How to Run

### 1. Start this agent as an A2A server

```bash
cd image_scoring_adk_a2a_server/remote_a2a/image_scoring
adk web    # or adk run with A2A server config on port 8001
```

### 2. Connect from the parent orchestrator

The `a2a_agent.py` in the parent directory creates a `RemoteA2aAgent`:

```python
RemoteA2aAgent(
    agent_card="http://localhost:8001/a2a/image_scoring/.well-known/agent.json",
    timeout=300.0
)
```

## Key Files

| File | Description |
|------|-------------|
| `agent.py` | Same LoopAgent composition as core `image_scoring/` |
| `agent.json` | Static A2A agent card metadata |
| `config.py`, `policy.json` | Same configuration as core package |
| `sub_agents/`, `tools/` | Same sub-agent and tool structure |

## References

- [Google ADK Documentation](https://google.github.io/adk-docs/)
- [A2A Protocol](https://google.github.io/A2A/)
- [Create Multi-Agents Codelab](https://codelabs.developers.google.com/codelabs/create-multi-agents-adk-a2a)
- [Instavibe ADK Multi-Agents](https://codelabs.developers.google.com/instavibe-adk-multi-agents/instructions)
