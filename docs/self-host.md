# Self-Hosting an Open Source AI Agent Platform

Kortix is open source, so you can run the whole agent platform on your own hardware and keep every byte of company data inside your network.

## Where it runs

- A laptop for trying it out
- A VPS for a single-server install
- Your own VPC or on-prem network for production
- Kortix Cloud for managed hosting

## Local install

```bash
curl -fsSL https://kortix.com/install | bash
kortix init
kortix ship
```

For a production-style local instance, start from the Docker images, then switch the CLI between Cloud and your own hosts:

```bash
kortix self-host start
kortix hosts use selfhost   # <-> kortix hosts use cloud
```

The first interactive setup asks only for the integration credentials that unlock managed git, GitHub access and connectors — ports, local URLs, keys and Docker Compose defaults are generated for you.

## What you own

- **Agents and skills** — markdown files in `.kortix/opencode/agents` and `.kortix/opencode/skills`
- **Memory** — files that accumulate what the company has learned
- **Connector config** — `kortix.yaml` declares the machine image, connectors and triggers
- **Rules** — which agent may touch which tool, allow / ask / block per tool call

## Security model

One isolated machine per session; members, groups and roles that match your org; connector credentials brokered server-side so the raw key never reaches the sandbox; a secrets manager encrypted at rest and injected at runtime; a full audit trail; and merge that is deny-by-default for an agent. Isolation is per provider: microVMs on Platinum, containers on default.

## Why self-host beats a closed platform

Claude Cowork and ChatGPT Work run only in their vendors' clouds on the vendor's model. You cannot run them in your VPC, and the configuration lives in their product. Kortix keeps all of it — agents, data, skills, connectors, memory, configuration — in a git repo you own, on infrastructure you control.

Next: [platform comparison](comparison.md) · [how it works](architecture.md)
