# Context Status

Report on documentation health and identify gaps.

## Instructions

1. **Check `.context/` freshness**:
   - Read each file in `docs/.context/`
   - Check "Last updated" dates
   - Flag any file not updated in the last 7 days

2. **Check PR paper trails**:
   - Run `gh pr list --state open` or check git branches
   - For each open PR, verify `docs/prs/YYYY-MM-DD-PR-{num}-{slug}/` exists
   - List any PRs missing paper trails

3. **Check feature documentation**:
   - List all folders in `docs/features/`
   - For each feature, verify SPEC.md, IMPLEMENTATION.md, CHANGELOG.md exist
   - Flag any incomplete feature documentation

4. **Check decisions**:
   - Read `docs/decisions/_INDEX.md`
   - Verify all listed decisions have corresponding files
   - Check for any "Proposed" decisions that need resolution

5. **Check JTBD/ICP currency**:
   - Read `docs/.context/JTBD.md` and `docs/.context/ICP.md`
   - Report days since last update
   - Flag if > 30 days stale

## Output Format

```
## Documentation Health Report

### .context/ Files
- CURRENT_SPRINT.md: {status} (updated {X days ago})
- ACTIVE_PRS.md: {status}
- RECENT_DECISIONS.md: {status}
- KNOWN_ISSUES.md: {status}
- JTBD.md: {status} - {days since update}
- ICP.md: {status} - {days since update}

### PR Paper Trails
- {X} open PRs
- {Y} with paper trails
- {Z} missing paper trails:
  - PR #{num}: {title}

### Feature Documentation
- {X} features documented
- {Y} complete
- {Z} incomplete:
  - {feature}: missing {files}

### Decisions
- {X} total decisions
- {Y} accepted
- {Z} proposed (pending)

### Recommendations
1. {Action item}
2. {Action item}
```
