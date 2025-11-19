### Personal Expense Assistant: Multimodal Receipt Intelligence with Gemini 2.5  

This repository contains a fully implemented multimodal expense-tracking system powered by Google's Agent Development Kit (ADK) and Gemini 2.5.
Users can upload receipt images, query their expense history, and retrieve contextual insights through a conversational interface.
All data—including metadata, receipts, and vector embeddings—is stored and served using GCP services.

This project demonstrates how to build and deploy a complete agentic application using Gemini, ADK, Firestore, FastAPI, Gradio, and Cloud Run.

#### 1\. Project Summary

The system provides an end-to-end workflow for expense understanding:

Upload any receipt image

Extract store name, date, line items, total cost, and categories

Store metadata and embeddings in Firestore (vector search enabled)

Retrieve expenses through natural-language queries

Display results in a unified chat interface

Support for contextual follow-ups and chained queries

The backend logic is built using a custom ADK agent, and the deployment bundles both frontend and backend in a single container.

#### 2\. System Architecture

The application consists of three cooperating components:

Agent Layer (ADK + Gemini 2.5)

Runs multimodal reasoning

Parses receipt photos

Generates metadata + embeddings

Performs contextual lookup and synthesis

Backend Layer (FastAPI)

Serves the ADK agent

Manages sessions and artifacts

Handles storage, vector search, and receipt retrieval

Frontend Layer (Gradio)

Chat interface

Image upload and preview

Query + response display

All components are deployed together on Cloud Run, using Supervisord as a process manager.

#### 3\. Technology Stack

Component	Technology
LLM Reasoning	Gemini 2.5 Flash (Vertex AI)
Agent Platform	Google ADK v1.18
Database	Firestore (Native Mode + Vector Search)
File Storage	Google Cloud Storage
Backend	FastAPI + Uvicorn
Frontend	Gradio 5.x
Deployment	Cloud Run + Docker + Supervisord
Configuration	Pydantic Settings + YAML

#### 4\. Core Features

Multimodal receipt ingestion (image → structured fields)

Automated storage of extracted data and receipt images

Vector-based similarity search for contextual expense queries

Metadata filters (stores, dates, categories, totals)

Conversation-aware reasoning

Cloud-native deployment with minimal setup

Unified full-stack prototype combining ADK + FastAPI + Gradio

#### 5\. Development Workflow

Initialize GCP project

Enable Vertex AI, Firestore, Cloud Run, Cloud Storage

Create Firestore database and a GCS bucket

Configure the agent

Define ADK agent tools

Add multimodal prompt

Implement Firestore + Storage handlers

Add callbacks to manage image bindings

Build frontend and backend

FastAPI route for ADK interaction

Gradio interface for chat and images

Local testing

Run the ADK dev UI

Validate extraction and database insertion

Deploy to Cloud Run

Build container

Run FastAPI + Gradio with Supervisord

Set environment variables

Interact with deployed instance

Upload receipts

Query expenses by date, store, or concept

Retrieve stored receipt images

