# What a free open source AI agent platform plan includes: 45 agent-hours

description: Kortix's free plan is 500 credits a month, about 45 hours of agent computer, and here is the arithmetic of a three-person team against it.

Free on an open source AI agent platform is a quantity, not a feature list. Kortix's free plan costs $0 and renews 500 credits every month with no card, on one project. At the published rate of 11 credits an hour, that is about 45 hours of agent computer. Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. Read the hours first. The models, the project count and the connectors only matter once you know whether 45 hours covers your week.

## What does the free plan actually include?

The [pricing page](https://kortix.com/pricing) lists four things, checked October 2026:

- 500 credits a month, no card needed
- one project
- managed models, with no key to add
- your own API key for a premium model, or a ChatGPT subscription connected to the account

The grant renews monthly, so nothing expires and no trial clock runs out. What ends with the month is capacity, not access. People on the account share one credit pool, and adding a person does not add a line to the bill.

A project is one `kortix.yaml` plus the files around it. The starter manifest fits on a screen: a schema version, a default agent, the project name, five tools every agent inherits, a triggers block.

```yaml
kortix_version: 2
default_agent: kortix

project:
  name: "{{projectName}}"
  description: A Kortix project.

tools:
  web_search: kortix:web_search
  image_search: kortix:image_search
  scrape_webpage: kortix:scrape_webpage
  memory: kortix:memory
  show: kortix:show
```

Credits buy the agent computer, the cloud machine that runs the agent's code, browses and edits files. Its shape is 2 vCPU, 4 GB of memory and 20 GB of disk, billed per second and free while stopped. One credit is $0.01, so eleven credits buy an hour, and the page states the short form as about five minutes per credit. Model calls sit on a second meter. Your own keys send model spend straight to your provider. The first $1,000 a month passes through with no fee, and 5% above that comes out of credits. A connected ChatGPT subscription spends no credits at all. Kortix's own models cost the provider's base price plus 5.5%, out of the same pool.

## Credits are hours, and a trigger spends them while nobody watches

The manifest is where a free allowance quietly leaks. A trigger starts a session with nobody at the keyboard.

```yaml
triggers:
  - slug: harness-reflector
    name: Harness reflector
    type: cron
    agent: harness-reflector
    enabled: false
    cron: "0 0 3 * * *" # 03:00 daily
    timezone: UTC
```

The starter ships that trigger switched off, and the comment beside it gives the reason: every run creates a sandbox. Take a 15-minute run at the published rate. That is 2.75 credits, or 83 credits across 30 days, close to a sixth of the free month spent by a job nobody watches. A second daily trigger of the same length commits another sixth. Two of them take a third of the allowance before a person starts any work.

Session length moves the same number at smaller scale. A 25-minute session is 4.6 credits and a 90-minute session is 16.5, so one task in a different shape costs three times the capacity. So the useful question about a free plan is how long sessions run and what fires without you.

## A three-person team against 500 credits

Take three people, two sessions each a day at 25 minutes, across 21 working days. Round numbers for a task-shaped workload, and they turn the allowance into a verdict.

| Line | Arithmetic | Result |
| --- | --- | --- |
| Sessions in the month | 3 people × 2 × 21 days | 126 |
| Minutes of agent computer | 126 × 25 | 3,150 |
| Agent-hours | 3,150 ÷ 60 | 52.5 |
| Credits at 11 an hour | 52.5 × 11 | 578 |
| What 500 credits covers | 500 ÷ 11 | 45 hours |
| Share of the month covered | 45 ÷ 52.5 | 86% |
| Hours left uncovered | 52.5 less 45 | 7.5 |

Eighty-six percent is the whole answer. It covers most of a small team's month and fails in the last week. Read the allowance the other way and it is about 2.1 hours of agent computer per working day for the whole account, or 43 minutes per person. That is enough to put agents on real work and judge the output. It is short of a team that leans on them daily. The 7.5 uncovered hours are 82 credits, under a dollar of compute, with no card on file to buy them.

**Where free stops fitting.** A team that will run past roughly 45 hours a month should take the credit tier, which starts at $50 a month. The tier shown on the page, $200 a month, carries 20,000 credits and about 1,830 hours, some forty times the free allowance. A team that wants no configuration of its own to own should take a closed product such as Claude Cowork or ChatGPT Work, and give up self-hosting. A team whose computers stay on around the clock should price self-hosting instead of credits.

## Self-hosting moves the bill, not the capacity

Self-hosting deletes the plan fee and keeps the compute bill, which is the part most free plan write-ups leave out. The [self-hosting docs](https://kortix.com/docs/host) describe what an operator takes on. One Linux server with a public IP. A and AAAA records for the domain and its API subdomain, plus a wildcard for previews. Ports 80 and 443 open inbound. A Platinum API key is mandatory. Sessions run on Platinum sandboxes rather than on the server you rented, and without the key sessions do not start. The model key goes in after the first sign-in.

The stack is one Docker Compose project. It holds the web app, the API, a standalone LLM gateway, and a migration job that runs before the API starts. A daily updater pulls new images and rolls the services, Caddy terminates TLS, and the Supabase distribution brings Postgres, auth and storage. `kortix self-host doctor` reports what is failing. Account creation stays open to anyone who reaches the instance until you restrict it or set mail.

The software costs nothing and the operation is yours. You rent the server, pay a sandbox key that bills by the second, send model calls to your own providers, and staff the person who reads the doctor output. The [pricing guide for the field](https://opensourceaiagentplatform.com/ai-agent-platform-pricing) puts software, compute and model spend side by side before you commit to that route.

## Compare the field before you commit

A free plan answers one question: how much agent work runs before the first metered session. Every platform answers it in a different unit. Some renew a grant, some reset an execution cap, some hand out a credit once. The [free-tier comparison](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-free) measures the 500 credits against those alternatives.

Kortix runs on your own keys and self-hosts on a laptop, VPS, VPC or on-prem network. Session work lands as a change request a person reads as a diff. The [platform comparison](comparison.md) covers OpenHands, AnythingLLM, CrewAI, Claude Cowork and ChatGPT Work, on source, models, hosting, configuration and the review gate. The [compute sizing page](sizing-agent-compute.md) prices the same hours on the other rate cards, and the [self-hosting guide](self-host.md) covers the install. The code is at [Kortix on GitHub](https://github.com/kortix-ai/suna) if you would rather read the manifest than the pricing page.
