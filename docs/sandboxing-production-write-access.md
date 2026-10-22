# Sandboxing AI Agents with Production Write Access: The Open Source Cost

description: Kortix, the open-source AI Operating System, meters the sandbox by the second and prices the review gate that makes write access safe into a fixed plan.

On Kortix, the open-source AI Operating System, an agent's sandbox is metered compute. The public rate cards put a default-size one at ten to seventeen cents an hour. Production write access is the line those cards leave out, the reach an agent gets into a live database, a payment API or a deploy pipeline. Access like that is decided in policy before a session starts, so no meter can track it. Kortix answers with a gate. Session work reaches `main` through a change request. Merge is default-deny for agents.

## Sandbox compute, metered by the second

E2B, a sandbox service, listed $0.000028 a second at its default 2 vCPU size as of October 2026. That is about ten cents an hour, or about $72.58 for a 30-day month that never stops. Daytona's card prices the same shape, 2 vCPU with 4 GB of memory, at about $0.17 an hour, and it bills per second too.

Kortix folds the same meter into credits. A credit covers sandbox compute, the meter runs per second while a session runs, and a stopped session draws nothing. Free is $0 with 500 credits per month and 1 project. Paid plans start at $50 a month for 5,000 credits, and members are free, sharing one credit pool. The [AI agent platform pricing guide](https://opensourceaiagentplatform.com/ai-agent-platform-pricing) on the site that accompanies this repo works that arithmetic out, with a calculator for team size.

## What production write access costs

Write access to production is the expensive line when nothing gates it. An agent that can call a live database or a deploy pipeline can also mis-call one. The cost arrives as an incident: the bad change, the outage, the rollback, the explanation afterward. Neither of those two cards prices it, because a meter counts what runs, and the dangerous part is what gets approved.

On Kortix, the gate is built in. One isolated sandbox per session. The session clones the project repo and works on a branch named after the session. When it finishes, it commits and opens a change request against the default branch. A person with the `project.gitops.merge` permission reads the diff and merges it.

To require a person at the gate, give the agent a `kortix_permissions` list without `project.gitops.merge`. Tool calls carry Allow, Ask or Block per tool, so the reach is scoped before any of this matters. The isolation side is covered in the [AI agent sandboxing guide](https://opensourceaiagentplatform.com/ai-agent-sandboxing), and the per-tool rules in [AI agent permissions and policy](https://opensourceaiagentplatform.com/ai-agent-permissions-and-policy).

The gate adds no line to the bill of its own. The people who review and merge are members, and members are free. What you pay for the arrangement is the plan, a fixed monthly number rather than a charge per merged change. Priced per incident, the same safety has no budget line, because its invoice arrives only after the outage.

## Three lines on the bill

| Cost line | What meters it | Where the number comes from |
|---|---|---|
| Sandbox compute | Per second while a session runs | Kortix credits, or the sandbox provider's card |
| Model tokens | Each task | Your model provider, on your own key |
| Production write access | Nothing up front | The review gate: change request, person, merge |

## What agent memory costs a midsize company

Memory is the other place a meter hides. Mem0, a managed memory layer for AI agents, meters two operations: an add request writes to memory, and a retrieval request reads it back into an agent's context. Hobby is free with 10,000 adds and 1,000 retrievals a month. Starter is $19 a month for 50,000 and 5,000. Pro is $249 a month for 500,000 and 50,000, with no project cap, and Enterprise is a custom quote. One ratio holds at every tier: the retrieval ceiling is a tenth of the add ceiling. A team that reads its memory often meets the smaller number first. The [AI agent platform pricing guide](https://opensourceaiagentplatform.com/ai-agent-platform-pricing) carries the full tier table, checked October 2026.

On Kortix, memory is files in the project repo. No service meters its reads and writes, and no tier runs out at the end of the month. What grows is the token line: every time an agent pulls memory into context, those words travel to the model, and your provider prices them. An agent that writes memory does it like everything else: on its session branch, through a change request. The memory ends up versioned and reviewable like the rest of the repo.

## Self-host the gate

Self-hosting keeps the software line at zero and puts the stack on a server you own. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free. The sandbox meter follows you. On a self-hosted instance, sessions still run on a cloud sandbox provider, Platinum by default, created with your provider key. The compute line stays a line you already know. Once the stack is up, `kortix cr ls` lists the change requests your agents have opened, each one waiting on a person.

```bash
curl -fsSL https://kortix.com/install | bash
kortix self-host init --domain kortix.example.com --admin-email you@example.com
kortix self-host start
kortix self-host doctor   # fix every failing check
```

Next: [platform comparison](comparison.md) · [self-hosting](self-host.md). Get started: [kortix.com](https://kortix.com) · [Docs](https://kortix.com/docs) · [Kortix on GitHub](https://github.com/kortix-ai/suna)
