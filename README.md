# Learn Codex

A practical playbook for using Codex and ChatGPT Work as an agentic workspace: bringing in better context, organizing persistent work, building reusable skills, connecting tools, automating bounded workflows, and evaluating reliability.

> **Core loop:** bring context in → define a responsibility → produce a reviewable artifact → verify it → package repeated work → automate only after the workflow is trustworthy.

## 1. Think beyond a coding assistant

Codex can be useful for project coordination, messages, research, documents, websites, feedback monitoring, artifacts, and code. A useful mental model is:

1. Give the agent enough context to understand the responsibility.
2. Give it only the tools needed for that responsibility.
3. Define what successful completion looks like.
4. Make outputs easy for a human to inspect.
5. Preserve important state in durable notes.
6. Package proven procedures into skills or integrations.

## 2. Context quality drives output quality

Voice is useful not only because it is fast, but because people naturally provide more task-relevant context while speaking. A reusable briefing pattern:

```text
Outcome: What should exist when the task is complete?
Context: Why is it needed, and who will use it?
Sources: Which repositories, files, apps, or conversations should be consulted?
Constraints: What must remain unchanged or out of scope?
Verification: What evidence demonstrates correctness?
Authority: What may the agent change, and what requires approval?
```

Add information that changes the task: deadlines, dependencies, source locations, recipients, expected formats, and permission boundaries.

## 3. Treat persistent threads as responsibilities

Instead of starting a new thread for every question, keep a persistent thread for a coherent responsibility such as project status, documentation, feedback monitoring, a launch, or a chief-of-staff brief.

A simple contract:

```yaml
name: project-status
purpose: Produce a source-backed status brief for one project.
sources:
  - selected GitHub repository
  - selected project notes
allowed_actions:
  - read authorized sources
  - create draft reports
approval_required:
  - sending messages
  - merging pull requests
  - production changes
output:
  - summary
  - changes
  - blockers
  - decisions needed
  - source links
```

The “virtual employee” metaphor can be useful organizationally, but it should not replace permissions, evidence, or human accountability.

## 4. Start with a chief-of-staff brief

A high-value first workflow is a recurring brief combining authorized sources into a prioritized view.

```text
Review the authorized sources for changes since the previous brief.
Return:
1. Items requiring my decision.
2. Important changes.
3. Blockers and dependencies.
4. Messages that may need a reply, with drafts only.
5. Today's commitments.
6. Source links for factual claims.

Do not send messages, change calendar events, merge code, or publish anything.
```

Once the brief is consistently useful, schedule it. Only then consider narrowly defined write actions.

## 5. Keep an inspectable memory vault

Use durable notes such as `AGENTS.md`, todos, people notes, project notes, and agent notes. Keep facts that must survive future runs outside ephemeral conversation context.

```text
memory/
├── AGENTS.md
├── projects/
├── people/
├── decisions/
└── runs/
```

Project notes should record goals, current state, decisions, blockers, source links, and a verification date. Compaction can help long-running work, but durable notes remain inspectable, editable, and sourceable.

## 6. Turn repeated work into skills

Good candidates include source-backed briefs, issue triage, PR review preparation, release-note drafting, documentation audits, feedback classification, and artifact generation.

A skill should encode the procedure and quality bar, not hide unrestricted authority:

```text
1. Identify authorized sources.
2. Retrieve only relevant information.
3. Separate verified facts from inference.
4. Link material claims to evidence.
5. Identify blockers and decisions.
6. Draft next actions.
7. Never send, merge, publish, or delete without authorization.
```

Plugins and MCP tools expose external capabilities. Installation alone does not prove a particular read/write operation is available; inspect the actual operations and permissions.

## 7. GitHub workflow: research → change → verify → review

For engineering work, define a bounded loop rather than simply saying “fix it.”

```text
Goal: Resolve the selected issue.

1. Read the issue and repository instructions.
2. Inspect implementation and tests before editing.
3. State a short plan.
4. Make the smallest coherent change.
5. Run relevant tests and checks.
6. Review the diff for unrelated changes.
7. Summarize changes and uncertainty.
8. Commit with a descriptive message.
9. Create a PR only when requested.
```

For bugs, require reproduction evidence and report the exact checks run. Never claim success when a required check could not run.

## 8. Automate only after the manual loop works

For recurring agent workflows, use a gradual progression:

