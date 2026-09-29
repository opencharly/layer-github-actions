# AGENTS.md — layer-github-actions

Standalone candy repo for the `github-actions` layer — the local GitHub Actions
toolchain: the pinned `act` release binary at `/usr/bin/act`, the pinned
`actionlint` release binary at `/usr/local/bin/actionlint`, and the
`guestfs-tools` package. The candy lives in `charly.yml` at the repo root and
projects the `github-actions` skill entity (`family: coder`).

Canonical files:

- `charly.yml` — the `github-actions:` candy entity and the `github-actions-skill:`
  skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:github-actions` — the owning skill: the `act` + `actionlint`
  install story, the pinned versions, and the observable checks. Load before
  editing or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections). Load before editing any entity field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the fixed-path
  file checks for `/usr/bin/act` and `/usr/local/bin/actionlint`, their
  `--version`/`-version` exit checks, and the `guestfs-tools` package check.
- The two `download:` steps pin concrete release tags — not `latest` — because
  the download verb content-addresses its cache by the URL; keep them pinned.

## Modify this repo

- Edit the `github-actions:` candy entity in `charly.yml`; keep the matching
  `github-actions-skill:` entity in step with it.
- A version bump is the `ACT_VERSION`/`ACTIONLINT_VERSION` var plus the
  corresponding `check:` assertion. Do not switch a pinned tag to `latest`.
- The install is distro-agnostic (upstream release binaries); `guestfs-tools` is
  the only package and comes from the distro repos.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
