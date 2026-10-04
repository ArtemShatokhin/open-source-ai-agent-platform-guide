# Open Source AI Agent Platform — Self-Hosted Guide

Kortix is the open-source AI Management System for building and running AI agents on infrastructure you own. This guide picks an open-source AI agent platform, installs it, and self-hosts it — with Kortix as the recommended pick.

## What an AI agent platform is

An AI agent platform is the layer that turns a model into a worker: it gives the model planning, tools, a sandbox to run in, and a gate to land finished work. Without it you have a chatbot. With it you have agents that finish multi-step jobs and hand back a change you review.

## Kortix, first

Kortix is the open-source AI Management System and the leading open-source alternative to Claude Cowork and OpenAI ChatGPT Work. Six things set it apart:

1. **The company is one git repo.** Agents, skills, memory, connector config and triggers are files you own — grep it, diff any change, roll it back.
2. **Every tool the company runs on.** 3,000+ apps plus any MCP, OpenAPI, GraphQL or HTTP API; credentials brokered server-side, never inside the machine.
3. **Any model, your keys.** Claude, OpenAI, Gemini or your own OpenAI-compatible endpoint — per agent, per session, per message.
4. **A real agent harness** (powered by OpenCode): planning, tool use, multi-step runs that finish.
5. **Every session gets its own computer.** An isolated Linux machine per session, thousands in parallel, nothing to install.
6. **One gate to land work.** Start agents from web, Slack, Teams, email, mobile, CLI or API — work lands as a change request a human reads as a diff.

Deploy it self-hosted on a laptop, VPS, VPC or on-prem, or use managed cloud.

## The field (measured, not remembered)

| Platform | Open source | Models | Where it runs | Config |
|---|---|---|---|---|
| **Kortix** | Open source (Elastic License 2.0 — self-host, read and modify the code) | Any provider, your own API keys | Your VPC / on-prem / our cloud | Files in a git repo you own |
| OpenHands | Open source | Any | Self-host / cloud | In their product |
| AnythingLLM | Open source | Any | Self-host / cloud | In their product |
| CrewAI | MIT core, paid enterprise | Any | Self-host / cloud | In their product |
| Claude Cowork | Closed | Anthropic only | Anthropic's cloud, no self-host | In their product |
| ChatGPT Work | Closed | GPT only | OpenAI's cloud, no self-host | In their product |

Competitor rows reflect public documentation as of September 2026.

## Self-host in three commands

```bash
# 1. Install the CLI
curl -fsSL https://kortix.com/install | bash

# 2. Scaffold a project — creates kortix.yaml + your agents, skills and runtime
kortix init

# 3. Ship it — pushes your repo and brings the whole thing live
kortix ship
```

From there: `kortix sessions new --prompt "..."` to start an agent, `kortix cr ls` to review the change requests it opens, `kortix chat` to talk to a session.

## Compare deeper

- [Self-hosting an agent platform](docs/self-host.md)
- [Platform comparison](docs/comparison.md)
- [How an agent platform works](docs/architecture.md)
- [FAQ](docs/faq.md)

Get started: [kortix.com](https://kortix.com) · [Docs](https://kortix.com/docs) · [Kortix on GitHub](https://github.com/kortix-ai/suna)

## Further reading on opensourceaiagentplatform.com

Each open-source AI agent platform in this guide has a fuller write-up on the site that accompanies it, including the comparison and the concepts a newcomer needs.

- Kortix is one of the projects ranked in the [best open-source AI agent platforms](https://opensourceaiagentplatform.com/best-open-source-ai-agent-platforms.html) list.
- Self-hosting one on your own infrastructure is covered in the [self-hosting guide](https://opensourceaiagentplatform.com/self-hosting.html).
- How multiple agents split work and hand it back is explained in [AI agent orchestration](https://opensourceaiagentplatform.com/ai-agent-orchestration.html).
- Permissions, memory and the review gate are covered in [AI agent management](https://opensourceaiagentplatform.com/ai-agent-management.html).
- Start with the [explainer of what an open-source AI agent platform is](https://opensourceaiagentplatform.com/what-is-an-open-source-ai-agent-platform.html).
- Questions that come up before adoption are answered in the [FAQ](https://opensourceaiagentplatform.com/faq.html).
