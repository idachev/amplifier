---
description: DDD Phase 5 - Cleanup and finalize (project:ddd)
argument-hint: [optional instructions]
allowed-tools: TodoWrite, Read, Write, Bash(git:*), Bash(make check:*), Bash(rm:*), Glob, Task
---

# DDD Phase 5: Wrap-Up & Cleanup

Loading context:

@docs/document_driven_development/phases/06_cleanup_and_push.md
@ai_working/ddd/

Instructions: $ARGUMENTS

---

## Your Task: Clean Up & Finalize

**Goal**: Remove temporary files, verify clean state, push/PR with explicit authorization

**Every git operation requires EXPLICIT user authorization**

---

## Phase 5 Steps

### Step 1: Cleanup DDD Working Files (Commit the Removal)

The `ai_working/ddd/` files have been tracked throughout development to document the AI-assisted process. Now we remove them with a final commit.

```bash
# Show what will be deleted (these were tracked in git)
ls -la ai_working/ddd/

# Ask user: "Remove DDD working files and commit the removal?"
# This creates a clean final state while preserving the development history
```

If YES:

```bash
# Remove the DDD working directory
rm -rf ai_working/ddd/

# Stage the removal for commit
git add -A ai_working/ddd/

# Commit the cleanup
git commit -m "chore: remove DDD working files after feature completion

The ai_working/ddd/ directory documented the AI-assisted development
process throughout this feature. The development history is preserved
in previous commits. Removing now that feature is complete.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

### Step 1b: Cleanup AI Temporary Files

Check for AI-generated temporary files in `ai_working/tmp/`:

```bash
# Check for AI temporary files
ls -la ai_working/tmp/ 2>/dev/null || echo "No ai_working/tmp/ directory"

# Ask user: "Clean up ai_working/tmp/ directory?"
# If yes:
rm -rf ai_working/tmp/
```

**Note**: Only clean up files in `ai_working/tmp/`. Do not touch project-specific directories like `./tmp/`, `.pytest_cache/`, `__pycache__/`, or other build artifacts.

### Step 2: Final Verification

Run complete quality check:

```bash
# For Node.js/TypeScript projects
pnpm lint

# For Python projects (or run both if mixed codebase)
make check
```

**Status**: ✅ All passing / ❌ Issues found

If issues found:

- List all issues clearly
- Ask user: "Fix these before finishing?"
- If yes, fix and re-run
- If no, note in summary

Check git status:

```bash
git status
```

**Questions to answer**:

- Are there uncommitted changes? (Should there be?)
- Are there untracked files? (Should they be added/ignored?)
- Is working tree clean? (Or remaining work?)

List all commits from this DDD session:

```bash
# Assuming session started after last push
git log --oneline origin/$(git branch --show-current)..HEAD

# Or since specific commit
git log --oneline [start-commit]..HEAD
```

**Show user**:

- Number of commits
- Summary of each commit
- Overall changes (insertions/deletions)

### Step 3: Commit Any Remaining Changes

Check for uncommitted changes:

```bash
git status --short
```

If changes exist:

**Ask user**: "There are uncommitted changes. Commit them?"

If YES:

- **Run linting first**:
  ```bash
  pnpm lint     # For Node.js/TypeScript projects
  # OR
  make check    # For Python projects
  ```
- Ensure linting passes before proceeding
- Show the diff
- Ask for commit message or suggest one
- Request explicit authorization
- Commit with provided/suggested message

If NO:

- Leave changes uncommitted
- Note in final summary

### Step 4: Push to Remote

**Ask user**: "Push to remote?"

Show context:

```bash
# Current branch
git branch --show-current

# Commits to push
git log --oneline origin/$(git branch --show-current)..HEAD

# Remote branch exists?
git ls-remote --heads origin $(git branch --show-current)
```

If YES:

- Confirm which remote and branch
- Push with: `git push -u origin [branch]`
- Show result

If NO:

- Leave local only
- Note in final summary

### Step 5: Create Pull Request (If Appropriate)

**Determine if PR is appropriate**:

- Are we on a feature branch? (not main/master)
- Has branch been pushed?
- Does user want a PR?

If appropriate, **ask user**: "Create pull request?"

Show context:

```bash
# Current branch vs main
git log --oneline main..HEAD

# Files changed
git diff --stat main..HEAD
```

If YES:

**Generate PR description** from DDD artifacts:

```markdown
## Summary

