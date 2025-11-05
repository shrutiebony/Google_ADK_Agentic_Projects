### Purchasing Concierge – Agent-to-Agent (A2A) Cloud Run Demo

This project demonstrates how to build a **Purchasing Concierge AI agent** that communicates with remote **Burger** and **Pizza Seller Agents** using the **Agent2Agent (A2A)** protocol on **Google Cloud**.
It showcases how multiple agents — each powered by different frameworks — can collaborate seamlessly using standardized A2A communication.


##### Overview

The system consists of three main agents:

**Purchasing Concierge** – The main AI assistant (A2A Client) that interacts with the user and delegates tasks.
**Burger Seller Agent** – An A2A Server built using the CrewAI framework that manages burger-related requests.
**Pizza Seller Agent** – An A2A Server built using the LangGraph framework that handles pizza orders.

The Concierge is deployed on **Vertex AI Agent Engine**, while the seller agents run on **Cloud Run**.
All agents communicate via A2A messages using standardized JSON structures.

---

##### Architecture

1. The user interacts with the **Purchasing Concierge** through a Gradio web app.
2. The Concierge discovers available remote agents using their public A2A cards.
3. It delegates specific tasks (e.g., fetching menus or placing orders) to the Burger and Pizza agents.
4. Each seller agent responds with structured results (menu, prices, confirmations).
5. The Concierge merges these responses and presents them back to the user naturally.

This architecture highlights the **A2A protocol’s** ability to connect independent AI agents across different services, frameworks, and platforms.

---

##### Key Technologies

| Component            | Technology                          | Purpose                                |
| -------------------- | ----------------------------------- | -------------------------------------- |
| Purchasing Concierge | Google ADK + Vertex AI Agent Engine | Orchestrator and A2A Client            |
| Burger Seller Agent  | CrewAI Framework                    | A2A Server for burgers                 |
| Pizza Seller Agent   | LangGraph Framework                 | A2A Server for pizzas                  |
| Communication Layer  | A2A SDK                             | Handles message passing between agents |
| Frontend UI          | Gradio                              | Chat-based user interface              |
| Hosting & Deployment | Google Cloud Run + Cloud Storage    | Serverless infrastructure              |

---

##### Workflow Summary

1. Set up a **Google Cloud Project** and enable billing.
2. Clone the starter repository and install dependencies using `uv`.
3. Deploy the **Burger Agent** and **Pizza Agent** on **Cloud Run**.
4. Add the **HOST_OVERRIDE** environment variable to each Cloud Run service so the agents advertise their correct public URLs.
5. Verify their A2A cards by visiting the `/.well-known/agent.json` routes.
6. Configure and deploy the **Purchasing Concierge** to **Vertex AI Agent Engine**.
7. Launch the **Gradio** web interface to start interacting.
8. Chat naturally with the Concierge — it will contact both seller agents to fulfill your request.

---

##### Example Conversation

* **User:** “Show me burger and pizza menu.”
  **Agent:** Displays menus from both seller agents.

* **User:** “I want to order one BBQ chicken pizza and one spicy cajun burger.”
  **Agent:** Sends two A2A tasks (one to each seller) and confirms both orders.

This demonstrates seamless collaboration between multiple agents in real time.

---

##### Cleanup After Testing

To prevent unwanted costs:

* Delete deployed services from **Cloud Run** and **Vertex AI Agent Engine**, or
* Delete the entire Google Cloud project when finished.
