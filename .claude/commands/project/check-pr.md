# Check PR

Run the PR documentation gate locally to catch issues before CI.

## Arguments

The user may optionally provide:
- PR number (auto-detected from current branch if omitted)

Example usage:
- `/check-pr` — auto-detect PR from current branch
- `/check-pr 181` — check specific PR

## Instructions

1. **Determine the PR number**:
   - If the user provided a number, use it
   - Otherwise, detect from the current branch:
     ```bash
     gh pr view --json number --jq '.number' 2>/dev/null
     ```
   - If no PR exists for the current branch, tell the user and stop

2. **Run the docs-gate check locally**:
   ```bash
   python scripts/pr_docs_check.py <PR_NUMBER>
   ```

3. **Interpret the result**:
   - **Exit 0**: All checks passed — report success
   - **Exit 2**: Small PR exemption — report exempted
   - **Exit 1**: Checks failed — read `/tmp/pr-docs-summary.md` and report failures

4. **For each failure, suggest the fix**:

   | Missing Item | Suggested Action |
   |---|---|
   | Docs folder not found | Run `/start-pr` to create it, or check that branch name matches folder |
   | RESEARCH.md | Create via `/start-pr` or write manually |
   | IMPLEMENTATION-PLAN.md | Create via `/start-pr` or write manually |
   | TEST-STRATEGY.md | Create via `/start-pr` or write manually |
   | REVIEW.md | Run `/review-pr` to generate it |
   | STAKEHOLDER-ALIGNMENT.md | Run `/stakeholder-alignment` to generate it |
   | Review verdict not Approve | Run `/review-pr` to re-review |
   | No aggregate alignment score | Run `/stakeholder-alignment` to generate scores |
   | Uncovered acceptance criteria | Add tests to TEST-STRATEGY.md test matrix covering the missing ACs |
   | Unchecked Definition of Done | Mark completed items as `[x]` in TEST-STRATEGY.md, or complete the remaining work |

5. **Report summary** with pass/fail status and any recommended next steps
