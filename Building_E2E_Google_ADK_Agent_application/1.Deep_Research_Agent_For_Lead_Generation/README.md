# Deep Research Agent for Lead Generation

Automates B2B lead research using Gemini on Vertex AI. Given an industry and optional location, the agent searches for companies, performs deep research on each, extracts structured contact information, and exports CSV and JSON reports.

## What It Does

1. **Search companies** — Finds companies in a target industry using Gemini
2. **Deep research** — Generates detailed profiles (overview, decision makers, news, tech stack, funding, lead score, outreach strategy)
3. **Extract contacts** — Pulls structured contact info (email, phone, LinkedIn, decision maker)
4. **Export reports** — Writes timestamped CSV and JSON files to `outputs/`

## Key Files

| File | Description |
|------|-------------|
| `research_agent.py` | Main application — `ResearchAgent` class and interactive CLI |
| `test_agent.py` | Automated end-to-end test script |
| `requirements.txt` | Python dependencies |
| `leads_report_*.csv` | Sample CSV output |
| `research_summary_*.json` | Sample JSON output with full research history |

## Requirements

```
google-genai>=0.2.0
google-cloud-aiplatform>=1.38.0
requests>=2.31.0
beautifulsoup4>=4.12.0
pandas>=2.0.0
python-dotenv>=1.0.0
```

## Setup

```bash
python -m venv venv
venv\Scripts\activate          # Windows
pip install -r requirements.txt
```

Create a `.env` file:

```env
GOOGLE_CLOUD_PROJECT=your-project-id
GOOGLE_CLOUD_LOCATION=us-central1
MODEL_NAME=gemini-2.5-flash-exp
```

Authenticate with Google Cloud:

```bash
gcloud auth application-default login
```

## Usage

**Interactive mode** (prompts for industry and location):

```bash
python research_agent.py
```

**Automated test**:

```bash
python test_agent.py
```

Reports are saved to `outputs/` (or `outputs/test/` when running `test_agent.py`).

## Workflow

```
User input (industry, location)
    ↓
search_companies()         → Gemini → parsed company list
    ↓
deep_research_company()    → Gemini → detailed research (×5 companies)
    ↓
extract_contact_info()     → Gemini → structured JSON contacts
    ↓
generate_lead_report()     → CSV + JSON to outputs/
```

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `GOOGLE_CLOUD_PROJECT` | Yes | — | GCP project ID |
| `GOOGLE_CLOUD_LOCATION` | No | `us-central1` | Vertex AI region |
| `MODEL_NAME` | No | `gemini-2.5-flash-exp` | Gemini model |

## Tech Stack

Python 3.10+, `google-genai`, Vertex AI, `python-dotenv`
