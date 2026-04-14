# Claude Code Project Context

This is the **Crawl stage** of the Crawl → Walk → Run AI-native maturity model. Its only job is to make this project legible to humans and agents. No PR workflow. No CI gate. No compounding loop. Just the context foundation.

## You Are Here: Crawl / Walk / Run

- **Crawl** *(← you are here)* — Context Foundation only. `CLAUDE.md` + `docs/.context/` + `MCP_SERVERS.md` + credential policy.
- **Walk** — Paper Trail + Gate. Graduate to [`ai-native-dev-scaffold`](https://github.com/Epistemic-Me/ai-native-dev-scaffold) when the context here is stable.
- **Run** — Compounding Intelligence. Graduate to [`ai-native-dev-scaffold-compound`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-compound) after 2 sprints of Walk maturity.

**Don't skip Crawl.** 95% of enterprise AI pilots stall because teams jump to workflows without first making their project legible (MIT NANDA, 2025). Crawl is cheap (1–3 weeks) and unblocks everything.

## What Claude Code reads at session start

This file (`CLAUDE.md`) plus everything in `docs/.context/`:

- `JTBD.md` — jobs to be done, with evidence
- `ICP.md` — ideal customer profile
- `ARCHITECTURE-TAXONOMY.md` — system layers and boundaries
- `KNOWN_ISSUES.md` — active bugs and tech debt
- `ROADMAP.md` — phase-level plan
- `RELATED_REPOS.md` — sibling repos and what they own
- `MCP_SERVERS.md` — which MCP servers this repo uses, why, and credential paths
- `ACTIVE_PRS.md` — placeholder (populated at Walk)
- `RECENT_DECISIONS.md` — placeholder (populated by `/decision` at Walk)

## The One Command

Crawl ships with exactly one slash command:

- **`/project:context-update`** — inspects `docs/.context/*` and flags stale content, missing sections, or inconsistencies with `CLAUDE.md`.

Everything else (`/start-pr`, `/review-pr`, `/check-pr`, `/close-pr`, `/decision`, `/compound`, etc.) lives at higher stages. If you find yourself reaching for them, you're ready to graduate to the Walk scaffold.

## Project Structure

```
your-project/
├── .claude/commands/project/
│   └── context-update.md          # The one Crawl-stage command
├── docs/.context/                  # The project brain (AI reads at session start)
│   ├── JTBD.md
│   ├── ICP.md
│   ├── ARCHITECTURE-TAXONOMY.md
│   ├── KNOWN_ISSUES.md
│   ├── ROADMAP.md
│   ├── RELATED_REPOS.md
│   ├── MCP_SERVERS.md
│   ├── ACTIVE_PRS.md
│   └── RECENT_DECISIONS.md
├── CLAUDE.md                       # This file — project instructions for Claude Code
└── README.md                       # Human-facing intro + "You Are Here" staircase
```

## The 4 Context Primitives

Every AI interaction runs on four types of context. Crawl installs the repo-level versions of the first two:

1. **Instructions** — `CLAUDE.md` tells the agent how to behave in this specific project
2. **Knowledge** — `docs/.context/` gives the agent everything it needs about the domain, users, architecture, and open work
3. **Tool feedback** — `MCP_SERVERS.md` declares which tools the agent can use and what each returns
4. **Conversation history** — handled automatically by Claude Code; nothing to install at Crawl

(See Simon Willison's 2025 definition: https://simonwillison.net/2025/jun/27/context-engineering/)

## Credential Policy (Crawl-stage foundation)

Tool access and secrets are **context that must be declared**, not tribal knowledge. Agents and new engineers read this policy before touching anything.

1. **No secrets in the repo.** `.env` files are gitignored. `.env.example` documents what variables exist without values. Secrets in commits are a blocking issue — rotate immediately and force-push a clean history.
2. **Vault is the source of truth.** Every credential has a canonical path like `vault://engineering/{service}`. `CLAUDE.md`, `docs/.context/MCP_SERVERS.md`, and ticket Context Declarations reference vault paths, never the secret values.
3. **Service accounts, not personal credentials.** AI agents run as a dedicated identity separate from human developers. If the agent goes off-script, its blast radius is bounded.
4. **Least-privilege per task.** Credentials loaded for a task grant access only to what that task needs. No blanket admin tokens for day-to-day work.
5. **Short-lived where possible.** Prefer dynamic, expiring credentials over long-lived static keys. The exact mechanism (Vault dynamic secrets, 1Password Unified Access, cloud IAM role assumption, etc.) is an org-level choice — this scaffold declares the policy, not the vendor.

See `docs/.context/MCP_SERVERS.md` for per-server credential paths and the Context Declaration template for ticket descriptions.

## When to Graduate to Walk

You're ready for `ai-native-dev-scaffold` (Walk) when:

- Every file in `docs/.context/` has real content (no placeholder text)
- A new engineer can open the repo and understand the project in under an hour
- An AI agent can answer questions about architecture, roadmap, and known issues by reading `docs/.context/` alone
- `MCP_SERVERS.md` declares every tool the team uses + credential paths for each
- You have a shared credential policy (not ad-hoc per-dev `.env` files)
- The team wants a paper trail and CI-enforced docs gate for PRs

If you're not there yet, invest another sprint in Crawl before graduating.

## References

- Anthropic — [Claude Code best practices](https://code.claude.com/docs/en/best-practices) (CLAUDE.md under 300 lines, human-curated)
- Karpathy — [context engineering definition](https://x.com/karpathy/status/1937902205765607626) (June 2025)
- Simon Willison — [Context Engineering](https://simonwillison.net/2025/jun/27/context-engineering/) (June 2025)
- MIT NANDA — [GenAI Divide 2025](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/) (95% enterprise AI pilot failure)
- GitHub — [Continuous AI / Agentic Workflows](https://github.blog/ai-and-ml/automate-repository-tasks-with-github-agentic-workflows/) (uses crawl→walk→run framing)
