# AI-Native Development Scaffold — Crawl Stage

> Context Foundation. The project is legible to both humans and agents. No PR workflow yet.

This scaffold is the **Crawl** stage of a Crawl → Walk → Run maturity staircase. Its entire job is to make a project understandable to AI agents and new engineers in under an hour — before you attempt any PR lifecycle or compounding loop.

```
Crawl foundation:
  CLAUDE.md                                 (under 300 lines, human-curated)
  docs/.context/
    ├── JTBD.md                             (jobs to be done, evidence-backed)
    ├── ICP.md                              (ideal customer profile)
    ├── ARCHITECTURE-TAXONOMY.md            (system layers + boundaries)
    ├── KNOWN_ISSUES.md                     (active bugs + tech debt)
    ├── ROADMAP.md                          (phase-level plan)
    ├── RELATED_REPOS.md                    (cross-repo context)
    ├── MCP_SERVERS.md                      (tool registry)
    ├── ACTIVE_PRS.md                       (placeholder — populated by Walk)
    └── RECENT_DECISIONS.md                 (placeholder — populated by /decision at Walk)
  .claude/commands/project/
    └── context-update.md                   (the one agent command you need at Crawl)
```

**Deliberately absent**: `/start-pr`, `/review-pr`, docs-gate CI, ADR index. The Crawl stage proves the context foundation is independent of any workflow. When you're ready for the paper trail + gate, graduate to the Walk scaffold.

## You Are Here: Crawl / Walk / Run

AI-native development is a three-stage maturity staircase. Each stage has to be solid before the next is meaningful.

| Stage | What it is | Scaffold repo |
|---|---|---|
| **Crawl** *(← you are here)* | Context Foundation: `CLAUDE.md`, `docs/.context/` core set, `MCP_SERVERS.md`, credential policy. No PR workflow. | `ai-native-dev-scaffold-crawl` (this repo) |
| **Walk** | Paper Trail + Gate: Crawl + 5-stage PR lifecycle (`/start-pr → develop → /review-pr → /check-pr → /close-pr`) + docs-gate CI + ADR index. | [`ai-native-dev-scaffold`](https://github.com/Epistemic-Me/ai-native-dev-scaffold) |
| **Run** | Compounding Intelligence: Walk + `/stakeholder-alignment` + `/compound` + `/process-transcript` + self-model API integration. | [`ai-native-dev-scaffold-compound`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-compound) |

## Why Crawl Matters

GitHub's own "Continuous AI" guidance uses the same crawl → walk → run progression for agentic workflows. MIT NANDA's 2025 "GenAI Divide" report found **95% of enterprise AI pilots** stall with no measurable P&L impact — the root cause is the "learning gap" (missing organizational context), not model quality.

Teams jump straight to `/start-pr`-style workflows or compound loops without first making their project legible. The agent starts from zero every session. Secrets and MCP servers are wired up tribally. Every engineer reinvents the setup. That's the 95%.

Crawl is the cheap investment (1–3 weeks) that unblocks everything downstream.

## Quick Start

```bash
# 1. Clone and reinitialize for your project
git clone https://github.com/Epistemic-Me/ai-native-dev-scaffold-crawl.git my-project
cd my-project
rm -rf .git && git init && git add -A && git commit -m "init: crawl scaffold"

# 2. Fill in your project's context docs (start with stubs, iterate with AI)
#    - CLAUDE.md           → project instructions for Claude Code
#    - docs/.context/*.md  → the project brain
#    - docs/.context/MCP_SERVERS.md → which MCP servers this repo uses

# 3. Ask an AI agent to cold-read your repo and tell you what's missing
claude "Read CLAUDE.md and docs/.context/. Tell me what a new engineer still couldn't figure out in under an hour."

# 4. When all core docs are populated and MCP_SERVERS.md is declared, graduate to:
#    https://github.com/Epistemic-Me/ai-native-dev-scaffold
```

## What's in this scaffold

### `CLAUDE.md`
Root-level project instructions. Under 300 lines (per Anthropic best practices). Includes credential policy.

### `docs/.context/` — the project brain
Every file Claude Code reads at session start:

| File | Purpose |
|---|---|
| `JTBD.md` | Jobs to be done, with evidence strength per claim |
| `ICP.md` | Ideal customer profile with pain points |
| `ARCHITECTURE-TAXONOMY.md` | System layers, boundaries, what's Engine vs Platform vs App |
| `KNOWN_ISSUES.md` | Active bugs, tech debt, acknowledged gaps |
| `ROADMAP.md` | Phase-level plan (not sprint-level) |
| `RELATED_REPOS.md` | Cross-repo context — which sibling repos exist and what they own |
| `MCP_SERVERS.md` | Standalone tool registry — which MCP servers, why, which tickets need each, credential paths |
| `ACTIVE_PRS.md` | Placeholder (populated automatically at Walk stage) |
| `RECENT_DECISIONS.md` | Placeholder (populated by `/project:decision` at Walk stage) |

### `.claude/commands/project/context-update.md`
The one agent command you get at Crawl. It inspects `docs/.context/*` and flags stale content. Everything else (`/start-pr`, ADRs, `/compound`) comes at higher stages.

## Philosophy

> "AI-native is not AI-augmented. The difference is explicit context engineering. Without it you get autocomplete with extra steps."

Context engineering is the discipline of filling the context window with just the right information for the next step (Karpathy, 2025). The 4 primitives — instructions, retrieved knowledge, tool feedback, conversation history — are what every AI interaction runs on. This scaffold installs the repo-level versions of the first two.

## Not included (on purpose)

- `.github/workflows/pr-docs-gate.yml` — docs-gate CI is a Walk-stage artifact
- `.claude/commands/project/{start-pr,review-pr,check-pr,close-pr}.md` — workflow commands are Walk-stage
- `scripts/pr_docs_check.py` — Walk's validation logic
- `docs/prs/` folder structure — no PR workflow at Crawl
- `docs/decisions/` (ADRs) — shows up at Walk once you start tracking decisions
- `/stakeholder-alignment`, `/compound`, `/process-transcript` — Run-stage compounding loop

If you want those, you're ready for `ai-native-dev-scaffold` (Walk) or `ai-native-dev-scaffold-compound` (Run).

## References

- **Anthropic — Claude Code best practices**: CLAUDE.md should be under 300 lines, human-curated, treated like code. https://code.claude.com/docs/en/best-practices
- **Karpathy — context engineering definition** (June 2025): https://x.com/karpathy/status/1937902205765607626
- **Simon Willison — Context Engineering** (June 2025): https://simonwillison.net/2025/jun/27/context-engineering/
- **MIT NANDA — GenAI Divide 2025** (95% enterprise AI pilot failure): https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/
- **GitHub — Continuous AI / Agentic Workflows** (uses crawl→walk→run framing): https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/
- **Astrix Security — State of MCP Server Security 2025**: https://astrix.security/learn/blog/state-of-mcp-server-security-2025/

## License

MIT
