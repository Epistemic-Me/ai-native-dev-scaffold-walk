# Close PR

Complete PR documentation and update related docs after merge.

## Arguments

The user should provide:
- PR number (e.g., "142")

Example usage: `/project:close-pr 142`

## Instructions

1. **Run the docs-gate check** before proceeding:
   ```bash
   python scripts/pr_docs_check.py <PR_NUMBER>
   ```
   - If it fails (exit code 1), report the failures and suggest fixes (see `/check-pr` for the fix table). **Stop here** — do not proceed with close until the gate passes.
   - If it passes (exit code 0) or is exempted (exit code 2), continue.

2. **Find the PR folder** in `docs/prs/` matching the PR number

3. **Update IMPLEMENTATION.md**:
   - Ensure "Summary" section is filled
   - Ensure "Key Changes" documents what was built
   - Ensure "Testing Done" has checked items
   - Ensure "Learnings" sections are filled
   - Set "PR Merged" date to today
   - Change "Status" from "In Progress" to "Complete"

4. **Review for completeness**:
   - RESEARCH.md should have problem statement and chosen option
   - PLAN.md should have scope and implementation order
   - IMPLEMENTATION.md should have summary and learnings
   - Flag any incomplete sections to the user

5. **Update ACTIVE_PRS.md**:
   - Move PR from "Open PRs" to "Recently Merged" table
   - Include merge date and paper trail link

6. **Update related feature CHANGELOG.md** (if applicable):
   - Ask user which feature this PR relates to
   - Add entry to that feature's CHANGELOG.md:
   ```markdown
   ## [{date}] - PR #{num}

   ### Added/Changed/Fixed
   - {Summary from IMPLEMENTATION.md}
   ```

7. **Check for decisions**:
   - Review RESEARCH.md and PLAN.md for significant decisions
   - If architectural decisions were made, ask user if ADR should be created
   - If yes, run the decision flow

8. **Update RECENT_DECISIONS.md** if decisions were captured

9. **Report summary** of all updates made
