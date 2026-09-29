# LangGraph Agent Observability

## Overview

This lab instruments a LangGraph agent investigating a fictional emerald theft. The agent uses mock alibi, evidence, and case-metric tools while an in-memory observability layer records its execution for inspection.

## Key Concepts

- **Structured traces:** `StepLog`, `ExecutionTrace`, and `ObservabilityLogger` associate events with a trace ID, timestamps, status, and final answer.
- **Tool-call logging:** Records tool-selection decisions, arguments, calls, and returned results across the agent/tool loop.
- **Response logging:** Stores returned model text under the `reasoning` event type; these entries capture visible response content rather than hidden model reasoning.
- **Latency:** Measures model calls, tool-node execution, and total run time. Each result in a tool batch receives the same tool-node duration, so these are not individual tool timings.
- **Tokens and estimated cost:** Accumulates `usage_metadata.total_tokens` and applies a single fixed rate for a rough cost estimate. Input and output pricing are not calculated separately.
- **Trace inspection:** Produces console reports, a summary across runs, a latency breakdown, and JSON serialization with step content shortened to 300 characters.

## Technologies

Python, LangGraph (`StateGraph`, `ToolNode`), LangChain, OpenAI `gpt-4o-mini`, and Python dataclasses, timing, and JSON utilities.

## What I Practiced

- Following agent state and conditional routing between model and tool nodes.
- Instrumenting tool use and inspecting linked execution events.
- Comparing a single-suspect alibi query, a multi-tool investigation, and a two-suspect comparison.
- Interpreting latency, token usage, and approximate costs with their measurement limitations.
- Converting a trace into a structured JSON representation for inspection.

## Notebook

[agent_execution_logging.ipynb](./agent_execution_logging.ipynb)

The notebook includes dependency installation and prompts for an OpenAI API key. Its three detective tools use mock data and illustrative calculations.

## Context

Completed as part of the SDA Agentic AI Engineering Bootcamp 2026.

Original lab materials developed by WeCloudData.
