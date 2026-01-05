---
date: 2026-01-03 12:00:00
layout: post
title: "Why I Ditched Apple's Foundation Model"
subtitle: "A practical look at the limits of on-device AI for complex workflows"
description: "Why privacy and zero cost weren't enough to save my document analysis feature from the realities of a 3B parameter model."
image: /assets/img/posts/apple-intelligence-analysis.png
category: "blog"
tags:
  - apple-intelligence
  - on-device-ai
  - ai
author: Sunho Kim
paginate: false
---

# Why I Ditched Apple's Foundation Model

When Apple finally unveiled its intelligence strategy, the promise was intriguing. Free, light-weight, strong, and free on-device model? Sounds very promising. As Apple describes it:

> The Foundation Models framework allows developers to create new intelligence features that protect users' privacy and are available offline, all while using AI inference that is free of cost.

On paper, this is the holy grail: Privacy-first, zero-inference-cost, and offline-capable.

So I jumped in immediately. I mean, who doesn't like free stuff? I attempted to build a document analysis feature using Apple's ecosystem, but after weeks of development, I decided to pull the plug.

In this post, I'll walk through the feature I tried to build, the specific constraints I encountered, and why I ultimately decided that despite its engineering strengths, this model was the wrong tool for the job.

## The Feature: Document Analysis Agent

Before diving into the technical constraints, let's clearly define the workload. I wasn't trying to build a general-purpose chatbot. The goal was simple: ingest a standard PDF, analyze the text, and decompose it into actionable details.

### The Complexity

While the inputs were standard, the processing requirements were strict.

- **Input Volume**: Standard business documents (averaging 3–4 pages of dense text).
- **Contextual Intelligence**: This wasn't a zero-shot task. I built a custom RAG (Retrieval-Augmented Generation) pipeline to query a local database, aggregate user history, and pass it with the prompt.
- **Output Structure**: The result needed to be a structured list of subtasks, dynamically personalized to the user's past behavior.

### The Requirements

- **Low Hallucination**: Precision was key; invented tasks are a dealbreaker.
- **Mid-level Reasoning**: It needed to understand context, not just keyword match, but didn't require deep reasoning.
- **Solely Text Generation**: No multimodal inputs required.

## Phase 1: The Allure - Why I Chose Apple First

I didn't start with Apple just because it was new. I started because the value proposition for my specific use case was incredibly strong.

### 1. Privacy is King

The hybrid model (On-Device + Private Cloud Compute) is the best privacy narrative in the industry. For an app analyzing personal documents, being able to promise that data never leaves the ecosystem is a massive competitive advantage.

### 2. Zero Unit Costs

Free inference is a game-changer for unit economics. Not paying OpenAI or Anthropic per token means you can scale usage without scaling your monthly bill.

### 3. Surprising Performance

For a strictly text-generation model running on a phone, the speed is decent. It punches above its weight class compared to other 3B open-source models, likely due to heavy optimization for Apple Silicon's Neural Engine.

According to Apple's technical reports, it holds its own against significantly larger models in human preference evaluations:

| Benchmark Category | On-Device Model (~3B) Performance                   | Comparisons                                            |
| ------------------ | --------------------------------------------------- | ------------------------------------------------------ |
| Human Preference   | Outperforms Phi-3-mini (3.8B), Mistral-7B, Gemma-7B | Preferred in real-world prompts (coding, math, safety) |
| Safety             | >99% Jailbreak Resistance                           | Lower violation rates than open-source peers           |
| MMLU (Knowledge)   | ~44% (Base)                                         | Trails Llama 3.2-3B (~50-60%) due to efficiency focus  |

While it lags in raw academic benchmarks like MMLU (knowledge density), its instruction-following capabilities for specific tasks are surprisingly robust for its size.

## Phase 2: The Reality Check - Where It Broke Down

However, once I moved past simple summarization and into application logic, the cracks started to show.

### The Context Window Bottleneck

