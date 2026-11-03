---
title: "Offline sync"
order: 4
group: marginborn
tags: ["Data", "Architecture"]
problem: "Marginborn works offline-first and syncs later. A stale offline edit can overwrite a newer one."
decision: "A database guard rejects a stale write. It compares the time of the edit on the client, never the time the server received it."
rejected: "Stamping the server's time at sync. That turns last-write-wins into last-sync-wins."
scale: "8 synced tables guarded."
sources: [WL-022, E10-003]
---

## Context

Marginborn is offline-first: an edit goes to a local store and an outbox, and syncs when the device is online again. Two devices can edit the same record before either one syncs, so the server needs a rule for which write wins.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| Stamp the server's time at sync | One clock; no device clock to trust | The last device to sync wins, not the last edit | Rejected; an earlier version that did this was retracted |
| Compare the client's edit time | The newest edit wins | Trusts the device clock | Chosen |

## Decision

A guard on the 8 synced tables runs before each update and rejects a write that is older than the stored one. A second trigger stamps each insert and update from a per-table sequence, so a sync cursor pages through the changes in order and never misses a row that arrives late.

## How I built it

I designed the PostgreSQL data layer and maintain it via a series of schema migrations: tables, indexes, row-level security policies and grants, triggers and functions. I wrote the SQL with Claude under the Gated Conducted Engineering gates: 77 of 80 migration commits are co-authored.

## Lesson

A sync guard compares the client's edit time and never stamps the server's time, or last-write-wins becomes last-sync-wins.
