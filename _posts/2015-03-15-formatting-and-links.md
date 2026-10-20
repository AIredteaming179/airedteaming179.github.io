---
layout: post
title: "An AI Agent Pre-Launch Checklist: Observability, Evals, and Red Teaming"
date: 2026-10-20 00:00:00
description: A checklist for shipping an AI agent covering observability, evals, and red teaming, plus the changes that should trigger a retest.
tags: ai-agents evals observability red-teaming checklist
categories: ai-agents
---

*By the BotGauge team. Published October 20, 2026.*

> **Short answer:** Before an agent reaches real users, make sure you can see what it does, check that it keeps doing it, and have tried to break it. Then retest whenever its tools, permissions, model, or data change.

This checklist is our own synthesis of the sources listed at the end, not an industry standard. Use it as a starting point and adjust it to what your agent can touch.

#### Observability: can you see what the agent did?

- [ ] Every run records the prompts and context going in, each tool call with its arguments, and what each tool returned.
- [ ] You can check the outcome in the system of record (the database, ledger, or ticket system), not just the agent's reply. Anthropic's guide to [evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) shows why: a booking agent can say a flight is booked while no reservation exists.
- [ ] Each trace notes the model version, system prompt, tool list, and memory state it ran against.
- [ ] Someone looks at real traces regularly, not only when a customer complains. We explain what to record in [what an AI agent trace should show](/blog/2026/what-an-ai-agent-trace-should-show/).

#### Evals: does it still work?

- [ ] You have 20 to 50 simple tasks drawn from real or realistic failures, which Anthropic describes as a good starting point.
- [ ] Each task runs several times, because agents vary from run to run. Track a consistency measure such as pass^k for anything customers rely on every time.
- [ ] You keep two suites: capability evals that stretch the agent, and regression evals that should pass almost every time.
- [ ] Someone has read transcripts from many runs, to confirm the graders are fair and the tasks are unambiguous.
- [ ] Test environments are isolated from each other and close to production. See [why AI agents pass their tests and still fail in production](/blog/2026/why-ai-agents-pass-tests-fail-in-production/).

<hr>

#### Red teaming: have you tried to break it?

- [ ] You have a written list of every tool the agent can call, with its permissions, and every channel of outside content it reads (email, tickets, files, web pages, memory).
- [ ] You have written down the outcomes you cannot accept, as actions, such as "issues a refund without verification."
- [ ] You have tested prompt injection through content the agent reads, not only through the chat box. Our guide to [preventing prompt injection in AI agents](https://www.botgauge.com/blog/how-to-prevent-prompt-injection-in-ai-agents) covers the containment steps.
- [ ] Results are judged by what the agent did (tool calls and resulting state), not what it said. We explain the difference in [LLM red teaming vs agent red teaming](https://www.botgauge.com/blog/llm-red-teaming-vs-agent-red-teaming).
- [ ] Every confirmed failure is saved as a regression eval, so it runs again on every release. See [automated red teaming for AI agents](https://www.botgauge.com/blog/automated-red-teaming-for-ai-agents).

#### Retest triggers: what makes your results stale?

Re-run the relevant checks whenever one of these changes:

- a new tool, or wider permissions on an existing one
- a model, provider, or prompt change
- a new retrieval source or a change to memory
- a policy or approval rule change
- a jump in users or autonomy
- an incident or a near miss

Our guide on [when to red team an AI agent](https://www.botgauge.com/blog/when-should-you-red-team-an-ai-agent) explains each trigger and how much testing it deserves. For a broader view of how the three practices fit together, read [what observability, evals, and red teaming each catch](/blog/2026/ai-agent-observability-evals-red-teaming/).

#### Related reading

- [BotGauge's AI agent red teaming platform](https://www.botgauge.com/)
- [Best AI red teaming tools: open source and commercial](https://www.botgauge.com/blog/best-ai-red-teaming-tools-open-source-and-commercial)
- [AI observability and evaluation tools compared](https://www.botgauge.com/blog/best-ai-observability-tools)

We're building [BotGauge](https://www.botgauge.com/) to red team AI agents. If you want early access, [join the BotGauge waitlist](https://join.botgauge.com/red-teaming).

**Sources:** Anthropic Engineering, [Demystifying evals for AI agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents); BotGauge guides linked above.
