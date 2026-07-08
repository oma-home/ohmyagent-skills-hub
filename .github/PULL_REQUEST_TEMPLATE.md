## Summary

<!-- 1-3 bullet points. What changed and why. -->

## Related issue

<!-- Use "Closes #123" / "Fixes #123" / "Refs #123" so GitHub links it. -->

Closes #___

## Type of change

- [ ] Bug fix (non-breaking)
- [ ] New feature (non-breaking)
- [ ] Skill migration / adaptation
- [ ] Refactor / chore (no behavior change)
- [ ] Docs only
- [ ] Breaking change (something existing behaves differently)

## How to test

<!-- What did you run to verify this works? Be specific enough that a reviewer can reproduce your check. -->

## Skill checklist

- [ ] Source repository and source path are recorded in the issue, if this ports a third-party skill
- [ ] `SKILL.md` includes `name`, `description`, and `metadata.ohmyagent.compatibility: pi`
- [ ] Unsupported runtime tools are removed, mapped, or documented in `metadata.ohmyagent.notes`
- [ ] Referenced files exist and stay inside the skill folder
- [ ] Discovery was checked with `find skills -name SKILL.md -print`

## General checklist

- [ ] Read `docs/github-workflow.md` before opening this PR
- [ ] Self-reviewed the diff
- [ ] Tests or verification steps added/updated, or marked N/A
- [ ] Docs updated, or marked N/A
- [ ] No unrelated changes in this branch
