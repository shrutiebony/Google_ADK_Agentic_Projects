### Modern AI Workflows with Unsloth.ai

A Multi-Notebook Exploration of Fine-Tuning, Reinforcement Learning, and Continued Pretraining

This repository contains a complete series of hands-on Colab notebooks demonstrating modern LLM training techniques using Unsloth.ai.
Each notebook focuses on a different training paradigm—fine-tuning, LoRA, RL from Preferences, GRPO reasoning RL, and continued pretraining—and includes:

A reproducible end-to-end workflow

Dataset preparation notes

Input/output examples

Training artifacts (logs, charts, checkpoints, evaluations)

#### 1. Project Purpose

This repository serves as a practical guide for learning and demonstrating:

Supervised fine-tuning (SFT) on small models

LoRA parameter-efficient finetuning (PEFT)

Reinforcement learning with human/AI preferences

GRPO-style reasoning RL for chain-of-thought tasks

Continued pretraining (CPT) for new languages or niche domains

All techniques are implemented using Unsloth, enabling fast training on consumer GPUs or Colab accelerators.

Each notebook is structured for academic submission, industry demonstration, or reuse in future projects.

#### 2. Contents Overview
/colab-full-finetune/
    notebook.ipynb
    dataset_notes.md
    results/
    video/

 /colab-lora-finetune/
 /colab-rl-preferences/
 /colab-grpo-reasoning/
 /colab-continued-pretraining/
 /extras/
    export_to_ollama_notes.md
    safety_prompts.md

#### 3. Notebook Summaries
##### Notebook A: Full Finetuning (SFT)

Trains a small or quantized model end-to-end

Example base models: smolLM2 135M, Gemma 3 1B Unsloth 4-bit

Demonstrates tokenization, chat templates, training loop, and inference

Includes evaluation samples and before/after comparisons

##### Notebook B: LoRA Finetuning

Converts the SFT task to a parameter-efficient approach

Shows dramatic improvement in:

memory usage

training speed

deployability

Includes a direct comparison with Notebook A

##### Notebook C: RL with Preferences (RLAIF / DPO-like)

Uses a dataset containing:

input

preferred response

rejected response

Demonstrates:

reward signals

stability tricks

preference accuracy tracking

Includes qualitative and quantitative comparisons

##### Notebook D: GRPO Reasoning RL

Uses problem-only datasets

Model generates reasoning traces (“chain of thought”)

GRPO optimizes correctness and rationale clarity

Includes:

acceptance criteria

reward shapes

reasoning behavior changes

Notebook E — Continued Pretraining (CPT)

Extends the model’s vocabulary and knowledge on:

new languages

niche technical domains

Covers:

corpus cleaning

tokenization notes

perplexity improvements

behavior changes before/after CPT

#### 4. Models Used (Open Weights)

You may use the following models depending on notebook type:

smolLM2 (135M) — ideal for full SFT

Gemma 3 1B Unsloth 4bit — fast + capable

Llama 3, Llama 3.1 (8B) — LoRA & RL

Mistral 7B / NeMo 12B

Gemma 2 (2B/9B)

Phi-3 / Phi-3.5

Qwen2 7B

TinyLlama

Smaller models are preferred for full fine-tuning; larger ones suit LoRA or RL.

##### 5. Dataset Requirements

Each notebook includes a dataset_notes.md file describing:

Source and license

Schema and formatting

Preprocessing steps

Chat templates (if applicable)

Metadata for preference datasets (preferred vs. rejected outputs)

Reward definitions for GRPO/RL

Corpus preparation notes for CPT

##### 6. Deliverables Checklist

Every folder in this repo contains:

✓ A working Colab notebook

✓ Dataset notes or corpus preparation explanation

✓ At least 3 before/after qualitative examples

✓ Training logs, charts, or screenshots in results/

✓ A video walkthrough detailing:

#### Problem framing

Dataset explanation

Code tour

Training or log replay

Inference demonstrations

Summary of findings

#### 7. Evaluation Guidelines
Quality Metrics

Qualitative improvements

Reward curves (RL)

Preference accuracy

Reasoning correctness (GRPO)

Optional: BLEU/ROUGE/F1 depending on task

Efficiency Notes

Accelerator used (T4/L4/A100)

Runtime and memory metrics

LoRA rank and hyperparameters

Comparisons

Full SFT vs. LoRA

SFT vs. RL vs. GRPO

CPT before/after behavior