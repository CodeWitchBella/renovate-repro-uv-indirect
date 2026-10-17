# Renovate reproduction: security updates for indirect uv dependencies

Reproduction for [renovatebot/renovate#43377](https://github.com/renovatebot/renovate/pull/43377).

## Current behavior

Renovate only extracts dependencies listed in `pyproject.toml`.
`mako` is only an indirect dependency (via `alembic`), and is locked in `uv.lock` at `1.2.1`, which is affected by several advisories, e.g. [CVE-2022-40023](https://osv.dev/vulnerability/GHSA-v973-fxgf-6xhp).
Renovate does not offer a security update for it.

## Expected behavior

With `osvVulnerabilityAlerts` enabled, Renovate opens a `[SECURITY]` PR that runs `uv lock --upgrade-package mako==1.4.2` (the lowest version fixing all advisories).
Other indirect dependencies stay disabled.

## Also covered

- uv workspace: `packages/member` shares the root `uv.lock`, and should not get indirect deps of its own.
- `markupsafe` is indirect for the root project, but a direct dependency of `packages/member`, so it should not be extracted as `indirect`.
- `sqlalchemy` is listed twice in `uv.lock` (forked resolution).
- The root project and `member` are `editable` lock entries, and should not be extracted.
