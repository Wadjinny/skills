---
name: publish-github
description: >-
  Publishes a local project to GitHub as Wadjinny/<name-of-project>: git init,
  README with run instructions, .gitignore, secret-history check, .env.example,
  commit, then a private repo with description and tags. Use when the user
  asks to publish to GitHub, create a GitHub repo, git init and push, or names
  the publish-github skill.
---

# Publish GitHub

Owner is always `Wadjinny`. Repo name is the project directory name unless the user gives another.

Do this to the **current project**, not to `~/.cursor/skills`.

## Workflow

- git init (if not done)
- add readme or update it, includes the way to run the project
- add / update .gitignore
- check if there is any commited secrets, if yes, then untrack the file from history
- for any .env file add a tracked .env.example whith var names + placeholders
- commit
- push to github: Wadjinny/<name-of-project>
	- check if there is any suspiciously same repo already existes, to avoid duplicate
	- if not exists, new private repo by default
	- add description to github repo
	- add tags to github repo

Run the duplicate-repo check **before** `gh repo create`. Scan secrets **before** commit and **before** push.

## Hard rules

- Never update git config. If identity is missing, pass `-c user.name` and `-c user.email` for that command only (copy from another of the user's repos).
- Never skip hooks. Never force-push unless the user explicitly asks.
- Never commit real secrets. Never copy `.env` values into `.env.example` or the README.
- If a secret is already on a GitHub remote, **stop**. Do not rewrite and force-push. Report the files and wait.
- If a suspiciously similar `Wadjinny/*` repo exists, **stop** and ask. Do not create a duplicate.
- Do not `git init` inside an existing repo's subdirectory.

## git init (if not done)

```bash
git rev-parse --is-inside-work-tree
```

If that fails, `git init -b main` in the project root.

If `origin` already points at `Wadjinny/<name>`, skip create and push to that remote after the local steps. If `origin` points somewhere else, stop and ask.

## README

Add `README.md` or update the existing one so it includes **the way to run the project**.

Infer run commands from the repo (do not invent a stack):

| Evidence | Run section should cover |
|---|---|
| `package.json` | install + the real start/dev script; package manager from lockfile (`pnpm-lock.yaml` → pnpm, etc.) |
| `pyproject.toml` / `requirements.txt` | venv/install + the actual entry (`uvicorn`, `python -m`, CLI) |
| `Cargo.toml` | `cargo run` |
| `go.mod` | `go run` |
| `docker-compose*.yml` | `docker compose up` |
| `Makefile` | the documented/default target |

If a README already has a working run section, keep it and only fill gaps.

## .gitignore

Add or update `.gitignore`. Merge a language template with secret/noise paths.

Template (pick from the project): `gh api gitignore/templates/Node --jq .source` (also `Python`, `Go`, `Rust`, `VisualStudio`, …).

Always ignore at least:

```
.env
.env.*
!.env.example
*.pem
*.p12
id_rsa
id_rsa.dsa
credentials.json
```

Keep already-tracked source files tracked. After updating gitignore, `git rm --cached` only for secret/noise files that must not stay in the index.

## Committed secrets → untrack from history

Scan **working tree, index, and full git history** (all commits, all branches).

Look for:

- Filenames: `.env`, `.env.local`, `.env.production`, `credentials.json`, `*secret*`, `*.pem`, `id_rsa`, `google-services.json` with keys
- Content: `AKIA…`, `ghp_`, `gho_`, `sk-`, `-----BEGIN … PRIVATE KEY-----`, `api_key` / `token` / `password` assigned to real values

Commands:

```bash
git ls-files
git grep -I -n -E "AKIA[0-9A-Z]{16}|ghp_[A-Za-z0-9]{20,}|gho_[A-Za-z0-9]{20,}|-----BEGIN .*PRIVATE KEY-----" $(git rev-list --all)
```

If `gitleaks` or `trufflehog` is installed, run it too.

**Uncommitted secret file:** add it to `.gitignore`; do not `git add` it.

**Committed, never pushed:** untrack from history, then gitignore.

1. `git rm --cached --ignore-unmatch -- <file>` (current tree)
2. Rewrite history so the file is gone from every commit:

```bash
git filter-repo --invert-paths --path <file> --force
```

If `git-filter-repo` is missing, install it or use an equivalent path-removing history rewrite. `git rm --cached` alone is **not** enough.

Then add `.gitignore` so it cannot come back.

**Committed and already pushed:** stop. List paths and commits. Do not filter + force-push.

## .env → tracked .env.example

For every `.env`, `.env.local`, `.env.development`, `.env.production` (not `.env.example`):

1. Parse `KEY=value` lines (skip blank and `#` comments).
2. Write or update a tracked `.env.example` with the **same keys** and **placeholders only** (`DATABASE_URL=your-database-url`, `API_KEY=your-api-key`).
3. If `.env.example` already exists, add missing keys; do not replace good placeholders; never fill in real values.

`git add .env.example` — not the `.env` files.

## Commit

Stage the intended files (README, `.gitignore`, `.env.example`, source). Confirm `git status` / `git diff --cached` contains no secrets.

Commit with a short why-message. Do not commit if there is nothing to commit.

## Push to Wadjinny/<name-of-project>

`<name-of-project>` = directory name (kebab-case as-is). User override wins.

### Duplicate check

```bash
gh repo view Wadjinny/<name-of-project>
gh repo list Wadjinny --limit 200 --json name,description
```

Treat as suspiciously the same (stop and ask):

- exact name, case-insensitive
- same name after stripping `-` and `_` (`my_app` vs `my-app`)
- one name is the other plus a thin suffix (`-local`, `-app`, `-api`, `-copy`, `-2`)
- description/name clearly the same product

If the exact repo already exists and the user meant to update it, ask before pushing.

### Create if missing (private by default)

```bash
gh repo create "Wadjinny/<name-of-project>" --private --description "<description>" --source=. --remote=origin --push
```

Only use `--public` if the user asked.

**Description:** one line from the README intro or `package.json` / `pyproject` description (what the project is). GitHub max 350 characters.

**Tags** (GitHub topics, after create):

```bash
gh repo edit "Wadjinny/<name-of-project>" --add-topic <t1> --add-topic <t2>
```

Topics: lowercase, hyphens, from actual stack/purpose (`python`, `typescript`, `cli`, `nextjs`). Max 20. Do not invent unrelated tags.

If create succeeded but push failed, fix and `git push -u origin HEAD` — no force.

## Done

Return the repo URL, visibility, description, topics, and whether any secret files were removed from history.
