---
layout: post
title: "Async-first collaboration across time zones — what actually works"
date: 2025-02-20
category: Remote Work
excerpt: "Practical patterns from years working in globally distributed teams: communication norms, documentation habits, and the tools that reduce friction without adding ceremony."
read_time: 6
---

Remote work gets easier once you stop trying to replicate the office online and start designing for async by default. Here are the patterns that have genuinely made a difference across several distributed teams.

## Default to writing, not meetings

The instinct when you're blocked is to ping someone. Resist it. Write the question down first — clearly enough that the person can answer asynchronously, ideally with enough context that they can answer *completely* without a follow-up round trip.

This sounds obvious but it changes the texture of daily work. A well-written question gets a well-considered answer. A Slack ping at 9am your time interrupts someone mid-morning across the Atlantic.

The byproduct is a written record. In six months, when a new teammate asks why the service is designed the way it is, the answer is already in Confluence or Notion rather than trapped in someone's memory.

## Decision logs > decision meetings

For any non-trivial decision — architecture choices, API contracts, tech debt prioritisation — write a lightweight decision record before calling a meeting. Include the context, the options considered, and your recommendation.

Distribute it 24 hours ahead. By the time the meeting happens (if you need one at all), everyone has had time to form an opinion, and you're resolving disagreements rather than orienting people.

At Cisco and Dell, this pattern alone cut the average meeting from 60 minutes to 30.

## Be explicit about availability

Distributed teams fail when people treat async communication as synchronous but slower. Set clear expectations:

- Block focus time on your calendar and treat it as real
- Use Slack status to indicate when you're in heads-down mode
- Set explicit response-time expectations for different channels (Slack = same day, email = 48h, Jira comment = next sprint cycle)

When everyone knows the norms, nobody reads silence as disengagement.

## Over-document the "why"

Code explains *what*. Comments explain *how*. Neither explains *why* — and the why is what your future teammates (and future you) will desperately want when they're debugging something at 11pm.

Document the constraints that shaped a decision. "We used polling here instead of webhooks because the third-party API doesn't support them" is more valuable than any amount of inline code comments.

## The tools that actually matter

After trying most of them: **Notion or Confluence** for persistent docs, **Linear or Jira** for tickets, **Slack with strict channel discipline** for communication, and **Loom** for walkthroughs that would take four paragraphs to explain in text.

The tool matters far less than the discipline around it. A consistent Slack channel taxonomy beats a perfectly configured async tool that nobody uses the same way.
