<!-- ref:git-commit-v1 -->
# Git Commit, Push & PR (CLI-only)

Local and Host share this procedure using the caller's authorized tools.
No specific agent or model selection is required by this shared Git procedure.
Entrypoint model preferences do not require runtime attestation here. This
procedure neither changes the model nor widens access.

Stage all non-ignored workspace changes, including `agent-output/`, `infra/`,
and `.github/skills/sensei/`; auto-generate a conventional commit and push to
the current branch. This repository permits a direct push to `main` when the
user requests it. Ask about a PR only when pushing a non-main branch. Uses
`git` and `gh` only — no MCP tools.

## Scope

- Workspace must be a git repository with `origin` configured.
- `gh` CLI must already be authenticated. Do not run `gh auth` or request secrets
  through chat; report authentication failures without switching credentials.
- Verify author identity with `git var GIT_AUTHOR_IDENT` and
  `git var GIT_COMMITTER_IDENT` before staging. If either fails, restore the
  user's intended dotfiles configuration; never invent an author or overwrite
  their identity with repository-local defaults.
- Git author identity and push credentials are separate. On an account mismatch,
  compare `gh api user --jq .login` with the account in Git's denial. With explicit
  user authorization, use the command-scoped GitHub CLI helper shown in Step 4;
  do not change persistent credential configuration.
- No path exclusions beyond `.gitignore`; do not force-add ignored files or secrets.
- A direct commit and push to `main` is permitted in this repository when requested by the user.
- Never force-push.

### Identity And Access Troubleshooting

Run this sequence only when identity or access fails, not before every healthy commit:

1. Identify the environment of the failing command: Windows, WSL and the container
  have separate paths and configuration. Confirm a file exists there before recreating it.
2. Use the author checks above and `git config --show-origin --get-regexp '^user\.(name|email)$'`
  to locate missing dotfiles configuration. Author identity does not authenticate a push.
3. Test ordinary Git access with `git ls-remote <intended-private-repository> HEAD`.
  Inspect the active credential helper and compare the denied account with the intended account;
  a successful `gh` lookup does not establish that Git uses the same credentials.
4. For dev containers, test host access before forwarded container access. After an authorized
  repair, repeat the original command without overrides and verify dotfiles persistence after rebuild.

Do not repeatedly reauthenticate without identifying the failing layer. Any account refresh or
credential-route change requires explicit authorization; preserve other accounts and never expose secrets.
A read-only lookup verifies repository access, not write permission; only the authorized push proves publication.

## Inputs

| Variable | Source                                  | Default        |
| -------- | --------------------------------------- | -------------- |
| subject  | argument-hint or generated from diff    | auto           |
| branch   | `git branch --show-current`             | current branch |
| pr_mode  | user choice (new / update / skip; non-main only) | ask, non-main only |
| pr_base  | user choice                             | `main`         |

## Workflow

### Step 0 — Identify the current branch

Resolve the active branch:

```bash
BRANCH="$(git branch --show-current)"
```

All following Git commands operate on the entire worktree. Git's normal
ignore rules remain in force; never use force-add to include ignored files.

### Step 1 — Inspect

Run these in parallel and show the output:

```bash
echo "$BRANCH"
git status --short
git diff --stat HEAD
```

Stop if:

- the worktree status is empty.

### Step 2 — Stage all changes

Inspect the existing index with `git diff --cached --name-only`. Review staged
and unstaged changes, including user-authored changes, before staging. Do not
commit secrets or ignored files. After staging, review the entire index and
confirm it contains the intended worktree changes.

Stage every non-ignored change:

```bash
git add -A
git diff --cached --stat
```

### Step 3 — Compose conventional commit (auto)

If the user passed a subject via the argument-hint, use it (wrap it in
`<type>(<scope>): <subject>` if missing the prefix).

Otherwise, read `.github/skills/apex-github-operations/references/commit-conventions.md`,
inspect the staged diff:

```bash
git diff --cached -- . ':(exclude)*.lock' ':(exclude)package-lock.json' | head -200
```

…and compose a message in this shape:

```text
<type>(<scope>): <short sentence-case subject>

- <bullet 1>
- <bullet 2>
```

**Do not ask for confirmation.** Commit the auto-generated message
immediately in Step 4. The user reviews the result via the commit hash +
summary table at the end and can amend if needed. The only remaining
confirmation gate is the PR decision in Step 5.

### Step 4 — Commit and push

```bash
git commit -m "<auto subject>" -m "<auto body>" &&
git push origin "$BRANCH"
```

Never join commit and push with `;`: a failed commit must prevent the push.
For an explicitly authorized account-mismatch recovery, replace only the push
command with this command-scoped helper (keep the commit-success condition):

```bash
git -c credential.helper= -c credential.helper='!gh auth git-credential' push origin "$BRANCH"
```

If a pre-commit hook fails, capture its output, summarize the error and stop. Do not retry.
If push is rejected, print the error and suggest `git pull --rebase` — do
not force-push.

Show the resulting commit hash:

```bash
git log -1 --pretty=format:'%h %s'
```

### Step 5 — PR decision (non-main branches only)

If `BRANCH` is `main`, skip this step: the requested direct push is complete.
Otherwise, detect any open PR for this branch:

```bash
gh pr list --head "$BRANCH" --state open --json number,url,title
```

Call `vscode/askQuestions` with one question:

- header: `pr-action`
- question: `What do you want to do with a pull request?`
- options:
  - If an existing PR was found: `Update existing PR #<number>` (recommended)
  - `Create a new PR to main`
  - `Skip — no PR right now`

Branch on the answer:

**Update existing PR** — the push in Step 4 already updated the branch.
Optionally refresh the PR body or title:

```bash
gh pr view <number> --json url,title,state
# If user supplied a new title/body in their reply:
# gh pr edit <number> --title "<title>" --body "<body>"
```

Print the PR URL.

**Create a new PR** — auto-fill from the commit:

```bash
gh pr create --base main --head "$BRANCH" \
  --title "<commit subject>" \
  --body "$(git log origin/main..HEAD --pretty=format:'- %s')"
```

If `gh pr create` fails because the branch is not pushed or base is missing,
print the error verbatim. Provide the compare URL as fallback:
`https://github.com/<owner>/<repo>/compare/main...<branch>`.

**Skip** — print "PR step skipped." and finish.

## Output

Print this summary table at the end:

| Step         | Result                                                            |
| ------------ | ----------------------------------------------------------------- |
| Excluded     | None (Git ignore rules still apply)                              |
| Files staged | N files                                                           |
| Commit       | `<hash>` `<subject>`                                              |
| Push         | `origin/<branch>` — pushed                                        |
| Pull request | `<URL>` (created / updated) or `skipped`                          |

## Rules

- Stage all non-ignored workspace changes, including `agent-output/`, `infra/`,
  and `.github/skills/sensei/`; inspect the complete staged diff before committing.
- A direct commit and push to `main` is permitted in this repository when the
  user requests it. Skip the PR decision when the pushed branch is `main`.
- Never use `git push --force` or `--force-with-lease` in this procedure.
- Step 3 is **non-interactive** — the prompt auto-generates the commit
  message and commits without asking. The PR action in Step 5 applies only to
  non-main branches.
- Use `git` and `gh` exclusively — do not call any GitHub MCP tool.
- Do not use interactive flags (`-i`) on `mv`/`rm`/`cp` or `read -p`. Pipe
  long output (>50 lines) into a file under `tmp/` if needed.
