# Foundation Models Final Project - Agentic RAG

**Final Project (M.Sc. Data Science, HIT). A retrieval-augmented generation pipeline and a tool-calling agent over the EU AI Act (Regulation 2024/1689), built on local open models with no API — hybrid retrieval, reranking, function calling and ReAct, judged by a model from a different family. Does an agent with tools beat plain RAG, and what does it cost?**

## Headline Results
- **Retrieval does the heavy lifting:** success 0.25 → 0.76 from LLM-only to RAG, hallucination 0.71 → 0.20 (84 items, 3 seeds).
- **The agent does not pay off:** 0.73 success at about 3.5× the prompt tokens. It wins only where the answer needs data outside the text (registry, dates).
- **Reranking beats chunking:** MRR 0.944 and nDCG@5 0.897; reranking is the main factor, chunking secondary.
- **Two agents, two failure profiles:** function calling 18 of 30 tasks vs. ReAct 13 of 30, 2.3× faster with no format errors — wrong arguments vs. wrong tool choice.
- **The judge, measured too:** kappa 0.66 across evidence orders, 0.40 against manual labels (n = 24), and 19 of 20 padded answers caught.

## Key Features
- **Fixed Corpus and Query Set:** 126 articles and annexes, 18 fixed queries and 10 fixed tasks, kept constant across every part.
- **Two-Stage Retrieval:** BM25 + dense embeddings fused with RRF, then cross-encoder reranking; structural vs. window chunking compared.
- **Grounded Generation:** RAG vs. closed-book, with a claim-level groundedness criterion.
- **Two Agent Designs:** five tools, used through native function calling and through a hand-written ReAct loop.
- **Failure Taxonomy:** every failed run labeled by hand — tool prediction, tool call, or response generation.
- **LLM-as-a-Judge:** rationale before score, evidence-order consistency, agreement with manual labels, and a verbosity-bias probe.
- **Controlled Ablation:** LLM only vs. RAG only vs. full agent, on the same items, three seeds.
- **Bonus Extension - Dense vs. BM25 vs. Hybrid:** retrieval quality traced to its effect on final groundedness.
- **Local Models Only:** Qwen2.5-7B-Instruct agent and Phi-4 judge (both NF4), e5-base-v2 embeddings, bge-reranker-v2-m3 — all revisions pinned.
- **Replayable Runs:** fixed seeds, and every model call logged and replayed on rerun.

## Repository Structure
- `outputs/` — everything the saved run produced.
  - `calls.jsonl` (1,148 calls) — every model call: request, output and timing, keyed by a SHA-256 hash.
  - `corpus_aiact.jsonl` (126 units) — the EU AI Act corpus as used in the project.
  - `project_results.json` — all metrics, configurations and model revisions.
  - `run_date.txt` — the fixed date used by the `today` tool.
  - `figures/` (37 plots):
    - Part A (12): `a_corpus_overview.png`, `a_dataset_units.png`, `a_chunk_lengths.png`, `a_long_documents.png`, `a_queries.png`, `a_metrics_by_condition.png`, `a_main_per_query.png`, `a_recall_heatmap.png`, `a_rerank_demo.png`, `a_manual_relevance.png`, `a_effect_sizes.png`, `a_failure_analysis.png`
    - Part B (5): `b_verdicts.png`, `b_verdict_map.png`, `b_unsupported_per_query.png`, `b_citations.png`, `b_cost.png`
    - Part C (5): `c_tasks.png`, `c_registry.png`, `c_success.png`, `c_tool_usage.png`, `c_cost_by_approach.png`
    - Part D (1): `d_failures.png`
    - Part E (5): `e_agent_verdicts.png`, `e_consistency.png`, `e_order_effect.png`, `e_manual_agreement.png`, `e_verbosity.png`
    - Part F (6): `f_ablation.png`, `f_agent_vs_rag.png`, `f_agent_search.png`, `f_correctness.png`, `f_seeds_passk.png`, `f_success_map.png`
    - Bonus and synthesis (3): `bonus_retrieval_downstream.png`, `synthesis_agent_profile.png`, `compute_budget.png`
- `Foundation_Models_Project.ipynb`: Notebook — Parts A-F, bonus and the agent profile, with the written report (Hebrew) (Colab).
- `Project_AgenticRAG.pdf`: Original course project specification.
- `requirements.txt`: Library versions from the saved run.

## Dataset
The corpus is built from [jeroenherczeg/eu-ai-act](https://huggingface.co/datasets/jeroenherczeg/eu-ai-act) on Hugging Face — the English text of the EU AI Act (Regulation 2024/1689), loaded at a pinned commit and split into 126 units (113 articles, 13 annexes).
