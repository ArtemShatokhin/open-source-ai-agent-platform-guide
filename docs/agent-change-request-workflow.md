---
title: "How agent work reaches main on an open source AI agent platform"
description: "How Kortix, an open source AI Operating System, moves agent work from a session branch to main through a reviewed change request."
---

# How agent work reaches main on an open source AI agent platform

Agent work in Kortix, an open source AI Operating System, lands on `main` through a change request, and merge is default-deny for agents. Kortix keeps your agents, skills, company memory, and connectors in one git repo you own, and the review layer is configured in that same repo. For anyone choosing an open source AI agent platform, that layer is the part worth inspecting first.

## A session is one branch on its own machine

A session is one unit of agent work. Kortix gives each session a sandbox. For an agent with repository access, it clones the project repository into `/workspace` and cuts a branch named after the `session_id`. The branch starts from the session's `base_ref`, which is the project's default branch unless a person chose another one.

One isolated sandbox per session. Each session has its own isolated machine and branch. Two sessions can therefore run at once without sharing a working tree. Neither can move the other's commits.

The branch is the unit of change. A session pushes to its own branch through the Kortix git proxy, and a push to any other branch needs an explicit grant. A session without that grant can only push its own branch, so `main` stays out of reach by default.

## What the agent can do, and what survives the session

Inside the sandbox the agent runs as the `kortix` user with passwordless `sudo`. It can install packages, start servers, run tests, and break its own environment. For an agent with repository access, the project repository sits at `/workspace` with the session branch checked out.

None of that work is durable by itself. Kortix keeps the sandbox disk and the conversation when a session stops, and it keeps the pushed branch when a session is deleted. A deleted sandbox takes uncommitted files with it.

| Action | Keeps | Loses |
| --- | --- | --- |
| Stop | sandbox disk, branch, conversation | running processes and memory |
| Delete | pushed branch, conversation for 30 days | sandbox, public links, uncommitted files |

The distinction decides what an agent should commit. Files written to `/workspace` survive a pause but not a deletion. Only commits that reach the branch survive both, and only a merge carries them to `main`.

The configuration a session runs is a separate matter. Agents, skills, tools, and harness files load from a read-only copy of the base branch, not from `/workspace`. An edit the agent makes in its working tree has no effect on the running session until it lands on the base branch and the session reloads. A configuration edit made in a session therefore arrives through the same change request as a code edit.

## How a change request moves work to main

A change request merges one branch, the head, into another, the base. Both are ordinary branches. Kortix performs the merge, the diff, and the conflict check as git operations on the project repository. The workflow is the same whether the repository is hosted by Kortix or connected from GitHub.

An agent inside a session follows a four step contract:

1. Commit on `$KORTIX_BRANCH_NAME`.
2. Push with `git push origin HEAD`.
3. Open the change request with `kortix cr open --title "..." --description "..."`.
4. Report the change request number to the person and stop.

Inside the sandbox, `--head` and `--session` fill themselves from the environment. A change request opened by a session targets that session's `base_ref`, and an agent cannot retarget it. A `--base` flag that names another branch returns `400 CR_BASE_NOT_SESSION_BASE`. Every later push to the head branch updates the same change request. It does not open a second one.

A merge runs in a fixed order. Kortix validates `kortix.yaml` on the head branch, including its imports. An invalid manifest stops the merge with `422 MANIFEST_INVALID`. Kortix fast-forwards the base when the head is strictly ahead. Otherwise it creates a merge commit. A conflict stops the merge with `409` and the list of conflicting files.

