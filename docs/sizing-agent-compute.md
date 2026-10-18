# What an Open Source AI Agent Session Costs per Hour

description: An agent session costs 10 to 17 cents an hour on the published rate cards; Kortix, the open-source AI Operating System, prices the same machine in credits.

An agent session costs about 10 to 17 cents an hour on the published sandbox rate cards, and Kortix is the open-source AI Operating System that prices the same machine in credits. One credit is a cent, the default computer burns about 11 an hour, and a stopped session draws nothing. The rates come from each vendor's own pricing page, checked October 2026.

## What an agent session reserves

Every session runs on a sandbox with a fixed shape: vCPUs, memory, and disk. Kortix runs one isolated sandbox per session, and its default computer is 2 vCPU, 4 GB of memory, and 20 GB of disk. All three cards meter that machine by the second while it runs, and none charge for a stopped one.

The hours come from the work, and the work does not wait for a person. A cron schedule or a signed webhook can start a session with nobody at the keyboard, so a team's compute arrives in bursts that follow the workload instead of the calendar. A budget built from headcount misses the overnight runs, the retries after a failed step, and the jobs that outlast the workday. Hours are the unit that matters, and the cards price them differently.

## The three rate cards, side by side

| Platform | What it meters | The rate | Free allowance and the plan around it |
|---|---|---|---|
| Kortix | Sandbox seconds | 1 credit is $0.01; the default machine about 11 credits an hour | 200 credits a month, then tiers from $50/mo |
| E2B | Sandbox seconds, by size | $0.000028/s at the default 2 vCPU, about $0.10 an hour | $100 one-time credit; Pro adds a $150/mo base |
| Daytona | vCPU, memory and storage seconds | $0.0504 per vCPU-hour; $0.0162 per GB-hour | $200 free compute; pay as you go |

Kortix bills storage by the month, at $0.0526 per GB; the full rate card, including the 11-credits-an-hour default computer, is at [kortix.com/pricing](https://kortix.com/pricing). E2B, which rents running sandboxes by the second, sets its rate by size: from $0.000014 a second at 1 vCPU to $0.000112 at 8 vCPU. Its free tier caps sessions at one hour with 20 sandboxes in flight. Pro lifts the cap to 24 hours and 100 sandboxes for a $150 monthly base. E2B's own estimator puts 100 sandboxes running 10 hours a month at $316. Enterprise starts at a $3,000 monthly minimum. Daytona, another sandbox provider, prices every component separately: $0.0504 per vCPU-hour, $0.0162 per GB of memory each hour, and $0.000108 per GB of storage each hour. The first 5 GiB of storage is free. [E2B](https://e2b.dev/pricing) and [Daytona](https://www.daytona.io/pricing) calculate every bill per second.

## A 25-person team, priced three ways

Take 25 people who each run about two hours of agent work a day over 21 working days. That is 1,050 agent-hours a month, and each card prices the same hours from its own meter.

| Card | The 1,050-hour month | The plan on top |
|---|---|---|
| Kortix | 11,550 credits, or $115.50 | The $200 tier's 20,000 credits cover it, 8,450 to spare |
| E2B | $105.84 of sandbox usage | A $150 Pro base, so about $256 all in |
| Daytona | $173.88 | $200 of free compute covers the first month |

The trap is the machine that never stops. One E2B sandbox at the default size costs about $72.58 for a 30-day month that runs around the clock. The matching Daytona shape costs about $119.23. On Kortix, 720 hours of the default computer is 7,920 credits, or $79.20. Leave all 25 running and the same month costs about $1,814, $2,980, and $1,980. The burst month and the always-on month differ by 17 times, and that ratio is the reason to read the meter before the plan. These numbers price compute alone; model tokens ride on your provider's key, or on the same credit pool when a managed model runs the session.

## How sizing changes the decision

Bursts favor a per-second meter. A team that stops idle sessions pays for work and nothing else, so the 1,050-hour month is the whole compute bill. An always-on footprint wants the opposite arrangement: committed use. E2B sells larger tiers on committed usage with custom billing, Daytona advertises volume discounts as a team scales, and both push a steady footprint toward a negotiated rate. A reservation punishes bursts with a fixed bill.

A credit pool lands between the two. Kortix keeps the per-second meter and wraps a monthly pool around it, so the bursts come out of the pool and a stopped session draws nothing. Free is $0 with 200 credits per month and 1 project. Paid plans start at $50 a month for 5,000 credits, and members are free, sharing one credit pool. At 11 credits an hour, that $50 tier buys about 455 hours of the default computer. The per-seat route looks simpler to budget because the number is fixed by headcount. The rate cards say the hours set the cost, and a seat bill holds still while they swing. On Kortix the 25-person team adds no per-person line at all; the pool is what the work draws on.

## Next steps

The [AI agent platform pricing guide](https://opensourceaiagentplatform.com/ai-agent-platform-pricing) splits every bill into software, compute, and tokens, and carries a calculator for team size. The [self-hosting guide](https://opensourceaiagentplatform.com/self-hosting) shows where the compute line lands when the stack runs on your own server. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.

Next: [platform comparison](comparison.md) · [sandboxing with production write access](sandboxing-production-write-access.md). Get started: [kortix.com](https://kortix.com) · [Docs](https://kortix.com/docs) · [Kortix on GitHub](https://github.com/kortix-ai/suna)
