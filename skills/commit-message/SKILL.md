---
name: commit-message
description: Proposes a Conventional Commits 1.0.0 message from current changes. Use when the user asks for a commit message before committing, when wrapping up work, or when they want commit metadata without committing or pushing.
disable-model-invocation: true
---

# Commit Message

Propose a [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) message from the current working tree. Do **not** stage, commit, or push unless the user explicitly asks.

## When to run

- User requests a commit message.
- User wants commit metadata before committing.
- User is finishing a task and needs a conventional subject/body.

Skip when the user explicitly asks to commit or push — follow their commit workflow instead.

## Inspect first

Run in parallel when in a git repository:

```bash
git status --short
git diff --stat
git log -5 --oneline
```

Read enough of `git diff` (or the user's stated goal) to infer intent. When `git log` shows an established style, match its casing, scope habits, and verbosity. Prefer the user's described outcome over a file inventory.

## Commit message

Follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/#specification):

```
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

### Required elements

- **type** — noun prefix and required colon + space. Use `feat` for new features, `fix` for bug fixes. Other common types: `build`, `chore`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`, `revert`.
- **description** — short imperative summary immediately after the prefix (e.g. `fix: array parsing issue when multiple spaces were contained in string`).

### Optional elements

- **scope** — noun in parentheses for the affected area: `feat(parser): add array support`.
- **body** — blank line after description; short why-focused paragraph (1-2 lines). Only when the user asks for detail or the change's rationale isn't evident from the diff; otherwise omit.
- **footers** — blank line after body; git-trailer style (`Refs: #123`, `Reviewed-by: Name`). Use `BREAKING CHANGE: <description>` for breaking changes.

### Breaking changes

Indicate either:

- `!` before the colon: `feat(api)!: remove legacy endpoints`, or
- footer: `BREAKING CHANGE: environment variables now take precedence over config files`

Both may be used together per the spec.

### Style defaults

Unless the repository's `git log` clearly differs:

- Lowercase type and scope.
- Imperative description: "add", "fix", "remove" — not "added" or "adds".
- Keep the description ≤ 72 characters when practical.
- Default to a **subject-only** message (single line). Add a brief (1-2 line) why-focused body only when the change's rationale isn't evident from the diff, or the user explicitly asks for more detail.
- Split mixed-type changes into separate commits/messages when possible.

## Output format

Return:

```markdown
**Commit:**
```
type(scope): description
```

Body (1-2 lines) only when the change's why isn't evident from the diff; footers only for breaking changes or trailers.

State that nothing was committed or pushed unless the user asked.

## Do not

- Run `git commit`, `git push`, or `git add` unless explicitly requested.
- List every changed file in the body.
- Add a body that restates the diff, exceeds a few lines, or pads with boilerplate.
- Combine unrelated changes in one message.

## Examples

**Feature with scope**

```
feat(auth): add refresh token rotation
```

**With body (why not evident from diff)**

```
feat(auth): add refresh token rotation

Issuing short-lived access tokens narrows the session hijack window.
```

**Fix, description only**

```
fix: prevent duplicate form submission on double-click
```

**Breaking change**

```
feat(api)!: rename user profile endpoint

BREAKING CHANGE: `/v1/profile` is now `/v1/users/me`.
```

**Docs**

```
docs: correct installation steps for Node 20
```
