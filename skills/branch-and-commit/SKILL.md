---
name: branch-and-commit
description: Proposes a git branch name and Conventional Commits 1.0.0 message from current changes. Use when the user asks for a branch name, commit message, or both before committing, when wrapping up work, or when they want git metadata without committing or pushing.
disable-model-invocation: true
---

# Branch and Commit

Propose a branch name and [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/) message from the current working tree. Do **not** create the branch, stage, commit, or push unless the user explicitly asks.

## When to run

- User requests a branch name, commit message, or both.
- User wants git metadata before committing.
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

## Branch name

Conventional Commits does not define branch names; use a lightweight parallel convention:

**Format:** `<type>/<short-kebab-slug>`

| Prefix     | Typical use                     |
|------------|---------------------------------|
| `feat`     | New feature or capability       |
| `fix`      | Bug fix                         |
| `docs`     | Documentation only              |
| `chore`    | Tooling, deps, hygiene          |
| `refactor` | Behavior-preserving restructure |
| `ci`       | CI/CD changes                   |
| `test`     | Tests only                      |
| `perf`     | Performance work                |

Rules:

- Lowercase, hyphen-separated; aim for ≤ 50 characters.
- Name the outcome, not touched paths (`add-oauth-login`, not `update-auth-files`).
- One coherent deliverable per branch; split unrelated work.

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
- **body** — blank line after description; free-form paragraphs with extra context. Prefer **why** over **what** when the user asks for a why-focused message.
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
- Omit body when the description is sufficient.
- Split mixed-type changes into separate commits/messages when possible.

## Output format

Return:

```markdown
**Branch:** `type/slug`

**Commit:**
```
type(scope): description

Optional why-focused body.

Optional footers.
```
```

State that nothing was committed or pushed unless the user asked.

## Do not

- Run `git commit`, `git push`, `git checkout -b`, or `git add` unless explicitly requested.
- Use vague slugs (`updates`, `wip`, `changes`).
- List every changed file in the body.
- Combine unrelated changes in one message.

## Examples

**Feature with scope**

```
feat(auth): add refresh token rotation

Reduce session hijack window by issuing short-lived access tokens and
rotating refresh tokens on each use.
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
