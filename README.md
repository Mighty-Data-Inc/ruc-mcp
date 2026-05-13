# Render Unto Caesar (`ruc-mcp`)

LLMs make a great interface for requesting tasks, but a poor engine for executing them. They are good at assessing messy, ambiguous information, but poor at carrying out long, exact procedures. When an operation requires both the fuzziness of LLMs and the methodical rigor of traditional code, **Render Unto Caesar** separates the work into the parts that need interpretation and the parts that need machinery, and makes them interoperate to give you the best of both worlds.

Modern LLM apps make it natural to ask for complex work in plain English. The problem is that plain-English requests often mix together things LLMs are good at with things they are famously bad at. A user might ask ChatGPT, Claude, or Cursor to review support tickets, compare contracts, or clean a messy spreadsheet. Those tasks require semantic judgment, but they also require loops, counts, state, validation, consistency, and auditability — the stuff traditional code is built for.

Most users, even engineers, don’t naturally separate those two layers when they ask for the task. They describe the outcome they want, not the architecture needed to produce it reliably. RUC does that separation for them. It identifies which parts require deterministic execution — iteration, arithmetic, validation, state, and auditability — and which parts require classification, summarization, or ambiguity resolution. It defines the interfaces between those parts, specifying how code-shaped work should pass data into LLM-shaped work, and how LLM-shaped results should flow back into procedural execution. That lets both kinds of work operate in concert: code provides structure, continuity, and rigor, while the LLM handles nuance where rigid rules would break down.

The result is a system that can do LLM-shaped work with the control, structure, and repeatability of procedural software.

## Project status

`ruc-mcp` is currently in the concept/prototype stage.

The intended direction is an MCP server that helps LLM agents decompose plain-English tasks into reliable procedural workflows, while preserving controlled LLM calls at the exact points where semantic judgment is needed.

## Core idea

Render Unto Caesar is built around a simple split:

- **Code handles** loops, state, arithmetic, validation, retries, files, progress tracking, and aggregation.
- **LLMs handle** classification, summarization, ambiguity resolution, fuzzy matching, and other semantic decisions.

The important part is not merely that both are available. The important part is that RUC defines the boundary between them, so procedural code and LLM judgment can safely interoperate inside one workflow.

## Early design direction

The first proof of concept will likely focus on AI-authored Python workflows with explicitly marked LLM decision points.

For example, a generated workflow may contain a structured TODO annotation such as:

```python
# TODO(implement_with_llm: classify_feedback_tone)
# input:
#   feedback_text: str
# output:
#   tone: Literal["angry", "frustrated", "neutral", "positive", "unknown"]
# instructions:
#   Classify the customer's tone based on the feedback text.
tone = classify_feedback_tone(feedback_text)
```

RUC can then transform that semantic hole into a controlled LLM call, while leaving the surrounding workflow logic in ordinary Python.

## Why this matters

As LLMs become the interface to more software, users increasingly ask them to perform tasks that are partly semantic and partly procedural. Without a system like RUC, the LLM is often asked to do everything itself: reason, loop, count, remember progress, validate outputs, and produce final results.

That is exactly where LLMs are weakest.

RUC exists to let the LLM serve as the interface and semantic engine, while giving the procedural work back to code.


## Status

This repository currently contains a bare-bones scaffold only. Core functionality is intentionally left as TODOs.

## Scaffold layout

- `src/ruc_mcp/server.py`: `RucMcpServer` skeleton and TODO method stubs.
- `tests/test_server.py`: scaffold tests that verify placeholders and explicit TODO behavior.

## Run tests

```bash
python -m unittest discover -s tests -p "test*.py"
```
