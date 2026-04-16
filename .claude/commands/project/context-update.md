# Context Update (Walk stage)

Refresh `docs/.context/` files against current reality. At Walk stage, this is the **only** slash command you have — and it's the one you need most.

## When to Use

Run this when:
- You just finished a customer interview or research session (JTBD, ICP may need updates)
- You made a significant architectural choice (ARCHITECTURE-TAXONOMY may need a section)
- You discovered a new bug or tech-debt item (KNOWN_ISSUES)
- A new MCP server was added to the team's toolkit (MCP_SERVERS)
- You're onboarding someone new and they ask a question the docs can't answer
- A week has passed and nothing in `docs/.context/` has been touched (probably stale)

Example usage: `/project:context-update`

## Instructions

1. **Read everything in `docs/.context/`** to build a mental model of current recorded state.

2. **Compare against ground truth**:
   - `git log --oneline -20` — what has actually shipped recently?
   - `git branch -a` — what work is in flight?
   - Look at the most recently modified source files — do they reflect an architecture that matches `ARCHITECTURE-TAXONOMY.md`?
   - Check `README.md` and `CLAUDE.md` — do they contradict anything in `docs/.context/`?

3. **Flag gaps and staleness** — don't auto-rewrite without asking:
   - Placeholder text ("Stub file", "Replace this content") still present — flag which files
   - Claims in `JTBD.md` or `ICP.md` with no supporting evidence — flag for a follow-up interview
   - `KNOWN_ISSUES.md` items that were fixed but not removed — flag for cleanup
   - `ROADMAP.md` phases that have already completed — flag for update
   - `MCP_SERVERS.md` entries for servers the team no longer uses — flag deprecation
   - `ARCHITECTURE-TAXONOMY.md` layers that no longer match the code — flag mismatch

4. **Suggest specific updates** — present a diff-style proposal for each file that needs changes. Example:
   ```
   docs/.context/KNOWN_ISSUES.md
   - Remove "login fails on Safari 14" (fixed in commit abc123)
   + Add "rate limiter triggers on bulk imports > 500 rows" (observed in user feedback)
   ```

5. **Wait for user approval** before writing any changes. The user confirms each file individually.

6. **Apply approved updates** and report what changed:
   ```
   Context Updated (Walk stage):
   - JTBD.md: added evidence from 2 new interviews
   - KNOWN_ISSUES.md: removed 1 fixed bug, added 2 new observations
   - MCP_SERVERS.md: no changes
   - (other files): no changes
   ```

## What this command does NOT do

- **Does not** create PR docs folders — there is no PR workflow at Walk
- **Does not** run docs-gate CI — there is no CI at Walk
- **Does not** touch `ACTIVE_PRS.md` or `RECENT_DECISIONS.md` beyond noting they're placeholders — those are populated at the Walk stage
- **Does not** call any external API or MCP server — pure file reading and editing

## Why this is the only command at Walk

Walk is about making the project legible, not about running workflows. If you find yourself wanting `/start-pr`, `/review-pr`, or `/compound`, you're ready to graduate to [`ai-native-dev-scaffold-run`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-run) (Walk) or [`ai-native-dev-scaffold-sprint`](https://github.com/Epistemic-Me/ai-native-dev-scaffold-sprint) (Run).

## References

- See `CLAUDE.md` → "When to Graduate to Walk" for the exit criteria from Walk
- See `docs/.context/MCP_SERVERS.md` for the tool registry this command should treat as the source of truth
