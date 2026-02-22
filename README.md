# PR Size Checker

A GitHub Action that checks pull request size and warns or blocks merge when PRs get too large.

Large pull requests are harder to review, more likely to contain bugs, and slow down your team. This action enforces configurable line-change thresholds so your team ships smaller, safer PRs.

## What it does

When a pull request is opened or updated, this action:

1. Counts the lines changed (additions + deletions)
2. Compares against your configured threshold
3. Posts a **check run** on the PR (pass/fail) and optionally a **comment**

You configure the threshold and feedback mode (blocking or advisory) in your [Flux](https://www.positronlabs.com) dashboard.

## Usage

```yaml
name: PR Size Check
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  pr-size:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: positronlabs/flux-pr-size-checker@v1
        with:
          flux-token: ${{ secrets.FLUX_TOKEN }}
```

## Setup

1. Sign up at [positronlabs.com](https://www.positronlabs.com) and create an organisation
2. Go to **Org Admin > Tools** and create an API key
3. Go to **Product > Tools** and configure PR Size Checker with trigger app "CLI"
4. Set the line threshold and feedback mode (blocking or advisory)
5. Add `FLUX_TOKEN` as a repository secret
6. Add the workflow above to your repository

## Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `flux-token` | Yes | API key from Flux (Org Admin > Tools > API Keys) |

## Behaviour

| PR size | Blocking mode | Advisory mode |
|---------|--------------|---------------|
| Under threshold | Check run: pass | Check run: pass |
| Over threshold | Check run: fail, CI blocked | CI passes, warning comment posted |
