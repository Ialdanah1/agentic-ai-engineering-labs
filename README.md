# Agentic AI Engineering Labs

A collection of selected hands-on labs I completed during the **SDA Agentic AI Engineering Bootcamp 2026**. This repository documents my implementation and learning experience with agent observability, retrieval-augmented generation (RAG) evaluation, and agent behavior testing.

The notebooks explore how to inspect agent execution, compare retrieval approaches, and evaluate tool use and response quality. Original lab materials were developed by **WeCloudData**.

## Labs

| Lab | Main focus | Key technologies | Folder |
| --- | --- | --- | --- |
| LangGraph Agent Observability | Structured execution traces, tool-call logging, latency, and estimated cost | LangGraph, LangChain, OpenAI | [01-langgraph-agent-observability](./01-langgraph-agent-observability/) |
| FAISS RAG & RAGAS Evaluation | Vector index comparison, cross-encoder reranking, and retrieval and answer evaluation | FAISS, Sentence Transformers, RAGAS, OpenAI | [02-faiss-ragas-rag-evaluation](./02-faiss-ragas-rag-evaluation/) |
| Agent Behavior Evaluation | Scenario-based review of tool selection, scope boundaries, and failure cases | LangChain ReAct, OpenAI, tiktoken | [03-agent-behavior-evaluation](./03-agent-behavior-evaluation/) |

## Topics Covered

- Agent execution tracing, tool inputs and outputs, and JSON trace serialization.
- Latency measurement, token usage, and approximate cost tracking.
- Exact and approximate vector search with FAISS, plus cross-encoder reranking.
- Retrieval recall and RAGAS evaluation against reference answers and passages.
- Agent test design, responsible response expectations, adversarial prompts, and manual failure analysis.

## About

These labs represent my hands-on practice during the SDA Agentic AI Engineering Bootcamp 2026, using original materials developed by WeCloudData. Each lab README explains the notebook's implementation, evaluation approach, and learning focus. The examples and recorded results are educational lab work.
