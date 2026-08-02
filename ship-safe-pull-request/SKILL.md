---
name: ship-safe-pull-request
description: Safely delivers focused project changes through a pull request by confirming scope, inspecting repository state, isolating work, making intentional commits, running relevant validation, opening a well-described PR, reporting deployment previews, and waiting for explicit approval before merge. Use for implementing and publishing changes, preparing pull requests, or carrying an approved coding task through review-ready delivery.
---

# Ship Safe Pull Request

Deliver changes through a small, reviewable pull request while preserving unrelated work and keeping the user in control of important decisions.

## Follow repository instructions

1. Find and read every applicable `AGENTS.md` and `CLAUDE.md`, starting with the broadest scope and continuing to the target files.
2. Treat project-specific instructions as additions to broader instructions.
3. Follow the user's explicit request when it overrides a default in this workflow.

## Confirm the plan

1. Restate the intended outcome, affected area, and validation approach before editing code.
2. Ask only the questions whose answers would materially change the implementation.
3. Wait for explicit approval such as “OK”, “go ahead”, or “let’s do it” before implementation.
4. Treat approval as permission for the agreed scope, not for unrelated cleanup or broader product changes.

## Inspect repository state

1. Resolve the repository root, current branch, default branch, remotes, and working-tree status.
2. Inspect existing changes before staging or switching branches.
3. Preserve unrelated and user-owned changes. Never discard, overwrite, or silently include them.
4. Before any task that writes files in an existing Git repository, create an isolated `git worktree` from the latest remote default branch. Use the shared checkout only for read-only work unless the user explicitly authorizes editing it.
5. Treat an uninitialized repository without a first commit and a task that must continue specific uncommitted user changes as exceptions requiring an agreed safe approach before editing.
6. Never switch branches or use `git stash`, `git reset`, `git clean`, `git checkout --`, or equivalent cleanup commands in the shared checkout to prepare it for the task.
7. Re-check the active branch and worktree path immediately before commands whose target depends on them.

## Create an isolated worktree and branch

1. Fetch the remote state and start from the latest remote default branch unless the user names another base.
2. Use a unique worktree path and branch for each task.
3. Use the branch naming convention from repository instructions. Otherwise use `codex/<short-task-name>`.
4. Record the worktree path and exact base and head branch names for later checks and pull-request creation.
5. Do not remove a worktree while it contains uncommitted or unpushed changes.

## Implement the approved scope

1. Make the smallest coherent change that achieves the requested outcome.
2. Avoid unrelated refactoring, dependency upgrades, formatting sweeps, and visual redesign.
3. Preserve established project patterns unless changing them is part of the approved task.
4. Explain any newly discovered risk or material scope change and obtain approval before expanding the work.

## Validate proportionally

1. Discover the project's existing test, lint, typecheck, build, and smoke-test commands.
2. Run the narrowest relevant checks first, followed by broader checks when warranted by risk.
3. Prefer deterministic textual checks over an inaccessible deployment preview.
4. Do not report a check as passing unless it was actually run.
5. Report failures with their likely impact. Fix failures caused by the change; do not hide unrelated failures.

## Commit intentionally

1. Review the diff and status before staging.
2. Stage explicit paths when the working tree contains anything outside the approved scope.
3. Split work into small, meaningful commits when distinct changes can be reviewed independently.
4. Use concise commit messages that describe the completed change.

## Push and open the pull request

1. Push the head branch with upstream tracking.
2. Create the pull request with explicit head and base branches. Never rely on whichever branch happens to be checked out.
3. Write the title and body without asking the user to supply wording.
4. Include what changed, why it changed, user impact, and checks run.
5. Default to a draft pull request unless the user or repository instructions require a ready-for-review pull request.

## Report deployment previews

1. Detect whether the repository uses Vercel or another pull-request preview provider.
2. After opening the pull request, find and share the preview URL when available.
3. After every subsequent push to that pull request, find and share the newest preview URL again.
4. If preview access requires authentication, rely on build, lint, typecheck, tests, and safe HTTP checks for verification.

## Present the result

Always finish pull-request delivery with a concise, self-contained report containing:

1. **Outcome** — State the completed result in one sentence.
2. **Pull request** — Provide a clickable URL to the pull request.
3. **Preview** — Provide the latest clickable Vercel or other deployment-preview URL. If no preview is available, explicitly state why.
4. **Validation** — List every check actually run and its result. Never imply that an unrun check passed.
5. **Git** — State the head branch and relevant commit identifiers.
6. **Risks** — State unresolved failures, limitations, or `none`.
7. **Merge** — Ask for explicit permission to merge this specific pull request.

After every subsequent push to the pull request, send an updated self-contained report with the newest preview URL and validation state. Do not rely on an earlier message for required links or status.

## Stop before merge

1. Never merge based only on passing checks or an available preview.
2. Summarize the branch, commits, pull request, validation results, preview status, and remaining risks.
3. Ask for explicit permission to merge this specific pull request.
4. Merge only after receiving that permission and only if no new blocking condition has appeared.
