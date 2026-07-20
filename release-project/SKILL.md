---
name: release-project
description: Analyze release changes, recommend the next Semantic Version, write or update Keep a Changelog and GitHub Release notes, prepare version files, publish GitHub-hosted software releases, resume interrupted releases, and verify artifacts. Use for changelog-only requests, release readiness audits, local release preparation, complete version publishing, GitHub Release updates, and Tauri desktop releases. Require explicit confirmation before local release mutation and again before remote publication.
---

# Release Project

Handle changelog writing and version publication as one continuous, resumable workflow. Let AI inspect, decide, and write; use deterministic project, Git, build, and GitHub commands for mutations and verification.

## Concepts

- A **state** records where the release currently is so a later invocation can continue without repeating completed operations.
- A **confirmation gate** pauses before the next stage until required checks pass or the user approves the proposed action.

Read [references/state-machine.md](references/state-machine.md) before mutating release state. Read [references/changelog.md](references/changelog.md) whenever determining SemVer, editing `CHANGELOG.md`, or drafting Release Notes. For Tauri projects, also read [references/tauri-github.md](references/tauri-github.md).

## Select the Mode

- **Changelog mode:** For requests to analyze changes, choose a version, update `CHANGELOG.md`, or draft Release Notes. Stop after the changelog result; do not change version files or remote state.
- **Prepare mode:** For requests to prepare a release. Complete changelog work, synchronize version files, and run validation, then stop before remote publication.
- **Publish mode:** For requests to release, publish, continue, resume, or finish. Inspect live state and advance through preparation and remote publication, pausing at confirmation gates.
- **Audit mode:** For status, readiness, or advice. Remain read-only and report the next safe action.

## Stage 1: Audit and Changelog

1. Read repository instructions, release documentation, existing changelog, and prior Release Notes.
2. Inspect the branch, upstream, worktree, remotes, valid SemVer tags, GitHub Releases, version files, package metadata, release scripts, and `.github/workflows`.
3. Verify GitHub authentication before proposing remote actions. When authentication is invalid, use public APIs only for read-only evidence and block remote mutations. Never expose secrets.
4. Identify the last completed stable release. Ignore malformed version tags and report disagreements among Releases, tags, and version files.
5. Establish the release scope. Default to committed history between the baseline and release HEAD. Exclude uncommitted changes and treat them as a readiness blocker unless the user explicitly finishes and includes them.
6. Inspect actual behavior, compatibility, migrations, packaging, and updater changes. Follow [references/changelog.md](references/changelog.md) to recommend `major`, `minor`, `patch`, or `none` and write the changelog and GitHub Release body.
7. Inspect release scripts before running them. Flag commands that automatically stage, commit, tag, push, publish, delete, or rewrite state.
8. Discover required validation commands from repository instructions, package scripts, manifests, CI, and project conventions. In audit mode, report but do not run commands that create outputs, caches, lockfile changes, or generated files unless explicitly requested.
9. Detect the publication owner: tag-triggered workflow, manually dispatched workflow, local GitHub command, or no automation.

Report:

```text
State: audited / audited-not-ready
Mode: changelog / prepare / publish / audit
Baseline: v1.3.0
Recommendation: minor -> v1.4.0
Release scope: verified
Worktree: clean / dirty
Version sources: package.json, Cargo.toml, tauri.conf.json
Checks: npm test, npm run build, cargo test, cargo clippy
Publication path: tag push -> GitHub Actions -> GitHub Release
Risks: none / list
```

In changelog mode, return the completed changelog and Release body now. Edit `CHANGELOG.md` only when requested, review the diff, and stop.

## Gate 1: Approve Local Preparation

For prepare or publish mode, show before changing release files:

- target version and evidence;
- changelog and Release Notes summary;
- files expected to change;
- exact validation commands;
- release scripts with hidden side effects;
- treatment of unrelated working changes.

Require explicit confirmation. A request to inspect, audit, or draft does not approve local mutation.

After approval:

1. Write or finalize `CHANGELOG.md` and preserve unrelated history.
2. Synchronize every authoritative version source with an inspected safe local command. Do not use a script that also pushes or publishes when only preparation is approved.
3. Update lockfiles and generated metadata through normal project tooling.
4. Run required checks. Stop on failure and report actionable evidence.
5. Verify all version sources agree with the target.
6. Review the release diff and exclude unrelated changes.
7. Ensure intended product changes are already in committed history. Do not sweep unfinished product edits into a release-only change.
8. Prepare the exact release change and complete SemVer tag plan.

Report `prepared` state with changed files, checks, target tag, release title, and complete Release body. Stop here in prepare mode.

## Gate 2: Approve Remote Publication

In publish mode, show the exact push, tag, workflow, and GitHub Release operations. Require explicit confirmation before changing remote state.

After approval:

1. Recheck branch, HEAD, worktree, target version, remote tag, and existing Release.
2. Require a complete SemVer tag such as `v1.4.0`, even if CI accepts a broad pattern such as `v*`. Never overwrite an existing tag silently.
3. Push the prepared release change and tag in the required order.
4. If tag push creates the Release, do not create a duplicate. Monitor the matching workflow.
5. If a manual workflow owns publication, verify that the tag exists, resolves to the intended commit, matches version files, and is consumed by the workflow before dispatching.
6. If no workflow creates the Release, create it from the verified tag and prepared body.
7. Once the Release exists, replace placeholder or raw notes with the curated title/body.
8. Fetch the saved Release and verify title, body, tag, draft/prerelease/latest state, and URL.
9. Verify every expected artifact independently from workflow and build configuration.

Use short progress updates while builds run. Waiting is normal; do not retrigger a healthy workflow.

## Resume and Failure Rules

- Reinspect live local and GitHub state on every resume.
- Treat release change, tag, workflow, Release, artifacts, and final notes as separate checkpoints.
- Never republish an existing version because a later verification step failed.
- Diagnose failed workflow logs before rerunning.
- If only notes are incomplete, edit the existing Release without rebuilding.
- If artifacts are partial, repair only the missing publication step.
- If approval, authentication, permissions, secrets, or checks block progress, report the exact unblock action and preserve state.

## Completion Report

Report the version and tag, release change, validation results, workflow result, GitHub Release URL, final notes status, verified assets, updater/signature verification, and remaining manual work.

Declare completion only when version files, tag, Release metadata, curated notes, and required artifacts agree.
