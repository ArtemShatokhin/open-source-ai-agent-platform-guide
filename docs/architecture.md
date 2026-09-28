# How an Open Source AI Agent Platform Works

Kortix is the open-source AI Management System. Here is the architecture, in order — six layers, where the sixth commits work back into the first.

## 1. One git repo that is the company

Agents, skills, memory, connector config and triggers are all text in one repo — not settings in someone else's database, files you own. `kortix.yaml` declares the machine image, the connectors and the triggers. Agents and skills are markdown; memory is files that accumulate.

## 2. Connectors — every tool the company runs on

Wire up Slack, docs, tickets, CRM, billing and code once, then scope which agent may touch which one. 3,000+ apps in a click, plus MCP, OpenAPI, Postman, GraphQL and raw HTTP. Credentials are brokered server-side and never enter the machine.

## 3. Any model, keep your keys

Kortix is model-agnostic. Pick the model per agent, per session or per message. Bring your own API key from any major provider, or your own models behind any OpenAI-compatible URL.

## 4. The harness

A model answers; the harness gives it planning, tool use and multi-step runs it actually finishes — powered by OpenCode, configured by a file in the repo. Say allow, ask or deny per tool, down to a single shell command.

## 5. Every session gets its own computer

Each session boots its own isolated Linux machine with your repo and tools already on it. The agent can install, run and break anything — only commits survive. Thousands run in parallel with no crossover.

## 6. One gate to land work

Start, watch and steer every agent from web, Slack, terminal, or nobody at all (cron and signed webhooks). Work commits back to the repo as a change request you read as a diff first.

```text
project (git repo + kortix.yaml)
  └─ session ──> cloud computer: isolated sandbox on a branch named after the session
       └─ the OpenCode agent works
       └─ change request ──> you review & merge ──> main
```

Next: [self-hosting](self-host.md) · [comparison](comparison.md)
