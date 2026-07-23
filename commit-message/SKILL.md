---
name: commit-message
description: Analyze Git changes and propose or create atomic Conventional Commit messages that match the repository's established style, including whether it uses Gitmoji. Use when the user asks to write, improve, review, split, or execute Git commits, commit messages, staged-change commits, release commits, or changelog-related commits. Always present the exact proposed commit plan first and wait for explicit user confirmation before running git commit; never push automatically.
---

# Commit Message

Generate tool-compatible Conventional Commits that preserve the repository's existing style, atomic history, and user control.

## Format and emoji style

Always keep the Conventional Commit type at the beginning. Use one of these formats according to the repository's established commit style:

```text
<type>(<optional-scope>)<optional-!>: <gitmoji> <description>
<type>(<optional-scope>)<optional-!>: <description>
```

Determine emoji usage from recent project history before proposing a message:

- If recent commits consistently or predominantly use emoji, include Gitmoji and match its established placement.
- If recent commits consistently or predominantly omit emoji, do not add emoji.
- If history is absent, too sparse, or genuinely mixed with no dominant style, default to including Gitmoji after the colon.
- Explicit user instructions and repository rules override history and the default.

Examples:

```text
feat(timeline): ✨ 新增截图点即时刷新
fix(search): 🐛 修复中文关键词无法命中记录
docs: 📝 更新安装与发布说明
refactor(i18n): ♻️ 统一多语言加载机制
chore(release): 🔖 发布 v0.4.0
feat(api)!: ✨ 调整时间记录接口
feat(timeline): 新增截图点即时刷新
fix(search): 修复中文关键词无法命中记录
```

Keep the Conventional Commit type at the beginning. Do not use `✨ feat: ...`, because common parsers expect the type first.

## Type and Gitmoji mapping

Choose the narrowest accurate type. Apply the Gitmoji column only when the selected project style uses emoji:

| Type | Gitmoji | Use |
|---|---|---|
| `feat` | ✨ | User-visible capability |
| `fix` | 🐛 | Bug fix |
| `docs` | 📝 | Documentation only |
| `style` | 💄 | Formatting or visual styling without logic changes |
| `refactor` | ♻️ | Internal restructuring without intended behavior change |
| `perf` | ⚡️ | Performance improvement |
| `test` | ✅ | Tests only |
| `build` | 👷 | Build system or packaging |
| `ci` | 💚 | CI/CD configuration |
| `chore` | 🔧 | Maintenance not covered above |
| `revert` | ⏪️ | Revert a prior commit |

Use contextual variants when they better explain the change without inventing a nonstandard type:

- Dependencies: `chore(deps): ⬆️ ...`
- Release: `chore(release): 🔖 发布 vX.Y.Z`
- Localization: usually `feat(i18n): 🌐 ...`, or follow an explicitly configured project type such as `l10n`
- Security fix: `fix(security): 🔒 ...`
- Intentional removal: choose the semantic type and use `🔥`, for example `refactor: 🔥 移除废弃兼容层`

## Analyze before proposing

1. Read applicable repository instructions, including `AGENTS.md`, `.git-style.md`, `CONTRIBUTING.md`, commitlint configuration, and release documentation when present.
2. Run `git status --short`.
3. Prefer staged changes when the index is non-empty. Inspect them with `git diff --cached --stat` and `git diff --cached`.
4. If nothing is staged, inspect `git diff --stat` and `git diff`, but do not stage anything yet.
5. Inspect recent non-merge history with `git log -30 --no-merges --pretty=format:"%s"` to infer language, scope vocabulary, capitalization, release conventions, and especially whether commit subjects use emoji. Prefer the dominant recent pattern over isolated exceptions.
6. Identify distinct logical concerns. Propose multiple commits when changes are independently reviewable or revertible.
7. Do not include unrelated existing changes. Never discard, reset, overwrite, or rewrite user work to simplify a commit.

Explicit repository or user instructions override inferred history. Otherwise match the repository's dominant style, including emoji presence and placement. When history does not establish an emoji convention, use the Gitmoji format by default. Match the repository's dominant language; if unclear, use the user's language.

## Write the message

- Describe the outcome, not the editing activity.
- Keep the subject concise, specific, and free of a trailing period.
- Use a scope only when it adds useful location or domain context.
- Do not use vague subjects such as `update code`, `修改问题`, `misc fixes`, or `调整内容`.
- Do not claim behavior that the diff does not implement.
- Add `!` and a `BREAKING CHANGE:` footer for incompatible changes.
- Add a body only when the reason, migration impact, or non-obvious tradeoff matters.
- Add issue footers such as `Closes #123` only when supported by the task or repository context.

## Confirmation gate

Before any commit, show:

```markdown
### Proposed commit

Files:
- path/to/file

Message:
`feat(scope): ✨ description` or `feat(scope): description`, according to the inferred project style

Reason:
One concise sentence explaining the type, scope, and split.
```

For multiple commits, show them in execution order with the exact file set and message for each.

Then stop and request explicit confirmation. Do not run `git add` or `git commit` merely because the user asked for a message. Treat replies such as “确认提交”, “commit”, “按这个提交”, or an equally explicit approval as authorization for the displayed plan only.

If the user edits the message, present the final exact message before committing unless their reply explicitly both supplies the replacement and authorizes the commit.

## Commit after confirmation

1. Re-run `git status --short` and the relevant diff before committing. If the approved files or content changed materially, stop and present a refreshed plan.
2. If the user had already staged changes and approved one commit, commit the staged snapshot without silently adding other files.
3. If the approved plan requires staging, stage only the explicitly approved paths for the current atomic commit.
4. Preserve partial-file boundaries. Do not stage an entire file containing unrelated hunks unless the user approved that content.
5. Run repository-required checks when instructions require them. If checks fail, report the failure and do not create a misleading success commit.
6. Run `git commit` with the approved subject and any approved body/footer.
7. Verify with `git log -1 --oneline` and `git status --short`.
8. Report the commit hash, exact message, committed paths, and remaining uncommitted changes.

Never run `git push`, publish a release, create a tag, amend, rebase, or force-update history unless the user separately and explicitly requests that operation.

## Message-only requests

When the user asks only for a message, return 1-3 ranked suggestions. Include the recommended message first and a short explanation only when ambiguity exists. Do not commit.
