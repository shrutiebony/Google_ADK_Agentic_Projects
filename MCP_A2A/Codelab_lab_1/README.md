### Currency Agent — MCP + A2A Integration

**Currency Agent** is an intelligent AI agent built using **Google ADK**, **FastMCP**, and the **A2A Protocol**.
It can fetch **real-time currency exchange rates** via an MCP tool and expose itself as an **A2A server**, enabling seamless communication with other agents or clients.


---

##### Overview

This project demonstrates how to:

* Build an **MCP (Model Context Protocol) server** using **FastMCP**.
* Deploy the MCP server to **Google Cloud Run**.
* Create a **currency-conversion AI agent** using **Google ADK** and **Gemini**.
* Expose the agent via the **A2A Protocol** to allow agent-to-agent communication.
* Test the A2A interface using an **A2AClient**.

The agent retrieves live exchange rates and answers questions like:

> “How much is 100 USD in CAD?”

---

##### Architecture

1. **FastMCP Server**
   Exposes a lightweight tool `get_exchange_rate` that fetches real-time currency data from the Frankfurter API.

2. **ADK Agent**
   Uses Gemini (`gemini-2.5-flash`) and the MCP tool to handle currency conversion queries.

3. **A2A Protocol**
   Converts the agent into a fully discoverable and callable service through `to_a2a()`.

4. **Testing**
   The `test_client` script uses the **A2A Python SDK** to send messages and verify responses.

---

##### Features

**MCP Integration:** Real-time API calls via FastMCP tools.
**Gemini-Powered Reasoning:** Uses Google ADK for LLM orchestration.
**Cloud Run Ready:** Easily deploy the MCP server in a scalable environment.
**A2A Interoperability:** Connect and collaborate with other agents over open standards.
**Testable Setup:** Includes A2A client test suite to validate responses.

---

##### Workflow Summary

1. Set up dependencies and environment variables (`.env` file).
2. Run the local MCP server to expose the `get_exchange_rate` tool.
3. Deploy the server to Cloud Run (optional).
4. Start the ADK agent connected to the MCP server.
5. Expose the agent as an A2A server.
6. Verify with the **A2A Agent Card** endpoint.
7. Test communication with the **A2A client**.

---

##### Endpoints

* **MCP Server:** `http://127.0.0.1:8080/mcp`
* **A2A Agent Card:**

  * `http://127.0.0.1:10000/.well-known/agent.json`
  * or `http://127.0.0.1:10000/.well-known/agent-card.json`

---

## Concepts Demonstrated

* Model Context Protocol (MCP)
* Google ADK & Gemini API usage
* Agent2Agent (A2A) Protocol
* Cloud Run Deployment
* Agent Interoperability

---

##### Tech Stack

* **Language:** Python 3.12
* **Frameworks:** FastMCP, Google ADK
* **AI Model:** Gemini 2.5 Flash
* **Deployment:** Google Cloud Run
* **Protocol:** Agent2Agent (A2A)
* **Testing:** A2A Python SDK


