# Modern AI Workflows with Unsloth.ai

A hands-on notebook series demonstrating modern LLM training techniques using [Unsloth.ai](https://github.com/unslothai/unsloth). Each notebook covers a different training paradigm with reproducible end-to-end workflows designed for Google Colab.

## Notebooks

| # | Notebook | Topic | Model | Dataset |
|---|----------|-------|-------|---------|
| 1 | `1_Full_Finetuning_with_a_small_model.ipynb` | Full supervised fine-tuning (SFT) | `unsloth/smollm2-135m` | `tatsu-lab/alpaca` (500 samples) |
| 2 | `2_LORA_parameter_efficient_Finetune.ipynb` | LoRA parameter-efficient fine-tuning | `unsloth/smollm2-135m` | `tatsu-lab/alpaca` (500 samples) |
| 3 | `3_ReinforcementLearning_Using_Dataset(DPO_RLHF).ipynb` | DPO / preference alignment | `HuggingFaceTB/SmolLM2-135M-Instruct` | `HuggingFaceH4/ultrafeedback_binarized` |
| 4 | `4_ReinforcementLearning_with_GRPO.ipynb` | GRPO reasoning RL | `HuggingFaceTB/SmolLM2-135M-Instruct` | `openai/gsm8k` (800 math problems) |
| 5 | `5_ContinuedPretraining_HindiLanguage.ipynb` | Continued pretraining for Hindi | `unsloth/Qwen2-0.5B-bnb-4bit` | `allenai/c4` (Hindi subset) |

## What Each Notebook Covers

### 1. Full Finetuning (SFT)

Trains a small model end-to-end. Covers tokenization, chat templates, training loop, inference, and before/after comparisons.

### 2. LoRA Finetuning

Converts the SFT task to parameter-efficient fine-tuning (rank=16, 4-bit). Demonstrates dramatic improvements in memory usage, training speed, and deployability compared to full SFT.

### 3. RL with Preferences (DPO)

Uses a dataset with preferred and rejected responses. Covers reward signals, stability techniques, and preference accuracy tracking.

### 4. GRPO Reasoning RL

Uses problem-only datasets where the model generates reasoning traces. GRPO optimizes correctness and rationale clarity with custom numeric reward functions.

### 5. Continued Pretraining (CPT)

Extends model vocabulary and knowledge for Hindi. Covers corpus cleaning, tokenization, perplexity improvements, and behavior changes.

## Workflow (Each Notebook)

```
Install deps (unsloth, trl, transformers, datasets, bitsandbytes)
    → Load model (FastLanguageModel.from_pretrained, 4-bit)
    → Attach LoRA adapters (notebooks 2–5)
    → Load/preprocess dataset
    → Train (SFTTrainer / DPOTrainer / GRPOTrainer)
    → Inference smoke tests
    → Export to Hugging Face Hub
```

## How to Run

1. Open any `.ipynb` in **Google Colab** with a **GPU runtime** (T4 recommended)
2. Set Colab secrets:
   - `HGFaceApi` — Hugging Face token
   - W&B token (notebooks 2–5)
3. Run cells sequentially

No local `requirements.txt` — dependencies are installed inline via `%pip` in each notebook.

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Framework | Unsloth (`FastLanguageModel`), TRL (`SFTTrainer`, `DPOTrainer`, `GRPOTrainer`) |
| Models | SmolLM2-135M, Qwen2-0.5B (4-bit quantized) |
| Training | LoRA (PEFT), 4-bit quantization, bitsandbytes |
| Tracking | Weights & Biases (notebooks 2–5) |
| Platform | Google Colab (T4 GPU) |
| Export | Hugging Face Hub |

## Models Used

| Model | Best For |
|-------|----------|
| `unsloth/smollm2-135m` | Full SFT, LoRA |
| `HuggingFaceTB/SmolLM2-135M-Instruct` | DPO, GRPO |
| `unsloth/Qwen2-0.5B-bnb-4bit` | Continued pretraining |
| Gemma 3 1B, Llama 3, Mistral 7B, Phi-3, Qwen2 7B | LoRA and RL (alternatives) |

## Evaluation Guidelines

- **Quality:** Qualitative before/after comparisons, reward curves (RL), preference accuracy, reasoning correctness (GRPO)
- **Efficiency:** Accelerator type (T4/L4/A100), runtime, memory metrics, LoRA rank and hyperparameters
- **Comparisons:** Full SFT vs LoRA, SFT vs RL vs GRPO, CPT before/after behavior

## References

- [Unsloth GitHub](https://github.com/unslothai/unsloth)
- [Unsloth Documentation](https://docs.unsloth.ai/)
- [TRL (Transformer Reinforcement Learning)](https://github.com/huggingface/trl)