```text
manual read-only
→ scheduled read-only
→ scheduled draft creation
→ bounded reversible writes
→ narrowly scoped autonomous actions
```

For recurring work, record sources checked, changes detected, actions proposed/performed, approvals, checks, failures, and links or commit SHAs.

Manager threads should have limits: maximum child tasks, one issue per child, explicit source/write scopes, no recursive spawning by default, automatic closure, and escalation when evidence conflicts.

## 9. Optimize cost to the job

The strongest model at maximum effort is unnecessary for every recurring check. Control cost with narrower source sets, lower cadence, cheaper monitoring/classification, and expensive reasoning only after meaningful changes are detected.

```text
cheap monitor
  ↓ meaningful change?
no → stop
 yes
  ↓
focused retrieval
  ↓
higher-reasoning analysis
  ↓
draft artifact
  ↓
human review / bounded action
```

Measure the real workflow and adjust model choice, cadence, and retrieval depth based on results.

## 10. Turn failures into evaluations

When a workflow fails:

1. Save the input and relevant context.
2. Identify the real failure mode.
3. Remove irrelevant noise while preserving difficulty.
4. Add the case to a regression set.
5. Test configuration/model changes against the same set.

Evaluate retrieval, grounding, tool choice, permission handling, completeness, efficiency, and recovery from missing/conflicting data. If every model scores 100%, the evaluation is probably too easy.

## 11. Permission ladder

| Level | Agent may do | Examples |
| --- | --- | --- |
| 0 | Read | inspect repo, messages, documents |
| 1 | Draft | brief, reply, patch, plan |
| 2 | Reversible workspace changes | branch, draft PR, local file |
| 3 | External reversible action | low-risk message, selected record update |
| 4 | High-impact action | merge, delete, deploy, purchase, publish |

Start at the lowest useful level. For bulk operations, preview the number of affected items, selection criteria, sample changes, ambiguous records, and rollback plan before execution.

## 12. Prompt library

### Project status

```text
Act as the status owner for this project. Use only authorized repository and project notes. Summarize changes since the previous run, blockers, decisions needed, and the next three useful actions. Link factual claims to sources. Do not modify external systems.
```

### GitHub issue triage

```text
Read the issue, linked discussion, repository instructions, and relevant code. Classify it, identify affected components, list missing reproduction information, and draft a triage response. Do not post it.
```

### Documentation audit

```text
Compare documentation against the current implementation. Find stale commands, renamed features, broken references, and unsupported claims. Return findings with source locations and proposed edits before changing files.
```

### Recurring monitor

```text
Check the authorized source for meaningful changes since the previous run. If nothing changed, record that and stop. If something changed, summarize evidence, impact, and a proposed next action. Do not take external action unless explicitly authorized.
```

## 13. First-week adoption plan

**Day 1:** Brief one real task with outcome, context, sources, constraints, verification, and authority.

**Day 2:** Create one persistent responsibility and give it a task contract.

**Day 3:** Add `AGENTS.md` and one durable project note.

**Day 4:** Produce a reviewable artifact instead of relying on raw chat history.

**Day 5:** Turn one repeated procedure into a skill or documented workflow.

**Day 6:** Save at least five difficult evaluation cases.

**Day 7:** Schedule one read-only or draft-only workflow and review its first runs before increasing authority.

## 14. Official references

- OpenAI — ChatGPT Work: https://learn.chatgpt.com/docs/get-started-with-work
- OpenAI — Appshots: https://learn.chatgpt.com/docs/appshots
- OpenAI — Voice: https://learn.chatgpt.com/docs/features/voice
- OpenAI — Scheduled tasks: https://learn.chatgpt.com/docs/automations
- OpenAI — Long-running work: https://learn.chatgpt.com/docs/long-running-work
- OpenAI — AGENTS.md: https://learn.chatgpt.com/docs/agent-configuration/agents-md
- OpenAI — Skills: https://learn.chatgpt.com/docs/build-skills
- OpenAI — Plugins: https://learn.chatgpt.com/docs/plugins
- OpenAI — GitHub: https://learn.chatgpt.com/docs/third-party/github
- OpenAI — Agent approvals & security: https://learn.chatgpt.com/docs/agent-approvals-security
- OpenAI — Computer Use: https://learn.chatgpt.com/docs/computer-use
- OpenAI — Codex Remote: https://learn.chatgpt.com/docs/remote
