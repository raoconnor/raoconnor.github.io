---
layout: post
title: "AI agents don't have a budget problem. They have a visibility problem."
description: "What a Nutanix Agent Gateway is, who needs one, and why it puts cost data where decisions are made."
date: 2026-11-15
categories: [finops, ai]
tags: [FinOps, AI, Nutanix, AgenticAI, PlatformEngineering, HuggingFace]
---

*What a Nutanix Agent Gateway is, who needs one, and why it puts cost data where decisions are made.*

If you run infrastructure, you already know this pattern. A new technology arrives, developers adopt it fast, and the bill turns up weeks later with no clear owner. It happened with public cloud, and it's happening again with AI agents.

The difference this time is that the costs are harder to see, and the people making the choices that drive them often never see the cost at all.

## First: how AI models actually cost money

You don't need to be an AI expert for this. Three ideas cover most of it.

**1. You pay per token.** A token is a small chunk of text, roughly three-quarters of a word. Every request is billed on the tokens you send in (the prompt plus any documents or history attached to it) and the tokens the model sends back. Output tokens usually cost more than input tokens. On a private model the meter is GPU time rather than an invoice, but the logic is the same.

**2. Model choice is a big multiplier.** A large frontier model can cost an order of magnitude or more per token than a small one. Many tasks, such as classifying a ticket, summarizing a log or pulling fields out of a form, don't need the biggest model. Without guidance, though, developers tend to default to it.

**3. Agents multiply everything.** A chatbot makes one call per question. An agent makes many: it plans, calls a tool, reads the result, thinks again and calls another tool. Each step re-sends a growing context. One user request can become 10, 20 or 50 model calls. A retry loop or a badly scoped task can burn through tokens overnight while nobody is watching.

So cost isn't set when you buy the GPUs or sign the API contract. **It's set every day by the developer who picks the model, writes the prompt and decides how the agent loops.** That's the gap.

## Training vs. inference: two very different cost profiles

AI costs fall into two categories, and they behave differently.

**Training** is how a model is built. It means feeding a huge amount of data through a large GPU cluster for weeks or months. It's very expensive, and for nearly every enterprise **it's someone else's cost**: the model builder pays it, and you download or call the finished result.

What enterprises do in practice:

- **Fine-tuning:** adapting an existing model with your own data. It's a scaled-down version of training: a significant but bounded GPU burst, measured in hours or days, that you run occasionally.
- **Inference:** using the model, so every question asked, every summary produced and every agent step. This is where your money goes, and it **never stops**. It grows with every user, every agent and every new use case.

| | Training / fine-tuning | Inference |
|---|---|---|
| When | Project-based, occasional | Continuous, 24/7 |
| Cost shape | Large, one-off GPU burst | Small per request, enormous at scale |
| Who drives it | Data science team | Every developer, app and agent |
| How to control it | Project budget | Model choice, quotas, routing, right-sizing |

**The takeaway for admins:** inference is the long-term cost line, and it's driven by decisions spread across many teams. That's exactly why you need visibility at the point of use.

On-prem, inference cost shows up as **GPU capacity**: how many GPUs a model needs, how well they're used, and whether they sit idle. Keep that in mind; we'll come back to it.

## What problem does an Agent Gateway solve?

Picture how most organizations use AI today:

- Team A calls a public LLM directly with an API key stored in their app config.
- Team B runs a private model on-prem, pulled straight down from Hugging Face, the public hub where thousands of open models are shared.
- Team C's agent connects to GitHub, a ticketing system and an internal database through MCP servers (MCP is the standard way agents connect to tools and data).

Each of these is a separate, direct connection. Infra and finance see only totals: one invoice and one GPU cluster. Nobody can answer basic questions:

- Which team, app or agent is using the tokens?
- Is this spend producing value, or is it a runaway loop?
- Which agents can reach which tools and data?
- What happens when a key leaks or a developer leaves?

**An Agent Gateway is a single control point between your agents and apps on one side, and the models and tools they use on the other.** Every request goes through it. Nutanix Agent Gateway is part of Nutanix Enterprise AI and provides:

- **One endpoint for public and private models**, so apps call the gateway rather than each provider directly
- **Token tracking per team, user or agent**, so usage is assigned to an owner
- **Consumption quotas and token-based rate limits**, so budgets are enforced, not just reported
- **MCP server governance**: access policies and tool-level filtering for which agents can use which tools

If you've run an API gateway, a web proxy or a cloud landing zone, the idea will feel familiar. It's the same pattern applied to AI.

## The big idea: putting data in the decision path

This is the point I'd most like admins to take away.

Most cost tools are **after the fact**. You get a report, find the overspend and send an email. By then the money is spent and the design decisions are locked in.

A gateway moves cost data **into the decision path**:

- **Developers** see token usage for their own agent while building it, not at month end. "This agent uses 40K tokens per task. Could a smaller model handle step 3?" becomes a normal engineering conversation.
- **Admins** set quotas and limits before a runaway agent becomes an invoice. An alert at 80% of budget prompts a conversation, and a hard stop at 100% keeps the problem contained.
- **Platform teams** can route by cost, sending high-volume, low-difficulty work to private models on infrastructure they already own and keeping premium public models for tasks that need them.
- **Finance and leadership** get showback by team, which is the base for asking the real question: what does this agent cost per useful outcome?

