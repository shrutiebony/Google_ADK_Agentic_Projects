### Google ADK Intelligent Agents – End-to-End Implementations

This repository contains five fully developed intelligent agents built using the Google Agent Development Kit (ADK) and the Gemini ecosystem.
Each agent demonstrates a real-world workflow, integrates advanced tools, and is implemented from start to finish using Google Cloud and MCP.

##### Overview

This collection showcases practical ADK applications designed to help developers understand how to build, orchestrate, and deploy AI-driven agents.
Every project includes:

###### Full source code

###### ADK pipeline configurations

Tool integration logic (MCP, Gemini CLI, AlloyDB)

Screenshots, logs, and sample outputs

###### A dedicated walkthrough video

All implementations are based on Google Codelabs, Google Cloud blog samples, and real ADK reference projects.

##### Projects Included
###### 1. Deep Research Lead Generation Agent

Automates multi-step research using ADK pipelines.
Performs data extraction, insight generation, ranking, enrichment, and structured report creation.

###### 2. Advanced Tool Agent (Gemini CLI Integrated)

Shows how to expose the Gemini CLI as an ADK tool.
Useful for executing command-line tasks through a reasoning agent.

###### 3. MCP Tools Bug Assistance Agent

A debugging assistant capable of analyzing logs, reading files through MCP, detecting issues, and suggesting actionable fixes.

###### 4. Code Review Assistant

Production-grade reviewer that evaluates code for structure, clarity, correctness, and engineering best practices.
Implements structured suggestions and multi-step evaluation.

###### 5. E-Commerce Agent (ADK + MCP + AlloyDB)

A conversational shopping assistant that retrieves product data using AlloyDB + MCP and responds to user queries using Gemini.

Repository Structure
/
├── agent_lead_generation/
├── agent_gemini_cli_tool/
├── agent_mcp_bug_assistant/
├── agent_code_review/
├── agent_ecommerce/
├── video_walkthrough/
└── README.md


##### Each project directory includes:

src/ — Main logic

config/ — ADK / tool configuration

assets/ — Screenshots, logs, examples

README.md — Setup + usage instructions

.env.example — Required environment variables

##### Tech Stack

The agents in this repository utilize:

Google Agent Development Kit (ADK)

Gemini models (2.0 Flash / Pro / Experimental)

Google Cloud Run

MCP Tools

Gemini CLI

AlloyDB

Python 3.10+

##### Streamlit for optional UI demos

Setup & Installation
###### 1. Clone the repository
git clone <your-repo-url>
cd <repo>

###### 2. Create a virtual environment
python3 -m venv venv
source venv/bin/activate

###### 3. Install dependencies

Each agent includes its own requirements.txt file.

###### 4. Configure environment variables

Follow each agent’s README.md for your ADK, Gemini, and Google Cloud settings.

###### 5. Run or deploy

Agents can be run locally or deployed to Cloud Run using the included build/deploy scripts.

##### Execution Flow

Although each project differs, the general workflow is:

Initialize ADK

Load and register tools

Run the pipeline

Inspect logs and outputs

Validate tool interactions

Deploy for production use (optional)

Video Walkthroughs

All demonstration videos are located under:


