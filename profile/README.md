# 📦 craftainer

Devcontainers, crafted from parts that fit together.

Every repo in this org is a starter template: a runtime wired into a
devcontainer, with a set of infrastructure services you can add,
remove, or swap without re-plumbing the rest of the project. Use one
as-is, or fork it and change the panels you need changed.

## How a template is built

- **A shared base.** [`template-base`](https://github.com/craftainer/template-base)
  is the frame every repo here starts from: devcontainer, CI, hooks, the
  Claude Code workflow, a release Makefile contract, and template-sync.
  It has no runtime.
- **A runtime on top.** Each runtime template is an instance of base
  that adds its language and, for the backends, independent panels under
  `.devcontainer/stack/`: one compose stanza per service, documented on
  its own. It is in turn a template for your project.
- **Updates flow down.** Fixes land in base, reach the runtime
  templates, and then your project, each hop as a template-sync PR.
- **Nothing you don't ask for.** Panels are opt-in. A template with six
  supported services doesn't run six containers unless your project
  needs them.

## Getting started

1. Pick the closest template repo below and click **Use this
   template**.
2. Open in your devcontainer — the stack starts with the default
   panels wired in.
3. Read that repo's `docs/TEMPLATE.md` for what's included and how to
   swap a panel.

Start from `template-base` for a repo with no runtime of its own, or
one in a language with no template yet.

> **Built AI-assisted.** These templates are developed with Claude
> Code — plans, changes, and checks are tracked in-repo so the
> reasoning behind a template's structure is as visible as the code.

## Templates

| Repo | Runtime | Swappable pieces | Status |
|---|---|---|---|
| [template-base](https://github.com/craftainer/template-base) | none | frame only: devcontainer · CI · AI workflow | ✅ live |
| [template-fastapi](https://github.com/craftainer/template-fastapi) | FastAPI (Python) | Postgres · Redis · S3 · MQTT · Keycloak · Selenium | ✅ live |
| [template-axum](https://github.com/craftainer/template-axum) | axum (Rust) | Postgres · Redis · S3 · MQTT · Keycloak · Selenium | ✅ live |
| [template-react](https://github.com/craftainer/template-react) | React + Vite + TS | none of its own; pairs with a fastapi backend | ✅ live |

Have a stack to propose? Open an issue on the relevant repo describing
the runtime or service and how it composes with the existing frame —
panels are reviewed for whether they stay independent, not just
whether they work.

---

Brand assets (logo, palette, type) live in [`brand/`](../brand).