[From plan.md: Problem statement and solution]

## Changes

### Documentation

[List docs changed]

### Code

[List code changed]

### Tests

[List tests added]

## Testing

[From test_report.md: Key test scenarios]

## Verification Steps

[From test_report.md: Recommended smoke tests]

## Related

[Link to any related issues/discussions]
```

**Create PR** (using existing /commit command or gh pr create):

```bash
gh pr create --title "[Feature name]" --body "[generated description]"
```

Show PR URL to user.

If NO:

- Skip PR creation
- Note in final summary

### Step 6: Post-Cleanup Check

Consider spawning specialized cleanup agent:

```bash
Task post-task-cleanup: "Review workspace for any remaining
temporary files, test artifacts, or unnecessary complexity"
```

Final workspace verification:

```bash
# Working tree clean?
git status

# No untracked files that shouldn't be there?
git ls-files --others --exclude-standard

# Quality checks pass?
make check
```

### Step 7: Generate Final Summary

Create comprehensive completion summary:

````markdown
# DDD Workflow Complete! 🎉

## Feature: [Name from plan.md]

**Problem Solved**: [from plan.md]
**Solution Implemented**: [from plan.md]

## Changes Made

### Documentation (Phase 2)

- Files updated: [count]
- Key docs: [list 3-5 most important]
- Commit: [hash and message]

### Code (Phase 4)

- Files changed: [count]
- Implementation chunks: [count]
- Commits: [list all commit hashes and messages]

### Tests

- Unit tests added: [count]
- Integration tests added: [count]
- All tests passing: ✅ / ❌

## Quality Metrics

- Linting (`pnpm lint` / `make check`): ✅ Passing / ❌ Issues
- Code matches documentation: ✅ Yes
- Examples work: ✅ Yes
- User testing: ✅ Complete

## Git Summary

- Total commits: [count]
- Branch: [name]
- Pushed to remote: Yes / No
- Pull request: [URL] / Not created

## Artifacts Cleaned

- DDD working files: ✅ Removed
- Temporary files: ✅ Removed
- Debug code: ✅ Removed
- Test artifacts: ✅ Removed

## Recommended Next Steps for User

### Verification Steps

Please verify the following:

1. **Basic functionality**:
   ```bash
   [command]
   # Expected: [output]
   ```
````

2. **Edge cases**:

   ```bash
   [command]
   # Expected: [output]
   ```

3. **Integration**:
   ```bash
   [command]
   # Verify works with [existing features]
   ```

[List 3-5 key smoke tests from test_report.md]

### If Issues Found

If you find any issues:

1. Provide specific feedback
2. Re-run `/ddd:4-code` with feedback
3. Iterate until resolved
4. Re-run `/ddd:5-finish` when ready

## Follow-Up Items

[Any remaining TODOs or future considerations from plan.md]

## Workspace Status

- Working tree: Clean / [uncommitted changes]
- Branch: [name]
- Ready for: Next feature

---

**DDD workflow complete. Ready for next work!**

````

---

## Using TodoWrite

Track finalization tasks:

```markdown
- [ ] Temporary files cleaned
- [ ] Final verification passed
- [ ] Remaining changes committed (if any)
- [ ] Pushed to remote (if authorized)
- [ ] PR created (if authorized)
- [ ] Post-cleanup check complete
- [ ] Final summary generated
````

---

## Agent Orchestration

### Parallel vs Sequential Execution

**CRITICAL**: Send ONE message with MULTIPLE Task tool calls when tasks are independent.

#### When to Parallelize (No Dependencies)

- **Verification checks** - Multiple independent quality assessments
- **Cleanup scans** - Search for different types of artifacts simultaneously
- **Final reviews** - Philosophy check + code review + test verification

#### When to Keep Sequential (ALL git operations)

- **Every git operation** - Commits, pushes, PRs require user authorization
- **User approval checkpoints** - Wait for human decision
- **Cleanup execution** - After scans, execute cleanup sequentially

### Parallel Agent Examples

**Parallel Final Verification** (spawn in ONE message):
```
# In a SINGLE message, run multiple verification checks:

Task zen-architect: "Final review of implementation for philosophy compliance
- ruthless simplicity maintained?"

Task bug-hunter: "Final scan for any obvious bugs, edge cases, or issues"

Task test-coverage: "Verify all critical paths are tested, no gaps in coverage"
```