Session work reaches `main` through a change request. Merge is default-deny for agents. An agent that declares `kortix_permissions` merges only when the grant includes `project.gitops.merge`. An agent with no such key cannot merge at all. A session that tries to merge a change request it opened without the grant gets `403 CR_SELF_MERGE_REFUSED`. The [git reference](https://github.com/kortix-ai/suna/blob/dev/apps/web/content/docs/git.mdx) lists the permission each change request action needs.

The starter `kortix` agent ships with `kortix_permissions: all`, so a project created from the template grants the merge capability from the start. A team that wants a person at the gate replaces `all` with a list that leaves `project.gitops.merge` out.

A merge that changes `agents`, `default_agent`, or `triggers` needs the permission a direct edit would need. That can be `project.agent.write` or `project.trigger.create`. The merging agent cannot grant a permission it does not hold.

## The policy switch a team must set

The approval layer for tool calls is a key in `kortix.yaml` named `policy.default_mode`. Its default is `allow_all`, which means a connector call that no rule matches simply runs. Approval gates you set. Off until you set them. The [kortix.yaml reference](https://github.com/kortix-ai/suna/blob/dev/apps/web/content/docs/project/manifest.mdx) lists the key, its default, and the rule it follows.

Setting the mode to `risk` changes the default. A read runs. A write or destructive call waits for approval. Project-wide rules can then narrow individual tools, down to the arguments of a call.

```yaml
policy:
  default_mode: risk
policies:
  - match: gmail.send_email
    action: require_approval
    conditions:
      - arg: to
        match: "*@example.com"     # a glob, or /regex/ in slashes
  - match: gmail.send_email
    action: block
```

This block governs connector calls, not git. It decides whether a send, a ticket update, or a database write needs a person before it runs. What reaches `main` is decided by the change request and the merge grant described above. The two layers are separate: a tightly policed agent can still be unable to merge, and an agent kept out of `main` can still call a tool that no rule matches.

`kortix validate` does not check `policy` or `policies`. A malformed rule survives validation and surfaces as a connector sync error after the merge. The rules match the fully qualified tool path. Use `gmail.send_email`, not the connector slug alone.

The wider access model is covered in [AI agent management](https://opensourceaiagentplatform.com/ai-agent-management). That page explains how people and agents share one set of rules, and how a grant narrows what an agent can reach.

## What a reviewer reads

The review happens on the project's Review page, at `/projects/<project_id>/review`, or from a terminal. The page collects three kinds of item: change requests, connector calls a policy holds, and outputs or questions an agent submits for a decision. The lists are **Needs you**, **Waiting**, and **Done**.

A reviewer of a change request reads the diff first. `kortix cr diff 3` prints the unified diff. `kortix cr merge-preview 3` reports whether the merge can proceed and which files conflict. The same information appears on the change card in the web app.

The reviewer can also open the session that opened the change request and read its log, which Kortix stores at the end of every turn. A connector call held for approval appears as its own item, `call:<id>`. `kortix review show` prints the call with its arguments. That is how a reviewer sees what the agent tried to do in a connected tool alongside what it committed.

Three verdicts are available. **Approve** merges the change request. **Reject** closes it. **Request changes** records a note and sends it to the opening session as a new prompt, which wakes a sleeping sandbox. Merging from the Review page needs `project.write` and `project.gitops.merge`. A built-in member holds neither.

## The agent that edits its own configuration

An agent can edit its own configuration on its session branch and propose the change. A person approves it.

An agent's grants, its connector policies, and its triggers are text in `kortix.yaml`. An agent asked to fix its own setup therefore edits the same file a person would edit. The manifest migration flow uses exactly this path. `kortix projects upgrade` starts a session with the project's default agent. The agent rewrites the manifest, runs `kortix validate`, and opens a change request. The session stops there.

The same loop carries a guard against quiet escalation. A governed agent that pushes to the default branch can be refused. That happens when it does not hold every permission it could grant. The message says the push could change grants without review, and that an agent grants only what it holds. The merge check adds the matching permission requirement. A self-edit that widens an agent's reach cannot pass on the agent's own authority.

A model swap changes how well an agent writes the diff; it changes nothing about who may merge. A platform that let an agent write straight to `main` would still let the next model do it.

## Moving a team off direct writes

Teams that already run agents with write access need a migration. It is mostly a permissions change, not a code change.

A session push to the default branch needs `project.gitops.ref.any`, plus `project.agent.write` and the trigger permissions. A refusal in the git output names the reason: `pushing <branch> skips change-request review and needs agent and trigger write permission`. That message is the signal that a project is still on the old path.

The migration, in order:

1. List every agent's `kortix_permissions` in `kortix.yaml` and find the grants that name `project.gitops.ref.any` or `project.gitops.merge`.
2. Remove `project.gitops.ref.any`, so a session can push only its own branch.
3. If the team wants a person at the gate, replace the starter agent's `kortix_permissions: all` with a list that omits `project.gitops.merge`.
4. Keep `project.gitops.push`, which is what opens a change request.
5. On `kortix_version: 3`, set `project_repository: true` under the `workspace` block of every agent that reads or edits the repository, runs scripts from it, or opens change requests.
6. Run one pilot session. Confirm that its work arrives as a change request, not a commit on `main`.

Kortix does not block a force-push. A GitHub repository should carry a ruleset that blocks force-pushes and deletions on the default branch. A dashboard, API, SDK, or `--apply` edit to the manifest commits to the default branch at once. The guarantee is scoped to session work for that reason.

For the team-level view, see [open source AI agent platform for teams](https://opensourceaiagentplatform.com/open-source-ai-agent-platform-for-teams). It covers one repository shared by everyone, with the same agents and connectors for each person who clones it. The command surface and the full key reference are in the [Kortix docs](https://kortix.com/docs).
