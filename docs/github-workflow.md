# GitHub Workflow

This repo uses an Issues -> Labels -> PR -> docs loop for both human contributors and coding agents. Read this before opening a pull request.

## Summary

Every meaningful change should start as an issue, land on a focused branch, open a pull request, and merge into `main` after review. Small documentation typo fixes may skip the issue, but the PR should say why.

## Issues

- Open or pick an issue before changing repository behavior, adding a skill, or migrating a third-party skill.
- Use the bug or feature template and fill the required fields.
- Skill migration issues should include the source repo URL, source skill path, license or attribution notes, unsupported tools or frontmatter fields, target skill name, prompt-only vs tool-using decision, and acceptance criteria.
- Maintainers triage new work with type labels (`bug`, `enhancement`, `documentation`, `chore`), priority labels, and status labels.

## Labels

Use a small canonical set:

- Type: `bug`, `enhancement`, `documentation`, `chore`, `question`
- Priority: `priority:critical`, `priority:high`, `priority:medium`, `priority:low`
- Status: `status:triage`, `status:in-progress`, `status:blocked`, `status:wontfix`
- Community: `good first issue`, `help wanted`

Existing GitHub default labels can remain. Do not delete labels in bulk.

## Branches

Branch from the latest `main` and use an intent prefix:

| Prefix | Use | Example |
| --- | --- | --- |
| `feat/` | New feature or skill | `feat/add-web-scraping-skill` |
| `fix/` | Bug fix | `fix/import-metadata` |
| `docs/` | Documentation | `docs/github-workflow` |
| `refactor/` | Refactor with no behavior change | `refactor/skill-validation` |
| `chore/` | Tooling, config, or repo maintenance | `chore/update-labels` |
| `test/` | Test-only change | `test/skill-discovery` |

```bash
git checkout main
git pull --ff-only
git switch -c feat/<short-slug>
```

## Commits

Use Conventional Commits style where practical:

```text
<type>(<scope>): <subject>

<body: why, not just what>
```

Common types: `feat`, `fix`, `docs`, `refactor`, `chore`, `test`, `perf`.

## Pull Requests

Use the PR template. Required fields:

- Summary: 1-3 bullets explaining what changed and why.
- Related issue: `Closes #N`, `Fixes #N`, or `Refs #N`.
- Type of change: pick every applicable category.
- How to test: concrete commands or manual checks a reviewer can reproduce.
- Skill checklist: complete it for skill migrations or mark non-applicable items as N/A.

Keep PRs focused. One issue should usually map to one branch and one PR.

## Review

- Review against the issue and this workflow, not personal style preferences.
- Ask for specific changes with file or line references when possible.
- Approve only when the change is ready to merge.
- Coding agents can help with review, tests, docs, and migration checks. A human maintainer still owns the merge decision.

## Coding Agents

When working as Claude Code, Codex, Pi, or another coding agent in this repo:

1. Read `AGENTS.md` and this document before editing.
2. Check open issues first:

   ```bash
   gh issue list --repo oma-home/ohmyagent-skills-hub --state open --limit 50
   ```

3. If adding a third-party skill and no issue exists, create one before implementation.
4. Keep edits scoped to `skills/<skill-name>/` and directly related docs unless the issue says otherwise.
5. Verify skill discovery before handing off:

   ```bash
   find skills -name SKILL.md -print
   ```

6. Self-review the diff before opening a PR.

## Branch Protection

Recommended target state for `main`:

- Require pull requests before merging.
- Require at least one approving review.
- Require status checks once CI exists.
- Dismiss stale approvals when new commits are pushed.
- Block force pushes and deletions.

Apply branch protection in GitHub settings or with `gh api` only after maintainers confirm repository admin access and the exact required checks.
