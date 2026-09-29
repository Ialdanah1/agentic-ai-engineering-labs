# FAISS RAG & RAGAS Evaluation

## Overview

This lab compares three FAISS indexes in a biomedical RAG workflow, evaluates retrieval against labeled passages, and scores generated answers with RAGAS. It also includes a separate cross-encoder reranking experiment.

The dataset is `enelpol/rag-mini-bioasq`, loaded through Hugging Face Datasets using the `text-corpus` and `question-answer-passages` configurations. The saved run contains 40,181 usable PubMed-derived passages and 707 test questions with reference answers and relevant passage IDs.

## Key Concepts

- Normalized dense embeddings and L2 search, with cosine-equivalent ranking for the exact index.
- Exact search, inverted-file search, and product quantization.
- Separating approximation error from retrieval of genuinely relevant passages.
- Reranking retrieved candidates with a cross-encoder.
- Comparing context quality, answer grounding, relevance, and correctness.

## Technologies

| Component | Implementation |
| --- | --- |
| Vector search | FAISS; setup installs `faiss-gpu-cu12`, with a cell that transfers indexes to GPU |
| Retrieval embeddings | Sentence Transformers `BAAI/bge-small-en-v1.5`, 384 dimensions, unit normalization, and a query instruction prefix |
| Reranker | `cross-encoder/ms-marco-MiniLM-L-6-v2` |
| Answer generation and evaluation LLM | OpenAI `gpt-4o-mini` for both roles |
| Evaluation embeddings | OpenAI `text-embedding-3-small` |
| Evaluation and analysis | RAGAS `0.4.3`, Hugging Face Datasets, NumPy, pandas, PyTorch, and Matplotlib |

## Indexing / Retrieval Approaches

| Approach | Configuration in the notebook |
| --- | --- |
| `IndexFlatL2` | Exhaustive nearest-neighbor baseline over normalized passage vectors |
| `IndexIVFFlat` | Trained inverted-file index; `nlist = min(512, max(16, len(passages) // 39))` and `nprobe = 10` |
| `IndexIVFPQ` | Same partition count and probe setting, with 8-bit product-quantization codes; the 384-dimensional embeddings use 48 subvectors |
| Cross-encoder reranking | Retrieves 100 candidates from `IndexFlatL2`, scores question/passage pairs, and keeps the top 10 |

The default retrieval depth is `TOP_K = 10`. A separate experiment varies exact-search depth across 5, 10, 20, and 50. The answer-generation and RAGAS comparison use direct FAISS retrieval; the reranker is evaluated separately.

## Evaluation

**Retrieval evaluation** uses the first 200 eligible questions and reports:

- Search time in microseconds per query, averaged over five batched searches with query encoding outside the timed region.
- `recall@k vs exact`: overlap with the neighbors returned by `IndexFlatL2`.
- `gold recall@k`: the fraction of labeled relevant passages retrieved.
- Gold recall before and after cross-encoder reranking.

**RAG evaluation** samples up to 20 questions with random seed 42 and generates answers from each index's retrieved contexts. The generation prompt requests context-grounded answers and an `I don't know.` response when evidence is insufficient.

| RAGAS metric | Evaluation focus |
| --- | --- |
| `ContextRecall` | Coverage of the reference answer by retrieved context |
| `ContextPrecisionWithReference` | Ranking of relevant retrieved passages |
| `Faithfulness` | Support for answer claims in the retrieved context |
| `AnswerRelevancy` | How well the answer addresses the question |
| `AnswerCorrectness` | Agreement with the reference answer |

The notebook displays mean scores, gold recall, a comparison chart, and per-question detail for `IndexIVFPQ`. Some saved `AnswerCorrectness` calls fail because of an output-token limit; the scoring helper records failures as `NaN`, which the mean excludes. The small evaluation sample and missing scores limit conclusions about index performance.

## What I Practiced

- Preparing passages and mapping gold passage IDs to FAISS rows.
- Building, training, and comparing exact and approximate indexes.
- Examining search latency, retrieval recall, and vector-compression tradeoffs.
- Testing reranking independently from the main RAG comparison.
- Interpreting reference-based and LLM-judged metrics alongside scoring failures.

## Notebook

[faiss_rag_ragas_evaluation.ipynb](./faiss_rag_ragas_evaluation.ipynb)

The notebook includes GPU-oriented setup, a session-restart instruction after FAISS installation, an OpenAI API-key prompt, and a compatibility workaround before importing RAGAS.

## Context

Completed as part of the SDA Agentic AI Engineering Bootcamp 2026.

Original lab materials developed by WeCloudData.
