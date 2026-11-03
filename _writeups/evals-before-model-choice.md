---
title: "Evals"
order: 2
group: marginborn
tags: ["AI"]
problem: "Is the cheaper model good enough for OCR?"
decision: "No. The evals showed unreliable OCR, so the pipeline runs on Claude Sonnet only. Every eval run has a pre-flight cost estimate and a hard spend cap."
rejected: "Haiku for OCR, to save cost. The evals showed that its output was not usable."
scale: "8 models in the cross-provider run; a weekly prompt-drift check."
sources: [WL-001, WL-013, WL-024, E10-002]
---

## Context

The pipeline does OCR and structured idea tagging against a concept taxonomy. A cheaper model cuts the cost of each call, but only if its output is usable.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| Haiku for OCR and tagging | Lower cost per call | The evals showed unreliable OCR | Rejected |
| Claude Sonnet only | Usable OCR and tagging | Higher cost per call | Chosen |

## Decision

The pipeline runs on Claude Sonnet only; Haiku runs nowhere. Opus is planned for judging in future features, not built.

## How I built it

The evals are a separate repository that "evaluates managed-AI quality across providers and models": an Anthropic baseline (Haiku, Sonnet, Opus), a cross-provider run of 8 models, and a vision matrix for transcription. Each run has a pre-flight cost estimate and a hard USD spend cap. The evals re-run against the committed baseline when a dependency or the canon changes, and a weekly CI job checks that the prompts in the evals still match production.

## Lesson

A cheaper model is only cheaper if the eval says its output is usable. A model choice holds only while the eval is re-run each time the model, the prompt or a dependency changes.
