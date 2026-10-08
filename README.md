# Foundation Models Final Project - Agentic RAG

**Final Project (M.Sc. Data Science, HIT). An end-to-end question-answering system over the EU AI Act: a retrieval pipeline, a grounded generator, and a tool-using agent on top, all running on local open models. The project asks a practical question: when does adding an agent with tools actually beat plain RAG, and what does it cost?**

## Headline Results
- **Retrieval is what matters most:** grounding the model in the right documents tripled the success rate (0.25 → 0.76) and cut hallucination by more than two thirds.
- **More complexity, no extra gain:** the full agent matched RAG (0.73) but used about 3.5× the tokens. It helped only when the answer needed data outside the text.
- **How the agent is built changes how it fails:** native function calling was faster and more reliable than a hand-written ReAct loop, and the two failed in different ways.
- **The evaluator was evaluated too:** the LLM judge was tested for consistency, agreement with human labels and verbosity bias before its scores were trusted.

## Key Features
- **Retrieval Pipeline:** keyword and semantic search combined, then reranked.
- **Grounded Generation:** answers checked claim by claim against the retrieved text.
- **Two Agent Designs:** function calling vs. ReAct, using the same tools on the same tasks.
- **Failure Analysis:** every failed agent run classified by where it broke.
- **LLM-as-a-Judge:** an independent model from a different family, validated against manual labels.
- **Controlled Comparison:** LLM only vs. RAG vs. full agent on identical items, repeated over three seeds.
- **Reproducible:** pinned models and data, fixed seeds, and every model call logged.

## Repository Structure
- `Foundation_Models_Project.ipynb`: The full project notebook - corpus and retrieval (Part A), grounded generation (B), the two agents (C), failure taxonomy (D), LLM-as-a-Judge (E), the ablation (F), the bonus retrieval comparison and the final agent profile, with code, outputs and analysis.
- `Project_AgenticRAG.pdf`: Original course project specification.
- `outputs/`: Everything produced by the saved run.
  - `project_results.json`: All metrics, configurations and pinned model revisions.
  - `calls.jsonl`: Log of all 1,148 model calls (request, output, timing), replayed on rerun.
  - `corpus_aiact.jsonl`: The 126-unit EU AI Act corpus as used in the project.
  - `run_date.txt`: The fixed date returned by the agent's `today` tool, for reproducibility.
  - `figures/`: All 37 plots from the notebook, prefixed by part (`a_` to `f_`, `bonus_`, `synthesis_`).
- `requirements.txt`: Library versions.

## Dataset
Built from [jeroenherczeg/eu-ai-act](https://huggingface.co/datasets/jeroenherczeg/eu-ai-act) on Hugging Face — the English text of the EU AI Act (Regulation 2024/1689), split into 126 articles and annexes.
