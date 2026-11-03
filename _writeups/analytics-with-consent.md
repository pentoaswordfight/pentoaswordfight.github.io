---
title: "Product Analytics"
order: 5
group: marginborn
tags: ["Product"]
problem: "Usage data for the native apps, without a tracking SDK, and only from users who opt in."
decision: "The apps post name-only events to an Edge Function that enforces an allowlist and forwards them to PostHog EU, with no person profiles. Consent is off by default."
rejected: "Analytics SDKs in the apps. They add vendors, and a solo founder needs one integrated platform to watch."
scale: "6 capture-button events allowlisted; consent off by default."
sources: [WL-015, WL-026]
---

## Context

Marginborn encrypts user content on the device. Usage data for the native apps had to fit that: collect only named events, and only with consent.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| Analytics SDKs in the apps | Quick to add | Vendor sprawl: one more system to watch for each vendor | Rejected |
| PostHog, reached through a first-party Edge Function | One integrated platform; the server decides what is accepted | Each new event needs an allowlist change | Chosen |

## Decision

- The apps post name-only events to an Edge Function.
- The function enforces an event allowlist on the server and forwards the events to PostHog EU, with no person profiles.
- Consent defaults to off, with a one-time opt-in prompt.
- The allowlist accepted its first 6 capture-button events after these rules shipped.

The next feature waits on this kind of data: zero-result and abandon rates, once they are instrumented, decide whether semantic search gets built.

## How I built it

An analytics-privacy reviewer persona reviews analytics changes, in the same gates as the rest of the code ([Gated Conducted Engineering](/gated-conducted-engineering/)).

## Lesson

Usage data is worth collecting only when the allowlist and the consent check ship before the first event. For a solo founder, every extra vendor is one more system to watch.
