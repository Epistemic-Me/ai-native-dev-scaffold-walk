# Review PR

AI code review + docs-gate check + merge + close. This is the exit command — it handles everything from review through merge.

## Arguments

- PR number (e.g., "042")

Example: `/project:review-pr 042`

## What This Command Does

1. AI code review (generates REVIEW.md)
2. Docs-gate check (same as CI)
3. If approved: merge → archive → update indexes
4. If issues: report what to fix, do NOT merge

## Instructions

### 1. Find the PR

```bash
gh pr view --json number,title,state,headRefName
```

Find the PR docs folder in `docs/prs/implementing/` matching `*-PR-{num}-*`.

### 2. AI Code Review

**Spawn a sub-agent** (Agent tool) with a clean context window. The sub-agent:

a. Reads project context:
   - `CLAUDE.md`
   - `docs/.context/ARCHITECTURE-TAXONOMY.md` (if exists)
   - `docs/.context/KNOWN_ISSUES.md` (if exists)
   - `docs/.context/ROADMAP.md` (if exists)

b. Reads the PR docs:
   - `RESEARCH.md` — what problem is being solved
   - `TEST-STRATEGY.md` — what the acceptance criteria are
   - `IMPLEMENTATION-PLAN.md` — what the implementation approach was
   - `IMPLEMENTATION.md` — what was actually built

c. Gets the PR diff:
   ```bash
   gh pr diff {num}
   ```

d. Reviews against these dimensions:
   - **Correctness**: Does the code do what IMPLEMENTATION-PLAN.md says? Edge cases?
   - **AC Coverage**: Does every AC in TEST-STRATEGY.md have test coverage?
   - **Architecture**: Consistent with project patterns?
   - **Risk**: What could break? What's the blast radius?

e. Returns a structured review between `---BEGIN REVIEW---` and `---END REVIEW---` markers.

### 3. Write REVIEW.md

Take the sub-agent's output and write (or append) to `REVIEW.md` in the PR docs folder:

```markdown
## Review v{N}

**Verdict**: Approve | Request Changes
**Reviewed**: {date}

### Summary
{One paragraph on what the PR does and whether it achieves its goals}

### Per-AC Coverage
| AC | Description | Test Coverage | Status |
|----|-------------|---------------|--------|
| AC1 | ... | T1, T3 | PASS |

### Issues Found
**Critical**: {list or "None"}
**Suggestions**: {list or "None"}

### Risk Assessment
{What could break, blast radius}
```

If previous reviews exist, increment the version number. The last verdict in the file is the one that counts.

### 4. Docs-Gate Check

Run the docs-gate validation:

```bash
python scripts/pr_docs_check.py {num}
```

If the script doesn't exist, check manually:
- [ ] RESEARCH.md exists and has content (>50 bytes)
- [ ] TEST-STRATEGY.md exists and has content
- [ ] IMPLEMENTATION-PLAN.md exists and has content
- [ ] REVIEW.md exists (just created it)

Report pass/fail for each check.

### 5. Decision Point

**If Verdict = "Approve" AND gate passes:**

Tell the user:
```
Review: APPROVED
Docs gate: PASSED

Ready to merge PR #{num}?
```

If user confirms, proceed to merge.

**If Verdict = "Request Changes" OR gate fails:**

```
Review: {verdict}
Docs gate: {pass/fail}

Issues to fix:
- {issue 1}
- {issue 2}

Fix these and run /project:review-pr {num} again.
```

Stop here. Do NOT merge.

### 6. Merge & Close

```bash
# Merge the PR
gh pr merge {num} --squash --delete-branch

# Checkout main
git checkout main
git pull origin main
```

### 7. Archive docs

```bash
# Move from implementing to _archive
git mv docs/prs/implementing/{folder} docs/prs/_archive/{folder}
```

### 8. Update indexes

Update `docs/.context/ACTIVE_PRS.md`:
- Move PR from "Open PRs" to "Recently Merged"
- Add merge date

Check if architectural decisions were made (scan RESEARCH.md/IMPLEMENTATION-PLAN.md for decision language). If so, ask: "Create an ADR for this decision?"

### 9. Commit and push docs

```bash
git add -A
git commit -m "docs: close PR #{num} — {title}"
git push origin main
```

### 10. Report

```
PR #{num} MERGED AND CLOSED

GitHub PR: {url}
Verdict: Approved
Docs: Archived to _archive/

Updated:
- ACTIVE_PRS.md
- REVIEW.md (v{N})

{If ADR created: "ADR-{N} created: {title}"}
```
