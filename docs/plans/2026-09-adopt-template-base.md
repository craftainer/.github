# Adopt template-base and refresh the org profile

## Status

In progress (steps 0-3 done; 4-5 remain, LICENSE deferred)

## Goal

Make this repo a regular, template-sync'd instance of
[`template-base`](https://github.com/craftainer/template-base), so it's
edited in the same devcontainer, with the same checks, CI, and
AI-assisted workflow as every other craftainer repo. Also bring
`profile/README.md` (the org homepage) up to date with the org as it
exists today. The homepage currently lists only `template-fastapi`, and
it describes a single-layer "one template per runtime" model. The org
now has a shared base plus three runtime templates built on it.

## Current state (2026-09-24)

This repo holds only `README.md`, `profile/README.md`, and `brand/`. It
has no devcontainer, hooks, CI, `CLAUDE.md`, or `LICENSE`.

Org repos, per the GitHub API:

| Repo | What it is | "Template repository" flag | `.devcontainer/stack/` |
|---|---|---|---|
| `template-base` | Frame only: devcontainer, CI, AI workflow. No runtime | yes | none |
| `template-fastapi` | FastAPI instance of base | yes | keycloak, mqtt, postgres, redis, s3, selenium |
| `template-axum` | Rust/axum counterpart to fastapi | **no** | keycloak, mqtt, postgres, redis, s3, selenium |
| `template-react` | Vite/React/TS/MUI frontend for fastapi's API and Keycloak realm | **no** | none |

`profile/README.md` is out of date in these ways:

- The Templates table leaves out `template-base`, `template-axum`, and
  `template-react`.
- It lists `template-node` and `template-go` as planned, but neither
  repo exists.
- fastapi's services are listed without Keycloak and Selenium.
- "How a template is built" says every template has
  `.devcontainer/stack/` panels. That's true only of the backend
  templates.
- It never explains the base → runtime template → your project chain.

`template-base`'s latest tag, `v1.0.0`, is behind `main`. It predates
the org rename fix in `template-sync` and the devcontainer-lock hook.

## Approach

### 0. Prerequisite in template-base: let a leaf instance classify its own files

The `template-sync-manifest` hook fails on any tracked path that isn't
listed in `.github/template-sync-manifest.yml`. That manifest is
`replace`-tier, so an instance that isn't also a template can't list
its own files there: `profile/` and `brand/` here, `src/` in an app. A
local edit would be overwritten on the next sync.

- In template-base, add an instance-owned extension file (proposed:
  `.github/template-sync-manifest.local.yml`, in the manifest's own
  `ignore` tier). `template_sync_manifest.py` merges it into the check
  but not into sync decisions. Document it in `docs/TEMPLATE.md`'s
  "Template sync" section. This needs its own plan in template-base.
- Cut a new template-base release (e.g. `v1.1.0`) that includes this
  and everything since `v1.0.0`. Every later step here copies from, and
  bootstraps sync against, that tag. It does not use `main`.

### 1. Copy the template-owned files

On a branch here, copy every `replace`-tier path from the release tag
verbatim, using `git archive <tag> <paths> | tar -x`:

- `.devcontainer/`, including `devcontainer-lock.json` and
  `resolve-ssh-agent.sh`
- `Dockerfile`, `scripts/`, `.dockerignore`, `.gitattributes`,
  `.gitignore`, `.pre-commit-config.yaml`, `.mcp.json`
- `.vscode/`, `.claude/`, `CLAUDE.md`
- `.secrets/README.md` and `.secrets/.gitkeep`
- `.github/workflows/`, `.github/scripts/`, `.github/CONTENTS.md`,
  `.github/renovate.json`, `.github/template-sync-manifest.yml`,
  `.github/ISSUE_TEMPLATE/`, `.github/PULL_REQUEST_TEMPLATE.md`
- `docs/TEMPLATE.md`, `docs/README.md`, and the `README.md` +
  `template.md` files in `docs/{adrs,frs,nfrs,plans}/`

Take all of them, including the ones this repo won't use yet, such as
`release.yml` and the `moderate-*` workflows. They're `replace`-tier, so
the next sync would recreate anything left out. They're also harmless
here: `release.yml` only runs when triggered by hand, and moderation
stays off until an `ANTHROPIC_API_KEY` secret exists.

This repo is special in one way. GitHub uses the community health files
in the `.github` repo, including those under its `.github/` folder, as
org-wide defaults. So `.github/ISSUE_TEMPLATE/` and
`PULL_REQUEST_TEMPLATE.md` become the defaults for any craftainer repo
that lacks its own. All four current repos have their own copies from
template-base, so nothing changes today. A repo created later without
the template would pick up the same forms, which is the behavior we
want.

### 2. Add the instance-owned files

- `.github/template-sync-manifest.local.yml`: lists `profile/` and
  `brand/` as `ignore`.
- `Makefile`: copy template-base's no-op targets unchanged. Nothing
  here is released.
- `LICENSE`: see Open questions.
- `README.md` (`merge`-tier): rewrite it in template-base's README
  shape (title, "vibe coded" note, pointer to `docs/TEMPLATE.md`,
  License section). Keep the existing pointers to `profile/` and
  `brand/`.
- `brand/README.md`: content stays the same. Check it against the "Code
  style" rules in `docs/TEMPLATE.md`.

Leave `docs/architecture.md`, the ADRs, and the rest of the
product-knowledge docs out. This repo has no product to document. The
`brand/` rationale is already in `brand/README.md`.

### 3. Refresh `profile/README.md`

- **How a template is built:** describe the two layers:
  - `template-base` is the shared frame: devcontainer, CI, hooks,
    Claude Code workflow, release Makefile contract, and template sync.
  - Each runtime template is an instance of base, adds its language and
    optional `stack/` panels, and is in turn a template for your
    project.
  - Updates flow down both hops through template-sync PRs.
- **Getting started:** step 3 still points at `docs/TEMPLATE.md`. Add a
  line saying `template-base` is the starting point for a repo with no
  runtime of its own, or for one using a language with no template yet.
- **Templates table:** four rows, all ✅:
  - `template-base`: no runtime; no services (frame only)
  - `template-fastapi`: FastAPI (Python); Postgres, Redis, S3, MQTT,
    Keycloak, Selenium
  - `template-axum`: axum (Rust); the same six services
  - `template-react`: React + Vite + TS; pairs with a fastapi backend,
    with no stack of its own

  The node/go rows depend on an open question below.
- Keep the brand pointer and the "Built AI-assisted" callout.

### 4. Verify

- Open this repo in its devcontainer. The user has to do this; it can't
  be done from template-base's container. Then run
  `prek run --all-files --hook-stage manual` and fix whatever fails.
  Expect end-of-file and whitespace fixes in `brand/*.svg`. The
  manifest check has to pass using the local extension file.
- Open a PR and confirm that `checks.yml` passes on both the amd64 and
  arm64 legs.
- Preview `profile/README.md` on the PR branch. Its relative link
  `../brand` must still resolve. After merging, check
  github.com/craftainer.

### 5. After merging

- Run `template-sync.yml` once by hand with
  `initial_sync_tag=<release tag>` and
  `template_repo=craftainer/template-base`. This seeds
  `template-sync-state.json` through a PR.
- Make sure Renovate is enabled for this repo so the pins in the copied
  files get bumped.
- Make sure there's no org-level `ANTHROPIC_API_KEY` secret. One would
  silently turn on issue moderation here and in every other repo.

## Open questions

- **License.** The sibling repos use GPL-3.0. Should this repo match
  them, or should `brand/` get a license suited to assets, such as
  CC BY 4.0 with a trademark note? Recommendation: GPL-3.0 at the
  root, plus a line in `brand/README.md` saying the mark isn't licensed
  for use as a trademark.
- **`template-axum` and `template-react` aren't flagged as template
  repositories.** Without the flag, the profile's "Use this template"
  step doesn't work for them. This is a settings change outside this
  repo. Should they be flagged, or does the profile list them as
  "preview" for now?
- **`template-node` / `template-go`.** Are these still planned? If not,
  drop the rows. Recommendation: drop them, since react already covers
  the Node frontend case.
- **Step 0's mechanism.** Is a local manifest extension the right fix,
  or should template-base instead add `profile/` and `brand/` to its
  own `ignore` tier? That's simpler, but it puts repo-specific paths
  into the shared base.
