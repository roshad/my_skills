# Changelog and Semantic Version Guide

## Workflow

1. Preserve an established project format and version policy when one exists.
2. Identify the current released version from verified Releases, SemVer tags, and authoritative version files. Report conflicts.
3. Inspect actual product behavior and compatibility across the release scope.
4. Recommend the highest applicable SemVer impact.
5. Select only notable user-facing changes and group them with Keep a Changelog categories.
6. Write a durable `CHANGELOG.md` entry and a more natural GitHub Release body.
7. Verify compatibility, migration, security, performance, issue, contributor, installation, and download claims. Never invent facts.

## Semantic Version Decision

- `major`: incompatible change to a supported public API, CLI, configuration, file format, protocol, persisted data, updater contract, or documented workflow.
- `minor`: backward-compatible user capability, supported platform, public API, configuration option, or meaningful workflow.
- `patch`: backward-compatible bug, regression, vulnerability, or performance fix without a new capability.
- `none`: documentation, tests, formatting, internal refactors, CI, or maintenance with no notable shipped behavior change.

Use the highest level across mixed changes. Let actual behavior and compatibility override metadata, diff size, and change count. Treat removals, changed defaults, renamed settings, storage migrations, packaging, and updater changes as compatibility questions.

For a `0.y.z` project, increment `minor` for breaking changes by default, such as `0.4.2` to `0.5.0`, unless project policy says otherwise. Do not guess prerelease promotion. If a credible breaking change remains ambiguous, state uncertainty and prefer the compatibility-safe higher recommendation. Recommend `none` when no release-worthy change exists.

## Keep a Changelog Format

Use fixed emoji mappings while retaining the standard English category word:

- `### ✨ Added`
- `### 🔄 Changed`
- `### ⚠️ Deprecated`
- `### 🗑️ Removed`
- `### 🐛 Fixed`
- `### 🔒 Security`

Omit empty categories. Never use only an emoji as a heading.

Use:

```markdown
## [Unreleased]

## [1.4.0] - 2026-07-20

### ✨ Added

- 新增后台驻留功能，关闭主窗口后仍可继续记录。

### 🐛 Fixed

- 修复切换日期后时间轴滚动位置变化的问题。
```

When finalizing `Unreleased`, create the dated version below it, move only included items, retain `Unreleased`, and update verified comparison links.

## Writing Style

- Write one observable change per bullet.
- Lead with what changed and why it matters.
- Use specific nouns and verbs; avoid hashes, file paths, implementation jargon, and vague phrases.
- Omit routine tests, formatting, and invisible maintenance.
- Mention documentation only when it is itself a user deliverable.
- Avoid performance claims without measurements.
- Explain verified breaking changes and migration steps explicitly.
- Keep security notes useful without publishing exploit instructions.

## GitHub Release Notes

Use the smallest structure suitable for the release.

Patch release:

```markdown
# v1.4.1

## 🐛 Fixes

- Fix a user-visible regression.

**Full changelog:** [v1.4.0...v1.4.1](VERIFIED_COMPARISON_URL)
```

Normal release:

```markdown
# v1.4.0

## 🌟 Highlights

- Summarize the most important user outcome.

## ✨ What's New

- **Feature:** Explain what users can now do and why it matters.

## 🔄 Improvements

- Describe meaningful improvements to existing behavior.

## 🐛 Fixes

- Describe corrected user-visible problems.

## ⚠️ Breaking Changes

Include verified migration guidance, or omit this section when compatibility status is not needed.

## 📦 Installation / Upgrade

Provide only verified instructions.

**Full changelog:** [v1.3.0...v1.4.0](VERIFIED_COMPARISON_URL)
```

For a major release, lead with breaking changes and migrations. Rewrite changelog bullets into natural user-facing prose rather than copying them mechanically. Treat `CHANGELOG.md` as durable history and the GitHub Release as the current announcement; they need not be identical.

## Release Result

Finish with:

```text
Current version: 1.3.0
Recommendation: minor
Target version: 1.4.0
Release date: 2026-07-20
Confidence: high
Reason: backward-compatible user capabilities; no incompatible contract change found
Changelog: updated CHANGELOG.md / draft only
Release title: v1.4.0 - Background Tracking
Breaking changes: none verified
Migration required: no
Compare refs: v1.3.0...v1.4.0
```

Then provide the complete GitHub Release body as a standalone Markdown block.
