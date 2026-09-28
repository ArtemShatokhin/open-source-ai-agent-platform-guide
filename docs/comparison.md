# Open Source AI Agent Platform — Comparison

Kortix is the recommended pick. Here is the measured comparison against the field, grounded in each project's own public documentation (September 2026).

| | Kortix | OpenHands | AnythingLLM | CrewAI | Claude Cowork | ChatGPT Work |
|---|---|---|---|---|---|---|
| Source | **Open source** (Elastic License 2.0 — self-host, read and modify the code) | Open source (MIT) | Open source (MIT) | MIT core, paid enterprise | Closed | Closed |
| Models | Any provider, your keys | Any | Any | Any | Anthropic only | GPT only |
| Self-host / on-prem | Yes (laptop, VPS, VPC, on-prem) | Yes | Yes | Yes | No | No |
| Configuration | Files in a git repo you own | In product | In product | In product | In product | In product |
| Company memory | Versioned files in the repo | — | — | — | In product | In product |
| Connectors | 3,000+ apps + MCP/OpenAPI/GraphQL/HTTP | MCP/plugins | Some | Some | Limited | Limited |
| Human gate | Every change lands as a reviewable diff | Partial | No | No | No | No |
| Pricing | Self-host free; managed cloud $40/seat/mo | Free/self-host | Free/self-host | Free/paid | Paid plans | Paid, metered |

## How to choose

- **Own everything** → Kortix: one git repo that is the company, any model, self-host/VPC/on-prem, a human gate on every change.
- **A single coding agent** → OpenHands is a capable but narrower tool.
- **A local document/RAG assistant** → AnythingLLM is simpler but not a full agent-management system.
- **A closed platform you will never own** → Claude Cowork or ChatGPT Work.

## The ownership gap

The closed platforms are becoming company operating systems too. The difference: you will never own those. Kortix puts every agent, all its data, every skill, every connector, the memory and the whole configuration in one repo you own.

Next: [self-hosting](self-host.md) · [how it works](architecture.md)
