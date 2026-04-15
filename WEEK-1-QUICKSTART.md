# Walker Week 1 Quickstart

> **One page. Five steps. Real value by Friday.**
>
> This guide is for the engineer who's never used `/start-pr`, who doesn't want to become an AI researcher, and who just wants to ship features without the busywork. If that's you, start here.

## The problem you have today

- Every PR starts with "let me catch Claude up on what this project is"
- You copy the same 3 paragraphs of context from your notes every sprint
- When you're blocked, you don't know who to ask, so you ping the senior dev who already has 12 other threads open
- The acceptance criteria in the Jira ticket are ambiguous, and by the time you notice, you've already written the wrong tests
- Your PR gets reviewed two days later and the reviewer asks "why did you do it this way?"

## What Week 1 fixes

By Friday, you'll have:
1. A project brain that Claude (or Copilot) reads automatically at the start of every session
2. A single command that creates your PR's research, test strategy, and plan before you write any code
3. A CI gate that blocks merge if those docs are missing — so you can't forget
4. A reviewer experience where the context is in the docs, not in a verbal catch-up

**Time investment**: ~4 hours spread across the week. Not a workshop. Just slot it into the work you'd do anyway.

---

## Monday: Clone the scaffold and fill in context (1 hour)

```bash
# Clone the Walk scaffold into a new project
git clone https://github.com/Epistemic-Me/ai-native-dev-scaffold-walk.git my-project
cd my-project
rm -rf .git && git init

# Open it in your editor with your AI tool (Copilot, Claude Code, Cursor, whatever)
```

Open `docs/.context/` and fill in the stub files. You don't need to write novels. Two paragraphs per file is enough to start.

**Priority order** (stop when you're out of the 1-hour budget):
1. `ARCHITECTURE-TAXONOMY.md` — what are the 3–5 main layers of this system? Engine/Platform/App? API/worker/DB? Just describe the shape.
2. `KNOWN_ISSUES.md` — what are the 3 bugs everyone knows about but nobody has time to fix?
3. `JTBD.md` — what is this project actually for? One-sentence answer is fine.
4. `ROADMAP.md` — what phase are you in? ("Launching MVP", "Scaling to 100 customers", etc.)
5. `MCP_SERVERS.md` — which MCP servers does your team use? Linear, GitHub, Playwright? Just list them. Don't worry about credential paths yet.

**Skip for now**: `ICP.md`, `RELATED_REPOS.md`, `RECENT_DECISIONS.md`, `ACTIVE_PRS.md`. These come later.

**Commit** everything. You're done Monday.

```bash
git add -A && git commit -m "init: walk scaffold with initial context"
```

## Tuesday: Start your first PR with `/start-pr` (30 minutes)

Pick any ticket you were going to work on this sprint. Doesn't matter which one. Use this as your first `/start-pr` experiment.

```bash
# In Claude Code / Copilot CLI / Cursor:
/project:start-pr 042 add-user-timezone-preference
```

This will:
1. Create a feature branch
2. Create `docs/prs/implementing/2026-04-14-PR-042-add-user-timezone-preference/` with 3 stub docs
3. Ask the AI to read your context docs + the ticket and draft RESEARCH.md

**Your job**: read the RESEARCH.md draft. It will probably be 80% right and 20% wrong. Fix the 20%. Push back on wrong assumptions. Iterate with the AI until you actually understand the problem.

This is the habit shift. You're doing your thinking in markdown, not in your head.

## Wednesday: Fill in TEST-STRATEGY.md *before* you write code (1 hour)

Open `docs/prs/implementing/2026-04-14-PR-042-.../TEST-STRATEGY.md`. The AI will have drafted an initial test strategy based on your research.

**Your job**: make sure the acceptance criteria table captures what "done" actually looks like. Every row should have a test ID, a test case, an AC it covers, and a pass criteria. If you can't write the pass criteria, you don't understand the requirement yet — go back to RESEARCH.md.

This is the moment the framework saves you time. When you realize "oh, the AC says X but the PM meant Y" — before you've written code — that's money.

Now fill in `IMPLEMENTATION-PLAN.md`. The AI will draft this too. You review the steps, reorder if needed, add things the AI missed.

## Thursday: Write the code (2 hours)

Now you write code. Follow the IMPLEMENTATION-PLAN.md step by step. Your AI tool has the full context from the context docs + the research + the test strategy + the plan. It will be dramatically better at generating code than it was Monday when it didn't know any of this.

As you code, update IMPLEMENTATION.md with what you actually did. It's a lightweight log — one bullet per step. Future-you will thank present-you when you come back to this PR in 6 months.

## Friday: Review, check, close (45 minutes)

```bash
# AI code review — reads your diff and the docs, produces a verdict
/project:review-pr 042

# Run the docs gate locally (same checks as CI will run)
/project:check-pr 042

# Merge + archive + update ACTIVE_PRS.md
/project:close-pr 042
```

You're done. Your PR shipped with:
- Research the reviewer can cold-read without asking you questions
- A test strategy that explicitly mapped tests to acceptance criteria
- An implementation plan that shows what you intended vs what you shipped
- An AI code review verdict
- A docs-gate CI check that passed

The reviewer didn't have to catch themselves up. The AI had full project context. The tests matched the requirement. Your future-self can understand why you did what you did.

---

## What happens in Week 2

- You run `/start-pr` again, and it's faster because the context docs are warm
- The second PR's reviewer is someone who wasn't in your planning conversation, and they can cold-read the docs folder and give useful feedback
- You start noticing you're catching bugs in research that you used to catch in QA
- You notice a command you wish existed — tell the Command Curator, they'll PR it into the scaffold

## If you get stuck

- **"The research doc is too long, Claude is rambling"** — Make the ticket smaller. A PR that produces a 500-line test strategy is a PR that should be 3 PRs.
- **"The AI doesn't know about [X thing in our codebase]"** — Add it to `docs/.context/ARCHITECTURE-TAXONOMY.md` or `KNOWN_ISSUES.md`. That's the whole point. The docs ARE the AI's memory.
- **"I want a command that [does Y]"** — Don't make a private command in `~/.claude/`. PR it into this scaffold. Tribal knowledge is the problem we're solving.
- **"My reviewer doesn't want to read the docs, they just want the diff"** — Share this quickstart with them. The docs make their life easier, not harder, once they get it.

---

## What you're NOT doing in Week 1

- Not learning about digital twins, stakeholder alignment, or the `/compound` loop. That's the Sprint stage (`ai-native-dev-scaffold-sprint`). You'll get there.
- Not writing authoritative per-feature specs in `docs/specs/`. That's the Run stage (`ai-native-dev-scaffold-run`). You get there after 2-3 sprints of Walk.
- Not building any new tools. You're using what the scaffold ships. If something doesn't fit, flag it — don't work around it silently.

---

## The one rule

**If it's not in the docs, it doesn't exist.** Not in the code, not in the decision tree, not in the AI's mental model, not in the reviewer's review. The docs are the project. The code is the implementation of the docs.

That's the habit shift. Once you've got it, you'll never go back.

---

*Questions? Feedback? File a PR against this quickstart in `ai-native-dev-scaffold-walk`. Your Week 1 experience is input for the next Week 1.*
