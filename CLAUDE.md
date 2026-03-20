# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a collection of shared GitHub Actions (composite actions) and reusable workflows used across KUKSA CI pipelines. Actions are versioned via git tags and referenced externally as `eclipse-kuksa/kuksa-actions/<action-name>@4`.

## Actions

| Directory | Purpose |
|-----------|---------|
| `check-markdown/` | Checks all `*.md` links using `markdown-link-check@3.12.2`. Config in `config.json` ignores `#`, `%`, and `http://localhost` patterns; treats HTTP 200/403/429 as alive. |
| `check-dash/` | Runs Eclipse Dash license compliance tool (downloads `dash.jar` at runtime). Accepts optional `dashtoken` to create Eclipse IP tickets. Archives `.out` and `.report` files as artifacts. |
| `post-container-location/` | Writes a step summary snippet with `docker pull` / `docker run` commands for a given OCI image. |
| `spdx/` | Runs `verify-spdx-headers` (Python 3 script) on an explicit comma-separated file list. Checks that each file has an SPDX license identifier matching accepted licenses. |

## Reusable Workflows

| File | Purpose |
|------|---------|
| `.github/workflows/check_ghcr_push.yml` | Returns `push=true/false` output — `true` only for push/tag events on official `eclipse-kuksa` or legacy `eclipse/kuksa.*` repos. Used by consumer workflows to gate `ghcr.io` image pushes. |
| `.github/workflows/pre-commit.yml` | Runs pre-commit hooks on changed files in PRs. |
| `.github/workflows/check_license.yml` | Runs the `spdx` action on files changed in a PR, enforcing `Apache-2.0`. |

## Pre-commit Hooks

Configured in `.pre-commit-config.yaml`:
- `trailing-whitespace` (excludes `.dbc` files)
- `end-of-file-fixer` (excludes `.dbc`, `.json`, `.token`)
- `check-yaml`
- `check-added-large-files`

To run locally: `pre-commit run --all-files`

## SPDX Script Notes

`spdx/verify-spdx-headers` is a Python 3 script. It reads the `files` and `licenses` env vars (comma-separated). Recognized extensions: `.py`, `.proto`, `.rs`, `.yml`, `.yaml`, `.json`, `.toml`, `.md`, `.rb`, `.c/.h`, `.cpp/.hpp/.cc/.hh/.kt/.java`, `.go`. Files under `.git` and `.github` are skipped.

## License

All files in this repo must carry an `Apache-2.0` SPDX header where applicable (enforced by `check_license.yml` on PRs).
