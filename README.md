# github-actions

Run and lint GitHub Actions workflows locally — the `act` CLI and the
`actionlint` linter — plus the `guestfs-tools` package.

`github-actions` installs the toolchain you need to execute a workflow on your
own machine before pushing it: [`act`](https://github.com/nektos/act) runs the
workflow in a container, and
[`actionlint`](https://github.com/rhysd/actionlint) statically validates the
workflow file. Both are pinned release binaries fetched from their upstream
GitHub releases during the image build, so each lands at a fixed path and
self-reports a version — its presence and operability are directly verifiable.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `github-actions` |
| Distro | all (release binaries are distro-agnostic) |
| Binaries | `act` (pinned `0.2.89`) at `/usr/bin/act`; `actionlint` (pinned `1.7.12`) at `/usr/local/bin/actionlint` |
| Package | `guestfs-tools` |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list:

```yaml
my-ci-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-github-actions:v2026.242.1323'
```

Then, inside the built image:

```bash
act --version            # act version 0.2.89
actionlint -version      # 1.7.12
rpm -q guestfs-tools     # package presence
```

## Layout

- `charly.yml` — the `github-actions:` candy entity: the pinned
  `ACT_VERSION`/`ACTIONLINT_VERSION` vars, the two `download:` steps, the
  `guestfs-tools` package, and the `check:` steps.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Family skill: `/charly-coder:github-actions`
- `/charly-distros:github-runner` — a self-hosted Actions runner (distinct from `act`)
- `/charly-coder:docker-ce` — a common companion for `act` workflow execution
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
