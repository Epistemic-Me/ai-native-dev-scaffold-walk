# Claude Code Project Context

This is the **Walk stage** of the Walk → Run → Sprint AI-native maturity model. It's the operational baseline: context foundation + 5-stage PR lifecycle + docs-gate CI. If your team isn't walking yet, you have problems — this scaffold gets you there.

**Tool-agnostic**: works with GitHub Copilot CLI, Copilot Cloud Agents, Claude Code, Cursor, or any AI tool that reads markdown command files. Context is in the files, not the harness.

## You Are Here: Walk / Run / Sprint

- **Walk** *(← you are here)* — Context foundation + 5-stage PR lifecycle + docs-gate CI. Baseline operational capability.
- **Run** — Walk + authoritative per-feature specs + product slop filter + compound learning from your own PR history. Graduate to [`ai-native-dev-scaffold-run`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-run) when your feature specs are scattered across Jira tickets and SMEs are the only way to answer "how should this work?"
- **Sprint** — Run + digital twins of external stakeholders + `/stakeholder-alignment` + `/compound` + `/process-transcript` + self-model API. Graduate to [`ai-native-dev-scaffold-sprint`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-sprint) when you want customer voice scored into every PR.

**Walk is the baseline, not an aspiration.** 95% of enterprise AI pilots stall because teams jump to workflows without first making their project legible (MIT NANDA, 2025). Walk is 1–3 weeks of investment that unblocks everything downstream.

## The 5-Stage PR Lifecycle

```
/project:start-pr → develop → /project:review-pr → /project:check-pr → /project:close-pr
```

1. **`/project:start-pr {num} {slug}`** — Create branch + 3 docs scaffold (RESEARCH / TEST-STRATEGY / IMPLEMENTATION-PLAN)
2. **develop** — Iterate with AI on the 3 artifacts before writing code, then implement against IMPLEMENTATION-PLAN
3. **`/project:review-pr {num}`** — AI code review writes REVIEW.md with verdict + per-AC coverage table
4. **`/project:check-pr {num}`** — Run docs-gate locally (same checks as CI)
5. **`/project:close-pr {num}`** — Merge + archive docs to `_archive/` + update `docs/.context/ACTIVE_PRS.md`

Plus:
- **`/project:decision {slug}`** — Create an ADR for any cross-team, architectural, or irreversible choice
- **`/project:context-update`** — Refresh `docs/.context/*` against current repo state
- **`/project:context-status`** — Staleness audit across context docs

## What Claude Code reads at session start

This file (`CLAUDE.md`) plus everything in `docs/.context/`:

- `JTBD.md` — jobs to be done, with evidence per claim
- `ICP.md` — ideal customer profile
- `ARCHITECTURE-TAXONOMY.md` — system layers, boundaries, ownership
- `KNOWN_ISSUES.md` — active bugs + tech debt
- `ROADMAP.md` — phase-level plan
- `RELATED_REPOS.md` — cross-repo context
- `MCP_SERVERS.md` — which MCP servers this repo uses, why, credential paths
- `ACTIVE_PRS.md` — populated by `/start-pr` and `/close-pr`
- `RECENT_DECISIONS.md` — populated by `/decision`

## The 4 Context Primitives

Every AI interaction runs on four types of context. Walk installs the repo-level versions of the first three:

1. **Instructions** — `CLAUDE.md` tells the agent how to behave in this specific project
2. **Knowledge** — `docs/.context/` gives the agent the domain, users, architecture, and open work
3. **Tool feedback** — `MCP_SERVERS.md` declares which tools the agent can use and what each returns
4. **Conversation history** — handled by the runtime; nothing to install at Walk

(See Simon Willison's 2025 definition: https://simonwillison.net/2025/jun/27/context-engineering/)

## MANDATORY: PR Workflow Rules

**NEVER commit directly to master/main.** All code changes MUST follow PR workflow.

1. Check current branch: `git branch --show-current`
2. If on master/main → **STOP** and create feature branch via `/project:start-pr`
3. Never merge without human approval
4. docs-gate CI blocks merge on missing RESEARCH / TEST-STRATEGY / IMPLEMENTATION-PLAN

## Credential Policy (Walk-stage foundation)

Tool access and secrets are **context that must be declared**, not tribal knowledge.

1. **No secrets in the repo.** `.env` files are gitignored. `.env.example` documents variables without values.
2. **Vault is the source of truth.** Every credential has a canonical path like `vault://engineering/{service}`. `MCP_SERVERS.md` references vault paths, not secrets.
3. **Service accounts, not personal credentials.** AI agents run as a dedicated identity; blast radius is bounded.
4. **Least-privilege per task.** Credentials loaded for a task grant access only to what that task needs.
5. **Short-lived where possible.** Prefer dynamic, expiring credentials over long-lived static keys.

See `docs/.context/MCP_SERVERS.md` for per-server credential paths and the Context Declaration template.

## Command Curator Role

Commands in `.claude/commands/project/` are **treated as code**:

- Every command change goes through a PR
- The **Command Curator** (a designated senior engineer or Platform team member) reviews command PRs
- New engineers discover commands via `ls .claude/commands/project/` and the README table
- Deprecated commands are moved to `.claude/commands/_deprecated/` with a reason and end-of-life date
- Private commands in `~/.claude/` are anti-pattern — if a sprinter needs a new command, they PR it here first

This prevents the "tribal knowledge" problem where sprinters create private commands that walkers never find.

## When to Graduate to Run

You're ready for `ai-native-dev-scaffold-run` (Run) when:

- Walk has been the baseline for 2+ sprints; every PR is going through the 5-stage lifecycle
- You're starting to feel the pain of scattered feature specs — bugs are hard to triage because the authoritative "how should this work?" lives in SME heads
- You want AI to auto-triage support tickets against feature specs
- You want a product slop filter that rejects features before they hit the roadmap
- You want compound learning from your own PR history ("we saw this pattern on PR #297")

If you're not there yet, invest another sprint in Walk. Get more teams onto it first.

## Best Practices

- **Fill RESEARCH.md before coding** — understand the problem first
- **Define exit criteria in TEST-STRATEGY.md before planning** — know what "done" looks like
- **Declare context up-front** — every ticket has a Context Declaration block referencing `docs/.context/` files and MCP servers
- **Update docs when scope changes** — docs lead, code follows
- **Record significant decisions as ADRs** — especially ones future devs will question
- **Run `/project:context-update` regularly** — keep the project brain current
- **Keep CLAUDE.md under 300 lines** (per Anthropic) — human-curated, treated like code

## References

- Anthropic — [Claude Code best practices](https://code.claude.com/docs/en/best-practices)
- Karpathy — [context engineering definition](https://x.com/karpathy/status/1937902205765607626)
- Simon Willison — [Context Engineering](https://simonwillison.net/2025/jun/27/context-engineering/)
- MIT NANDA — [GenAI Divide 2025](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/)
- GitHub — [Continuous AI / Agentic Workflows](https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/)
