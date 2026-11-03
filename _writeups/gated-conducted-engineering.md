---
title: "Gated Conducted Engineering"
order: 1
group: marginborn
tags: ["Delivery", "AI"]
problem: "How far can coding agents go without a human?"
decision: "Agents iterate freely on surface work. Encryption, payments and data migrations need a human gate, and agents have no access to the production database or payment credentials."
rejected: "Full agent autonomy (the Compound Engineering framework). A human in the loop is what keeps the agents aligned."
sources: [WL-006, WL-008, WL-020, WL-023, E10-004]
---

## Context

I build Marginborn as a solo founder, with coding agents. The question is not whether to use agents, but where their authority stops.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| Full agent autonomy (the Compound Engineering framework) | The fastest path from idea to merged code | No human gate to keep the agents aligned | Rejected |
| Gated Conducted Engineering | A human decides every change to the spine | Slower on the changes that matter most | Chosen |

## Decision

Gated Conducted Engineering (GCE) has three layers: Engineering, Conducted and Gated. Agents propose, and I dispose.

- Agents iterate freely on surface work.
- A spine change (encryption, payments, data migrations) needs a human gate.
- Features are gated on evidence before they ship.
- Agents have no access to the production database or to payment credentials.

The security review is split by threat model: one reviewer for the server boundary (Edge Functions, the AI proxy that holds the API key, JWT validation, CORS), a separate reviewer for the admin surface, and separate crypto and migration reviewers. A security review without the repository's trust-boundary profile returns HOLD.

## How I built it

GCE lives in its own repository: the gate rules and the reviewer personas that Marginborn changes pass through. The data layer in [Offline sync](/offline-sync/) was written this way.

## Lesson

The gate is where the trust boundary lives. A reviewer without the trust-boundary context should block, not guess.