**Parallel Cleanup Scan** (spawn in ONE message):
```
# In a SINGLE message, scan for different cleanup needs:

Task post-task-cleanup: "Scan for temporary files, debug code, or test artifacts
that should be removed"

Task Explore: "Find any TODO comments, placeholder code, or incomplete
implementations that need addressing"
```

**Parallel Documentation Verification** (spawn in ONE message):
```
# In a SINGLE message, verify docs are complete:

Task Explore: "Check all code changes have corresponding documentation updates"
Task zen-architect: "Review docs for consistency with implementation"
```

### Single Agent Tasks

**post-task-cleanup** - For thorough cleanup:
```
Task post-task-cleanup: "Review entire workspace for any remaining
temporary files, test artifacts, or unnecessary complexity after
DDD workflow completion"
```

### Git Operations Note

**ALL git operations remain strictly sequential** with explicit user authorization:
1. Cleanup commit (remove DDD files)
2. Push to remote
3. Create PR

**Never batch git operations** - each requires user approval.

---

## Authorization Checkpoints

### 1. Remove and Commit DDD Working Files

⚠️ **Ask**: "Remove ai_working/ddd/ and commit the removal?"

- Show what will be deleted
- Explain: This creates a cleanup commit while preserving development history
- Get explicit yes/no
- If yes: Remove files, stage removal, commit with cleanup message

### 2. Clean ai_working/tmp/ Directory

⚠️ **Ask**: "Clean up ai_working/tmp/ directory?"

- Show what's in ai_working/tmp/
- Get explicit yes/no

### 4. Commit Remaining Changes

⚠️ **Ask**: "Commit these changes?"

- Show git diff
- Get explicit yes/no
- If yes, get/suggest commit message

### 5. Push to Remote

⚠️ **Ask**: "Push to remote?"

- Show branch and commit count
- Get explicit yes/no

### 6. Create PR

⚠️ **Ask**: "Create pull request?"

- Show PR description preview
- Get explicit yes/no
- If yes, create and show URL

---

## Important Notes

**Never assume**:

- Always ask before git operations
- Show what will happen
- Get explicit authorization
- Respect user's decisions

**Clean thoroughly**:

- DDD artifacts served their purpose
- Test artifacts aren't needed
- Debug code shouldn't ship
- Working tree should be clean

**Verify completely**:

- All tests passing
- Quality checks clean
- No unintended changes
- Ready for production

**Document everything**:

- Final summary should be comprehensive
- Include verification steps
- Note any follow-up items
- Preserve commit history

---

## Completion Message

```
✅ DDD Phase 5 Complete!

Feature: [name]
Status: Complete and verified

All temporary files cleaned.
Workspace ready for next work.

Summary saved above.

---

Thank you for using Document-Driven Development! 🚀

For your next feature, start with:

    /ddd:1-plan [feature description]

Or check current status anytime:

    /ddd:status

Need help? Run: /ddd:0-help
```

## Process

- Ultrathink step-by-step, laying out assumptions and unknowns, use the TodoWrite tool to capture all tasks and subtasks.
  - VERY IMPORTANT: Make sure to use the actual TodoWrite tool for todo lists, don't do your own task tracking, there is code behind use of the TodoWrite tool that is invisible to you that ensures that all tasks are completed fully.
  - Adhere to the @ai_context/IMPLEMENTATION_PHILOSOPHY.md and @ai_context/MODULAR_DESIGN_PHILOSOPHY.md files.
- For each sub-agent, clearly delegate its task, capture its output, and summarise insights.
- Perform an "ultrathink" reflection phase where you combine all insights to form a cohesive solution.
- If gaps remain, iterate (spawn sub-agents again) until confident.
- Where possible, spawn sub-agents in parallel to expedite the process.

---

## Troubleshooting

**"Make check is failing"**

- Fix the issues before finishing
- Or ask user if acceptable to finish with issues
- Note failures in final summary

**"Uncommitted changes remain"**

- That might be intentional
- Ask user what to do with them
- Document decision in summary

**"Can't push to remote"**

- Check remote exists
- Check permissions
- Check branch name
- Provide error details to user

**"PR creation failed"**

- Check gh CLI is installed and authenticated
- Check remote branch exists
- Provide error details to user
- User can create PR manually

---

Need help? Run `/ddd:0-help` for complete guide
