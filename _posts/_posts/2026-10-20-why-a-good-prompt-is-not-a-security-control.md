---
layout: post
title: "Why a Good Prompt Is Not a Security Control for AI Agents"
date: 2026-10-20 00:00:00
description: Clear, structured prompts make AI agents more useful, but they can't enforce rules. Here is what belongs in the prompt and what has to be enforced in code.
tags: ai-agents prompts security red-teaming
categories: ai-agents
---

*By the BotGauge team. Published October 20, 2026.*

> **Short answer:** A well-written prompt makes an agent clearer and more consistent, but it is a request, not a guarantee. Put guidance in the prompt, and enforce anything that must hold in code, with permissions and approvals the model can't talk its way around.

Picture a refunds agent with one rule in its prompt: never refund more than $100 without a manager's approval. A customer ticket arrives with an extra line claiming the refund was pre-approved. If the only thing standing between that line and the refund is a sentence in the prompt, the model decides whether to follow it. That is the gap this post is about.

#### What a good prompt does well

A clear prompt sets the role, the task, the format, and the boundaries the agent should respect. Structured prompt frameworks exist for exactly this. A prompt generator such as [GPTPromptMaker](https://www.gptpromptmaker.com/) describes frameworks like RISEN, which covers role, instructions, steps, end goal, and narrowing, including what the model should not do. Writing prompts that way removes ambiguity and makes behavior more predictable, and that is worth doing for every agent.

#### Where the prompt stops

A prompt is still an instruction the model may not follow, especially when other text in its context pushes the other way. Our guide to [preventing prompt injection in AI agents](https://www.botgauge.com/blog/how-to-prevent-prompt-injection-in-ai-agents) puts it plainly: putting constraints in the system prompt is one mitigation, but it's a request, not a control that enforces anything, and no known method stops every injection. The practical goal is containment: assume an injection will sometimes work and make sure it can't do much.

The same guide describes one benchmark, AgentDojo, where restricting which tools an agent could use cut successful targeted attacks on GPT-4o from roughly 58 percent to under 8 percent in the paper's tests. Those tests used 2024 model versions, so treat the numbers as evidence that limiting tools helps, not as a guarantee for your agent.

#### What goes in the prompt and what goes in code

This split is our own rule of thumb, not an industry standard:

| Put it in the prompt | Enforce it in code |
|---|---|
| Role, tone, and output format | Spending and refund limits |
| How to explain a refusal to a customer | Which records each user can access |
| When to ask a clarifying question | Human approval for high-risk actions |
| Guidance on scope and escalation wording | Allowlists of tools and recipients |

If breaking a rule would cost money or expose data, it belongs in the right-hand column. Write it in the prompt as well, so the agent behaves sensibly, but don't rely on the prompt to hold the line.

#### How to find out which rules are really enforced

You find out by attacking the agent the way a real attacker would and checking what it did, not what it said. We explain the method in [what AI agent red teaming is](https://www.botgauge.com/blog/what-is-ai-agent-red-teaming) and the timing in [when to red team an AI agent](https://www.botgauge.com/blog/when-should-you-red-team-an-ai-agent). Each failure you find should become a regression check, which we cover in [automated red teaming for AI agents](https://www.botgauge.com/blog/automated-red-teaming-for-ai-agents).

For more on checking your own agent, see our [pre-launch checklist](/blog/2026/ai-agent-pre-launch-checklist/) and [why AI agents pass their tests and still fail in production](/blog/2026/why-ai-agents-pass-tests-fail-in-production/). To see what tracing should capture, read [what an AI agent trace should show](/blog/2026/what-an-ai-agent-trace-should-show/).

#### FAQ

**Does a better prompt reduce prompt injection?**
It can help the agent behave more consistently, but it can't stop an injection on its own. Limit permissions, require approval for high-risk actions, and enforce rules in code.

**Should I still invest in prompt quality?**
Yes. Clear prompts improve reliability and reduce ambiguity. Just don't treat them as a security boundary.

**What is the first rule I should move into code?**
The one with the highest cost if it fails, such as spending limits or access to other customers' data.

#### Related reading

- [BotGauge's AI agent red teaming platform](https://www.botgauge.com/)
- [What observability, evals, and red teaming each catch](/blog/2026/ai-agent-observability-evals-red-teaming/)
- [More guides on the BotGauge blog](https://www.botgauge.com/blog)

We're building [BotGauge](https://www.botgauge.com/) to red team AI agents. If you want early access, [join the BotGauge waitlist](https://join.botgauge.com/red-teaming).

**Sources:** BotGauge, [How to prevent prompt injection in AI agents](https://www.botgauge.com/blog/how-to-prevent-prompt-injection-in-ai-agents); [AgentDojo](https://arxiv.org/abs/2406.13352), ETH Zurich and Invariant Labs; [GPTPromptMaker](https://www.gptpromptmaker.com/).
