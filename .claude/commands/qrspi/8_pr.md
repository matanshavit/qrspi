---
description: Create a pull request with context from the design discussion
argument-hint: "thoughts/qrspi/<id>/"
---

# PR — Create the Pull Request

Create a pull request with a description grounded in the design document and the actual diff.

## Input

Read `$ARGUMENTS/design.md` for context on what was built and why.

## Process

1. **Detect the base branch** and **gather PR information:**
   - Detect base branch: `git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'` (falls back to `main`)
   - `git diff <base>...HEAD` — the full diff
   - `git log <base>...HEAD --oneline` — commit history
   - Read `$ARGUMENTS/design.md` for the "why" behind the changes

2. **Create the PR** using `gh pr create`:

   ```
   gh pr create --title "<concise title under 70 chars>" --body "$(cat <<'EOF'
   ## Summary
   [2-3 bullets: what this PR does and why, drawn from design.md]

   ## Design Decisions
   [Key decisions from design.md that reviewers should understand]

   ## Changes
   [Brief description of what changed, organized by component if multi-component]

   ## How to Verify
   - [ ] [Automated verification command]
   - [ ] [Manual verification step]

   ## References
   - Design: `$ARGUMENTS/design.md`
   - Plan: `$ARGUMENTS/plan.md`
   EOF
   )"
   ```

3. **Report the PR URL** to the user.

## Output

- PR created on GitHub
- URL reported to the user

## Rules

- Title under 70 chars. Use the body for details.
- The summary should explain WHY, not just WHAT. The diff shows what changed; the PR description should explain the reasoning.
- Reference the design and plan docs so reviewers can find the full context.
- If the branch isn't pushed yet, push it first with `git push -u origin <branch>`.
- If a PR already exists for this branch, update it with `gh pr edit` instead of creating a new one.