The gateway doesn't make cost decisions for anyone. It makes sure the people deciding have the data when it can still change the outcome.

## Running your own models: the Hugging Face story

Back to Team B. Their path is very common: someone finds a promising open model on Hugging Face, pulls it down and deploys it on-prem. It looks free because there's no per-token invoice. But someone still pays for the GPUs, the power, the engineers' time, and the risk.

Nutanix Enterprise AI can connect to Hugging Face and import models directly, but it treats them as **two categories**, and the difference matters for both cost and risk.

### Category 1: Nutanix-validated models

These models have been tested and pre-validated by Nutanix to run on the platform. What that gives you:

- **Lower supply chain risk.** A model file is software. Public hubs can host tampered weights, files that run code when they're loaded, or models with unclear licensing. Using a model that has already been validated cuts down on what you're trusting blindly.
- **Known GPU and resource requirements.** You know before you deploy how many GPUs it needs, what type, and how much memory. You can plan capacity and cost before you spend.
- **Predictable deployment.** It's known to work, so you're not troubleshooting at 2am.

### Category 2: Non-validated models

You can also pull other models from Hugging Face. That flexibility is real and valuable: a new model, a specialized model for your industry, or one your data scientists need to test.

But you take on the work Nutanix would otherwise have done:

- **No pre-validation:** the model hasn't been tested on the platform.
- **No published GPU or resource requirements:** you have to work out sizing yourself.
- **A higher chance of failure:** if you size it wrong, the deployment fails or performs poorly.

### Why this is a cost conversation, not just a technical one

A model's size largely sets the GPU memory it needs. A rough rule of thumb is **parameters × bytes per parameter**. A 7-billion-parameter model at standard 16-bit precision needs about 14 GB of GPU memory for its weights alone. A 70-billion-parameter model needs about 140 GB, which means several GPUs, plus extra headroom for each concurrent user.

Get that wrong with a non-validated model and the costs pile up quickly:

- **Undersized:** the deployment fails, an engineer spends a day troubleshooting, and the project slips.
- **Oversized:** expensive GPUs are reserved for a model that needed half of them, and they stay reserved indefinitely.
- **Trial and error:** every failed attempt ties up GPU capacity that other teams could have used.

This is the same theme again: **data in the decision path**. A validated model gives you the resource requirements *before* you commit GPUs, so the decision is made with data rather than by trial and error.

### A practical policy

- **Default to validated models** for production workloads.
- **Allow non-validated models** in a sandbox with a time-boxed GPU allocation, for evaluation and innovation.
- **Promote a model to production** only after your own testing has documented its resource needs, security review and licensing.
- **Route everything through the gateway**, so validated or not, every model's usage is tracked and attributed to an owner.

That gives developers the flexibility they want and gives admins the predictability they need.

## Who needs it?

You probably need one if any of these are true:

- More than one team is building with LLMs or agents
- You use both public AI APIs and private models
- Teams are pulling open models from Hugging Face to run on-prem
- Agents are, or soon will be, connecting to internal tools and data via MCP
- Someone has already asked, "Why is our AI bill so high?" and nobody could answer
- You have security or compliance rules on where data can go and who can reach it

If you have a single team running one proof of concept, you can probably wait. Plan for it before the second or third team arrives, though. Adding governance after the fact is always harder.

## What does it take to set up?

At a high level, adopting the gateway involves:

1. **The platform:** Agent Gateway ships with Nutanix Enterprise AI (GA in 2.7; MCP server management went GA in 2.8). If you already run Enterprise AI, you have the foundation.
2. **Choose your models:** start with Nutanix-validated models where possible, so GPU sizing is known up front. Use non-validated Hugging Face models deliberately, in a sandbox first.
3. **Register your models:** your private models served on Nutanix, plus any external providers. Provider API keys live at the gateway, not in every app's config.
4. **Define your consumers:** teams, users and agents, each with their own access and identity.
5. **Set policy:** quotas, rate limits and which MCP servers and tools each consumer can reach.
6. **Repoint apps:** developers send requests to the gateway endpoint instead of directly to providers.
7. **Review the data:** usage by consumer becomes your showback report and the start of your FinOps conversations.

The technology is the easy part. The real work is agreeing on **who owns which budget** and **what a sensible quota is**. Start by watching usage without enforcing limits, learn the patterns, then set limits.

## The bottom line

AI agent costs are set by many small decisions made by developers and platform teams every day: which model to use, where it comes from, how much GPU it's given, and how the agent loops. You can't govern what you can't see, and you can't change a decision after it's made. An Agent Gateway, together with validated models that come with known resource requirements, puts usage and cost data where the decisions happen. For infrastructure teams it's a chance to be the people who enable AI safely and affordably, not the people asked to explain the bill.

*Next in this series: public API vs. private inference, and how to work out the real break-even.*

*Views are my own.*
