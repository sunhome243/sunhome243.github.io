---
date: 2025-12-28 12:00:00
layout: post
title: "Why I Chose Lambda Over Amazon Bedrock AgentCore for Our AI Backend"
subtitle: "A practical decision-making guide for serverless AI workloads"
description: "After careful analysis of AWS Lambda vs Amazon Bedrock AgentCore for our AI tutoring platform, we chose Lambda. Here's the detailed cost, security, and performance breakdown."
image: /assets/img/posts/agentcore-vs-lambda-thumbnail.png
optimized_image:
category: "blog"
tags:
  - aws
  - lambda
  - bedrock
  - serverless
  - ai
author: Sunho Kim
paginate: false
---

## Introduction

AWS launched **Amazon Bedrock AgentCore**, an agentic platform for building, deploying, and operating AI agents securely at scale. With promises of up to 8-hour runtimes, built-in session memory, and **I/O-aware billing** that doesn't charge during wait times, it seemed like the obvious choice for our AI backend.

But after careful analysis, we went with AWS Lambda instead.

Here's why—but first, let me explain what we're building.

---

## What We're Building: FlowSpace

**FlowSpace** is a desktop application that helps students who struggle with task management stay on top of their assignments.

### The Problem

Students often face overwhelming assignments—a 20-page research paper, a complex coding project, or a multi-part lab report. The common response? Procrastination, anxiety, and last-minute cramming.

Traditional task managers don't help. They show you _what_ to do, but not _how_ to approach it.

### Our Solution: AI-Powered Task Breakdown with Academic Integrity

FlowSpace uses AI to:

1. **Break down complex assignments** into manageable subtasks

   - "Write a 20-page research paper" becomes a sequence of concrete steps: find sources, create outline, draft introduction, etc.

2. **Provide personalized tutoring** that maintains academic integrity

   - The AI explains concepts and guides thinking—it doesn't write answers for students
   - Socratic method: asking questions that lead students to discover solutions themselves

3. **Adapt to individual learning patterns**
   - Learns how long tasks actually take for each student
   - Suggests realistic schedules based on personal history

### Why This Matters for Architecture

This context is critical for understanding our infrastructure decisions:

- **Privacy-first**: Student learning data is stored on-device by default
- **Low-latency expectations**: Students won't wait 10+ seconds for AI responses
- **Cost sensitivity**: We're a startup—every dollar matters
- **Moderate AI workflow**: Typical interactions involve 2-6 LLM calls—not trivial, but not complex enough to require multi-agent orchestration

With this context, let's compare our options.

---

## Understanding the Options: AgentCore vs Lambda

### What They Share

Both are serverless compute options from AWS. No server management, pay-per-use, and native AWS ecosystem integration.

### Key Differences

| Aspect            | Lambda                            | AgentCore Runtime                                 |
| ----------------- | --------------------------------- | ------------------------------------------------- |
| Max Runtime       | 15 minutes                        | **8 hours**                                       |
| I/O Wait Billing  | Charged for entire duration       | **Free** (no CPU charges during I/O)              |
| Session Memory    | Manual implementation             | Built-in (short-term + long-term)                 |
| Billing Model     | Per GB-second of allocated memory | Per-second based on actual CPU/memory consumption |
| Session Isolation | Container-based                   | **MicroVM with complete isolation**               |

AgentCore's headline feature: **you don't pay while waiting for external I/O**. According to AWS, agentic workloads typically spend **30-70% of time in I/O wait**—waiting for LLM responses, tool calls, or database queries. With AgentCore, that idle time is free.

---

## Our AI Workflow in Detail

FlowSpace tutoring is a **multi-turn conversation**. A typical session looks like this:

```text
Student: "I don't understand recursion"
    ↓ Lambda invocation #1
AI: "Let's start with a simple example. What happens when you call factorial(3)?"
    ↓
Student: "It calls factorial(2)?"
    ↓ Lambda invocation #2
AI: "Exactly! And what does factorial(2) call?"
    ↓
... (conversation continues)
```

Each Lambda invocation:

1. Receives the full conversation history from the client
2. Makes 1-2 LLM calls to generate the next response
3. Returns the response to the client

**Measured performance (per invocation):**

| Metric                        | Value       |
| ----------------------------- | ----------- |
| Average latency               | ~28 seconds |
| P99 latency                   | ~45 seconds |
| Max observed                  | ~62 seconds |
| Typical LLM calls per request | 2-6 calls   |

**The key insight**: Each Lambda invocation is independent and short-lived. We don't maintain server-side state between turns—the client sends conversation history with each request. This "stateless" design means:

- No need for AgentCore's 8-hour runtime
- No need for built-in session memory (we manage it client-side)
- Each invocation stays well under Lambda's 15-minute limit

---

## The Decision Framework: Price, Security, Performance

