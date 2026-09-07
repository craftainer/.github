# 📦 craftainer

Devcontainers, crafted from parts that fit together.

Every repo in this org is a starter template: a runtime wired into a
devcontainer, with a set of infrastructure services you can add,
remove, or swap without re-plumbing the rest of the project. Use one
as-is, or fork it and change the panels you need changed.

## How a template is built

- **One frame.** `devcontainer.json`, a base compose file, CI, and a
  `docs/TEMPLATE.md` that stays identical across every instance of the
  template — so updates can flow back via template-sync.
- **Independent panels.** Each backing service — database, cache,
  object storage, broker — lives in its own compose stanza under
  `.devcontainer/stack/`, documented on its own.
- **Nothing you don't ask for.** Panels are opt-in. A template with
  five supported services doesn't run five containers unless your
  project needs them.

## Getting started

1. Pick the closest template repo below and click **Use this
   template**.
2. Open in your devcontainer — the stack starts with the default
   panels wired in.
3. Read that repo's `docs/TEMPLATE.md` for what's included and how to
   swap a panel.

> **Built AI-assisted.** These templates are developed with Claude
> Code — plans, changes, and checks are tracked in-repo so the
> reasoning behind a template's structure is as visible as the code.

## Templates

| Repo | Runtime | Swappable pieces | Status |
|---|---|---|---|
| [template-fastapi](https://github.com/craftainer/template-fastapi) | FastAPI | Postgres · Redis · S3 · MQTT · CI checks | ✅ live |
| template-node | Node / Express | Postgres · Redis · queue | 🚧 planned |
| template-go | Go | Postgres · S3 | 🚧 planned |

Have a stack to propose? Open an issue on the relevant repo describing
the runtime or service and how it composes with the existing frame —
panels are reviewed for whether they stay independent, not just
whether they work.

---

Brand assets (logo, palette, type) live in [`brand/`](../brand).
