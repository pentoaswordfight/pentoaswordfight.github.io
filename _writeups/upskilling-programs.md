---
title: "Tailored upskilling programs"
order: 6
group: also
tags: ["AI", "Delivery"]
problem: "Generic courses take months, teach one vendor's platform, and are too broad and too narrow in parts for one role."
decision: "A Claude Code skill builds a learning program from what the target role or domain needs, and a reviewer fails weak material before the learner sees it."
rejected: "Generic courses. No other option was weighed."
scale: "2 to 3 hours per program."
sources: [WL-027]
---

## Context

An upskilling tool that dynamically builds a learning program tailored to the learner's needs for a target role or domain. A generic course takes months and still does not know the role.

## What I weighed

| Option | For | Against | Verdict |
|---|---|---|---|
| Generic courses | Ready-made | They take months, teach one vendor's platform, and are too broad and too narrow in parts for one role | Rejected |
| A program built for the target role | Covers what the role needs, in 2 to 3 hours | A source sweep and a review for each program | Chosen |

## Decision

A Claude Code skill builds each program in steps:

1. **Map.** It maps what the role needs in three parts (domain, craft, organisation) and ranks the needs.
2. **Check.** It stops for the learner's check of the syllabus.
3. **Reuse.** It reuses modules from earlier programs.
4. **Sweep.** It sweeps public sources under fixed rules: each source is opened and read before it is listed, no paywalls, no sales pitches, and perishable facts come from the last 12 months.
5. **Case.** It adds one real case study.
6. **Quiz.** It writes quizzes for the start and the end of each session.
7. **Review.** A domain-practitioner reviewer fails craft misapplied to the domain, stale facts and generic case questions (a swap test) before the learner sees the program.

A Learn tab in a phone app shows the material offline, and a Worker behind Cloudflare Access commits progress to git.

## Lesson

A course earns its hours only when it is built from what one role needs.