### 1. Pricing: The Math Didn't Favor AgentCore (For Us)

AgentCore's I/O billing advantage shines when you have significant wait time. AWS cites 30-70% I/O wait as typical for agentic workloads.

**Our reality:**

- LLM inference: ~22-25 seconds (actual compute on LLM provider side)
- I/O wait on our side: ~2-3 seconds
- Database calls: ~1-2 seconds

**Total: ~25-30 seconds, with only ~10-12% spent in I/O wait**

We're on the **low end of I/O-heavy workloads**. Our agent makes 1-2 LLM calls per request, not 10-20 tool invocations.

```
Lambda:
- 1M free requests/month
- 400,000 GB-seconds free/month
- After free tier: $0.0000166667 per GB-second

AgentCore (after Sept 17, 2025):
- Consumption-based CPU/memory pricing
- Memory service: $0.25 per 1,000 events
- Gateway: $0.005 per 1,000 tool invocations
```

For our projected volume (early-stage startup), Lambda's generous free tier meant **months of zero compute costs**. The 2-3 seconds of I/O savings per request couldn't offset this.

### 2. Security: Both Strong, Different Models

**AgentCore** offers impressive isolation—each session runs in a dedicated **microVM** with isolated CPU, memory, and filesystem. After session completion, the entire microVM is terminated and memory is sanitized.

**Lambda** has battle-tested security with nearly a decade of production use. VPC integration, IAM roles, and encryption are well-documented.

For our education platform handling student data, both options met our requirements. Lambda's maturity gave us slightly more confidence.

### 3. Performance: 8-Hour Limit vs 15-Minute Limit

Lambda's 15-minute limit is often cited as a dealbreaker for AI agents. AgentCore extends this to **8 hours**—the longest in the industry for serverless.

Our telemetry:

- **Average response: 28 seconds**
- **P99 response: 45 seconds**
- **Maximum observed: 62 seconds**

We weren't even close to 15 minutes. The 8-hour runway was solving a problem we didn't have.

---

## Session Memory: Build vs Buy

AgentCore Memory provides:

- **Short-term memory**: Turn-by-turn interactions within a session
- **Long-term memory**: Extracts user preferences, facts, and session summaries across sessions
- **Built-in strategies**: Summary, user preferences, semantic extraction

Sounds powerful. But our requirements:

1. **No cross-device sync needed (yet)**: Our MVP is single-device, local-first
2. **Privacy-first architecture**: Session context stored on-device
3. **Cost consideration**: $0.25 per 1,000 memory events adds up

We pass session context directly in each request:

```typescript
const response = await invokeAI({
  studentContext: localSessionStore.get(),
  currentAssignment: assignment,
  learningHistory: recentHistory,
});
```

Simple. Controllable. Zero additional cost. No cloud database calls.

---

## When AgentCore Makes Sense

AgentCore is the right choice when you have:

- **Multi-step agents** with 10+ tool calls (30-70% I/O wait)
- **Long-running workflows** exceeding 15 minutes
- **Multi-agent collaboration** requiring extended sessions
- **Cross-device memory** requirements
- **Need for MicroVM-level isolation** between sessions
- **Framework flexibility** (LangGraph, CrewAI, Strands)

If we scale to complex multi-agent orchestration or real-time collaborative tutoring, we'll revisit this decision.

---

## Conclusion: Right Tool, Right Problem

Amazon Bedrock AgentCore is impressive and useful technology. The I/O-aware billing model is genuinely very helpful for agentic workloads.

But our decision came down to three questions:

1. **Does our I/O pattern (2-4 seconds) justify AgentCore's billing model?** No.
2. **Do we need runtime beyond 15 minutes?** No—we average 30 seconds.
3. **Does built-in session memory add value over our local-first approach?** No—not yet.

Lambda with its free tier, mature ecosystem, and proven reliability was the pragmatic choice for our workload pattern.

**The best architecture isn't the most sophisticated—it's the one that solves your actual problem at the right cost.**

---

## Key Takeaways

- AgentCore shines for **I/O-heavy agents** (30-70% wait time)—ours was only ~10-12%
- **Free tier economics** matter enormously for early-stage products
- **8-hour runtime** is powerful, but only if you need it (we averaged 28 seconds)
- **Built-in memory** is valuable for cross-device sync—not for local-first storage architectures
- Evaluate against _your_ workload, not theoretical maximums

---

## Sources

- [Amazon Bedrock AgentCore Overview](https://aws.amazon.com/bedrock/agentcore/)
- [AgentCore Pricing](https://aws.amazon.com/bedrock/agentcore/pricing/)
- [AWS Lambda Pricing](https://aws.amazon.com/lambda/pricing/)
- [AgentCore Memory Documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory.html)
- [AgentCore Runtime Documentation](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/agents-tools-runtime.html)
