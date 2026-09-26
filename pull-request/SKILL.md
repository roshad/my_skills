---
name: pull-request
description: Prepare, review, create, or update a GitHub pull request. Use for PR titles and descriptions, fork pushes, and publishing or editing a PR; require review of the exact public content before publication.
---

# Pull Request

Prepare a reviewable PR that follows the target repository's contribution rules. A request to create a PR authorizes preparation; it does not approve unseen commit messages, branch pushes, or PR text.

## Prepare

1. Identify the target repository, base branch, head branch, and whether the head is in the user's fork. Inspect the intended diff and current Git status. Check whether an existing PR already covers the work.
2. Read the repository's `CONTRIBUTING.md` before drafting or publishing. Also read applicable `AGENTS.md`, PR templates, and other repository instructions when present. If `CONTRIBUTING.md` is absent, say so; do not invent its rules.
3. Follow relevant contribution requirements for change scope, generated files, links or data formats, and checks. Run checks when authorized and permitted by the current session; distinguish local manual use, automated checks, and CI. State every check that was skipped, failed, or not yet reported. Do not overstate user-reported verification.
4. If a commit is needed, use the `commit-message` skill: show its exact proposed file set and message, and wait for the user's confirmation before committing. Do not push as part of that step.

## Review before publication

Show the user the exact proposed PR title and complete body, plus the base and head branches, commit(s), changed files, and check results. State which fork branch will be pushed. Wait for explicit confirmation of the draft before pushing or creating the PR. If the user changes the draft, show the revised final text before publishing unless their reply both supplies the replacement and authorizes publication. Do not treat a general request such as “create a PR” as approval of text the user has not seen.

Push only the reviewed branch to the intended fork. Do not force-push unless the user explicitly approves the history rewrite after seeing its effect. Create the PR against the reviewed base branch. Verify the published title, body, head, base, and URL, then report them to the user.

## Existing PRs

Before editing an existing PR title or body, show the exact replacement text or diff and wait for explicit approval. Before pushing additional commits, follow the commit review step and show the resulting PR change. Never silently amend, rebase, or rewrite a published branch.