The model ships with a strict 4,096 token limit.

For simple chats, this is passable. For document analysis, it is suffocating. My target documents were 3–4 pages.

By the time I loaded:

- The document text
- My system instructions
- The "personalized" context retrieved from my database

I had almost no room left for the model to "think" or generate a structured response. I constantly hit the ceiling, forcing me to truncate data and compromise on accuracy.

I even tried to engineer my way around this using a Map-Reduce strategy: splitting the PDF into smaller chunks, summarizing them individually, and then aggregating the results. It didn't work. The fragmentation destroyed the global context needed to link concepts across pages, resulting in disjointed and unusable outputs.

### Hallucination & The "Parrot" Effect

This was the ultimate dealbreaker. The model struggles significantly with prompt engineering techniques that act as standard guardrails for other models.

In standard LLM development, "Few-Shot Prompting" (giving the model examples of good input/output) is a standard way to improve reliability. When I fed this model examples to guide its reasoning:

- **Expectation**: It learns the pattern of the response.
- **Reality**: It would often parrot the examples back to me, completely ignoring the actual source document.

It felt like the model was "over-fitting" to the prompt structure rather than reasoning through the instructions.

### The Async Multi-Agent Experiment

Given the zero-cost nature of the model, I attempted another workaround: breaking down the workflow into smaller, independent tasks handled by multiple LLM sessions running asynchronously. The idea was simple—if a single session couldn't handle the full context, perhaps multiple specialized agents could each handle a piece.

This approach failed for two reasons:

- **Parrot Effect at Scale**: Each independent session suffered from the same prompt-overfitting issue. When one agent's output became another's input, the downstream agents would simply echo the structure without adding meaningful analysis.
- **Lost in Context**: Without shared memory or a sufficiently large context window, critical information was dropped between sessions. The final output felt disjointed and missed key connections that required understanding the full document.

Even with async execution minimizing latency, the user experience was unacceptable. The outputs lacked coherence, and the reliability was too low for production use.

### Ecosystem Friction

If you are used to the rich ecosystem of Python, LangChain, or LlamaIndex, you are on your own here.

- **No LangChain**: Support is non-existent or requires a massive amount of glue code.
- **No MCP**: Standards like the Model Context Protocol (MCP) don't work out of the box. You have to use Apple's specific "Tool Call" APIs.
- **User Friction**: The user has to manually install Apple Intelligence in Settings. Adding a "Please go to Settings and wait for a multi-GB download" step is a conversion killer for onboarding.

## Phase 3: The Verdict - Match Capabilities to Requirements

I believe the Apple Foundation Model has a place. I have seen multiple cool capabilities, but it is currently strictly defined. It is excellent for tasks that are short, extractive, and local. It fails at tasks that are long-context, agentic, or structural.

### Capability Summary

| Feature Use Case  | Apple Foundation Model Suitability | Reason                                                             |
| ----------------- | ---------------------------------- | ------------------------------------------------------------------ |
| Summarization     | ✅ Excellent                       | Fast, private, handles short paragraphs well.                      |
| Smart Reply       | ✅ Excellent                       | Zero latency, good tone matching.                                  |
| Data Extraction   | ⚠️ Mixed                           | Good for dates/times, bad for complex entities.                    |
| Complex Reasoning | ❌ Failed                          | Prone to "Parrot Effect" (repeats examples) instead of true logic. |
| Document Analysis | ❌ Failed                          | 4k context window is too small for RAG + Docs.                     |

## Conclusion

For now, I am switching back to a server-side solution. I'll have to pay the inference cost, but the performance will be stable, community is more vibrant, and the tools are mature enough to get the reasoning capability I need.

Apple's engineering is marvelous, but physics is physics. A heavily quantized 3B model with a 4k context window simply cannot handle the reasoning density required for a personalized agentic workflow. I might revisit this when the hardware can support more context window, but for now, privacy and cost aren't worth the trade-off in utility.
