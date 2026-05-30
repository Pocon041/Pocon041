# Profile README and Contribution Snake Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Update the `Pocon041` profile README and automatically publish light and dark contribution snake SVG files.

**Architecture:** Keep the existing README layout. Add one GitHub Actions workflow that generates SVG files with `Platane/snk/svg-only@v3` and publishes them to the orphan `output` branch with `crazy-max/ghaction-github-pages@v4`.

**Tech Stack:** Markdown, GitHub Actions YAML, PowerShell verification commands

---

### Task 1: Add Contribution Snake Workflow

**Files:**
- Create: `.github/workflows/snake.yml`

- [ ] **Step 1: Run the pre-change verification and confirm it fails**

Run:

```powershell
if (-not (Test-Path '.github/workflows/snake.yml')) { throw 'Expected workflow to exist' }
```

Expected: FAIL because `.github/workflows/snake.yml` does not exist.

- [ ] **Step 2: Add the workflow**

Create `.github/workflows/snake.yml`:

```yaml
name: Generate contribution snake

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5

    steps:
      - name: Generate contribution snake SVG files
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Publish SVG files to the output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Task 2: Update the Profile README

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Replace stale copied profile values**

Remove the copied author header and commented GitHub starter template. Replace every
`bigorange18` value with `Pocon041`. Replace the three snake URLs with:

```html
https://raw.githubusercontent.com/Pocon041/Pocon041/output/github-contribution-grid-snake-dark.svg
https://raw.githubusercontent.com/Pocon041/Pocon041/output/github-contribution-grid-snake.svg
```

- [ ] **Step 2: Preserve existing layout and links**

Keep the existing profile introduction, social links, statistics widgets, movie
GIF, and footer image.

### Task 3: Verify and Publish

**Files:**
- Verify: `.github/workflows/snake.yml`
- Verify: `README.md`

- [ ] **Step 1: Parse YAML and check README references**

Run a PowerShell verification command that:

- Parses `.github/workflows/snake.yml` with Python and PyYAML.
- Confirms `README.md` does not contain `bigorange18`.
- Confirms README snake URLs reference the `output` branch.
- Confirms the workflow contains daily, manual, and `main` push triggers.

Expected: PASS with exit code `0`.

- [ ] **Step 2: Inspect and commit the diff**

Run:

```powershell
git diff --check
git status --short
```

Expected: no whitespace errors and only the intended files changed.

- [ ] **Step 3: Push the update**

Run:

```powershell
git push origin main
```

Expected: the new commits are pushed to `origin/main`.
