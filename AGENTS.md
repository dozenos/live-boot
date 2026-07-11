# AGENTS.md

## Project purpose

DozenOS fork of Debian's `live-boot` — the initramfs/boot scripts that bring up a Debian Live system. The DozenOS extensions mount the DozenOS config directories during system startup (see `components/9990-dozenos.sh`). Required for the entire DozenOS image to boot. Ships Debian packages `live-boot`, `live-boot-doc`, `live-boot-initramfs-tools`.

## Tech stack

- POSIX shell + Roff manpages.
- Plain `Makefile` (no autotools).
- Debian packaging in `debian/`.

## Build / test / run

```sh
make           # default target builds + lints scripts
make test      # syntax check across backend/frontend/components scripts
dpkg-buildpackage -us -uc
```

`SCRIPTS = backend/*/* frontend/* components/*`. The `Makefile` enforces shell syntax via `sh -n` per script.

## Repository layout

- `backend/` — initramfs-tools and similar backend integration.
- `frontend/` — user-facing helpers.
- `components/` — boot-time hooks; `9990-dozenos.sh` is the DozenOS hook for config mount.
- `manpages/` — Roff manpages with i18n PO files under `manpages/po/`.
- `debian/` — packaging.

## Cross-repo context

Pre-dep of the live ISO. Consumed by `dozenos/dozenos-live-build` (the `live-build` fork) which is in turn consumed by `dozenos/dozenos-build` for ISO assembly. Listed in an internal repository. README explicitly notes the long-term goal of dropping this fork in favour of upstream Debian `live-build` plus added files in the `dozenos-build` chroot.

## Conventions

- Default branch `rolling`. LTS branches `sagitta`/`circinus`/`equuleus` for backports.
- Commit / PR title format: `component: T12345: description` (Phorge task ID at https://dozenos.dev) — enforced via `dozenos/.github` reusables (see workflows below).
- Active workflows: `cla-check.yml`, `pr-mirror-repo-sync.yml`, `trigger-rebuild-repo-package.yml`. So this repo IS wired into the mirror pipeline (sends PRs to an internal repository) and the rebuild-dispatch chain (notifies `dozenos-build-packages` to rebuild on merge).

## Notes for future contributors

- Long-term plan (per README): retire this fork. Don't add features here that could equally live in `dozenos-build`'s chroot overlay.
- Any change touching `components/9990-dozenos.sh` affects boot-time config mount on every DozenOS install — coordinate with `dozenos-1x` config-loading code.
