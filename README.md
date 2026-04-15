# AI-Native Development Scaffold — Walk Stage

> **You must be here or you have problems.** Context foundation + 5-stage PR lifecycle + docs-gate CI. The operational baseline for any AI-native engineering team.

```
/start-pr → develop → /review-pr → /check-pr → /close-pr
```

This scaffold is the **Walk** stage of a Walk → Run → Sprint AI-native maturity staircase. It gives your team an enforceable PR paper trail, CI-blocking docs-gate, and AI-ready context files — in a working repo you clone and start using on Monday.

**Tool-agnostic by design.** Works with GitHub Copilot CLI, Copilot Cloud Agents, Claude Code, or any AI tool that can read markdown command files. The context is in the `docs/.context/` and `.claude/commands/` files — the harness is your choice.

## You Are Here: Walk / Run / Sprint

| Stage | What it is | Scaffold repo |
|---|---|---|
| **Walk** *(← you are here)* | Context Foundation + 5-stage PR lifecycle + docs-gate CI. The operational baseline. Designed for walkers, not sprinters. | `ai-native-dev-scaffold-walk` (this repo) |
| **Run** | Walk + authoritative per-feature specs + product slop filter + compound learning from your own PR history. Mature engineering org with institutional memory. | [`ai-native-dev-scaffold-run`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-run) |
| **Sprint** | Run + digital twins of external stakeholders + `/stakeholder-alignment` + `/compound` + `/process-transcript` + self-model API. AI-native with the customer voice baked into every PR. | [`ai-native-dev-scaffold-sprint`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-sprint) |

**Don't skip Walk.** 95% of enterprise AI pilots stall because teams jump to workflows without first making their project legible (MIT NANDA, 2025). Walk is the cheap investment (1–3 weeks) that unblocks everything downstream.

## Quick Start (Walker Week 1)

```bash
# 1. Clone and reinitialize
git clone https://github.com/Epistemic-Me/ai-native-dev-scaffold-walk.git my-project
cd my-project
rm -rf .git && git init && git add -A && git commit -m "init: walk scaffold"

# 2. Fill in your project's context docs
#    docs/.context/*.md — the project brain
#    docs/.context/MCP_SERVERS.md — which MCP servers this repo uses
#    CLAUDE.md — project instructions (under 300 lines)

# 3. Start your first PR
/project:start-pr 001 my-first-feature
#    → Creates branch + docs scaffold (RESEARCH.md / TEST-STRATEGY.md / IMPLEMENTATION-PLAN.md)

# 4. Iterate with AI on the docs BEFORE you write code
#    → This is where 60-70% of your thinking happens

# 5. Execute, review, gate, close
/project:review-pr 001   # AI code review + verdict
/project:check-pr 001    # Run docs-gate locally
/project:close-pr 001    # Merge + archive + update ACTIVE_PRS.md
```

See `WEEK-1-QUICKSTART.md` for the one-page walker onboarding guide.

## What's in this scaffold

### `.claude/commands/project/` — the command library
The Walk-stage command set, treated as code (versioned, reviewed, curated):

- **`/start-pr`** — Create branch + 3 docs (research / test-strategy / implementation plan)
- **`/execute-pr`** — Implement from the plan with progress tracking (optional; you can just work normally)
- **`/review-pr`** — AI code review + verdict written to REVIEW.md
- **`/check-pr`** — Run docs-gate locally (same checks as CI)
- **`/close-pr`** — Merge + archive docs + update `ACTIVE_PRS.md`
- **`/decision`** — Create an Architecture Decision Record
- **`/context-update`** — Refresh `docs/.context/*` against current repo state
- **`/context-status`** — Staleness audit across context docs

### `docs/.context/` — the project brain (AI reads at session start)

| File | Purpose |
|---|---|
| `JTBD.md` | Jobs to be done, with evidence per claim |
| `ICP.md` | Ideal customer profile with pain points |
| `ARCHITECTURE-TAXONOMY.md` | System layers, boundaries, ownership |
| `KNOWN_ISSUES.md` | Active bugs + tech debt |
| `ROADMAP.md` | Phase-level plan |
| `RELATED_REPOS.md` | Cross-repo context |
| `MCP_SERVERS.md` | Standalone tool registry — which MCP servers, why, credential paths |
| `ACTIVE_PRS.md` | Populated by `/start-pr` + `/close-pr` |
| `RECENT_DECISIONS.md` | Populated by `/decision` |

### `.github/workflows/pr-docs-gate.yml` — the CI gate
Blocks merge if RESEARCH.md, TEST-STRATEGY.md, or IMPLEMENTATION-PLAN.md is missing from the PR's `docs/prs/{folder}/`. Walker-proof enforcement.

### `docs/prs/` — the paper trail
Per-PR folders with RESEARCH / TEST-STRATEGY / IMPLEMENTATION-PLAN / IMPLEMENTATION / REVIEW. Archived to `_archive/` after merge. Becomes your institutional memory.

### `scripts/pr_docs_check.py` — the gate validator
Walk-level checks (blocking): PR folder exists, RESEARCH.md ≥50 bytes, TEST-STRATEGY.md ≥50 bytes, IMPLEMENTATION-PLAN.md ≥50 bytes. Extensible.

## Why Walk Matters

GitHub's own "Continuous AI" guidance uses the same walk → run → sprint progression for agentic workflows. MIT NANDA's 2025 "GenAI Divide" report found **95% of enterprise AI pilots** stall because of the "learning gap" — missing organizational context, not model quality.

The Walk stage is the operational baseline that makes everything else possible:
- Context files are standardized (not tribal)
- Commands are curated (not invented by each sprinter privately)
- Every PR has an enforceable paper trail (not "read the diff and trust")
- AI sessions start with full project context, deterministically (not "catch Claude up first")

## GitHub Copilot Compatibility

This scaffold is **tool-agnostic**. Because commands are markdown files with explicit context, the AI harness matters less than model inference:

- **GitHub Copilot CLI**: read commands as prompt templates; invoke via `copilot suggest`
- **Copilot Cloud Agents**: trigger asynchronous review from Jira / GitHub Issues
- **Claude Code CLI**: native slash command execution
- **Cursor / Windsurf / Codex**: read commands from `.claude/commands/project/`

The context engineering foundation is the point, not the tool. Pick whichever fits your org's budget and adoption curve.

## Design Principle: Walker Floor, Not Sprinter Ceiling

80-90% of your engineers are walkers — they want AI to remove busywork, not to become AI researchers. This scaffold is designed for them. The Walk stage is boring on purpose. The interesting stuff (digital twins, compound loops, customer alignment) is at the Sprint stage, and you get there by walking first.

If you find yourself creating private commands that other engineers don't know about, you're creating tribal knowledge. Treat commands like code: PR them into this scaffold, document them, let the team benefit.

## References

- **Anthropic — Claude Code best practices**: CLAUDE.md <300 lines, human-curated. https://code.claude.com/docs/en/best-practices
- **Karpathy — context engineering definition** (June 2025): https://x.com/karpathy/status/1937902205765607626
- **Simon Willison — Context Engineering** (June 2025): https://simonwillison.net/2025/jun/27/context-engineering/
- **MIT NANDA — GenAI Divide 2025** (95% enterprise AI pilot failure): https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/
- **GitHub — Continuous AI / Agentic Workflows**: https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/
- **Astrix Security — State of MCP Server Security 2025**: https://astrix.security/learn/blog/state-of-mcp-server-security-2025/

## License

MIT
