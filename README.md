# OhMyAgent Skills Hub

Official Pi-compatible skills for OhMyAgent.

This repository is the curated source for skills that are expected to work well
inside OhMyAgent and the Pi coding-agent runtime. Community skills often target
Claude Code, Codex, Cursor, or other agents directly; their `SKILL.md`
frontmatter may contain tool declarations or runtime assumptions that OhMyAgent
does not support. Skills in this hub should be reviewed and adapted before they
are marked as official.

## Collaboration

Read [`docs/github-workflow.md`](docs/github-workflow.md) before opening issues
or pull requests.

## Directory Layout

Each skill lives in its own directory:

```text
skills/
  example-skill/
    SKILL.md
    references/
    scripts/
    assets/
```

Only `SKILL.md` is required. Supporting directories are optional and should be
referenced from `SKILL.md` by relative path.

## Skill Frontmatter

Every `SKILL.md` must include the standard agent skills fields:

```yaml
---
name: example-skill
description: A concise description of when the skill should be used.
---
```

OhMyAgent may also use optional metadata under `metadata.ohmyagent`:

```yaml
metadata:
  ohmyagent:
    compatibility: pi
    level: official
    tools:
      builtin: []
      custom: []
    notes: ""
```

Guidelines:

- `compatibility` should be `pi` when the skill has been adapted for the Pi
  runtime.
- `level` should be `official` for reviewed skills.
- `tools.builtin` and `tools.custom` list the tools the skill expects.
- Agent-specific fields from other runtimes may be preserved for provenance,
  but OhMyAgent only treats Pi-compatible metadata as trusted.

## Compatibility Policy

Before adding a skill here, check whether it depends on runtime-specific tools.
For example, Claude Code skills may include frontmatter such as:

```yaml
allowed-tools:
  - Read
  - Write
  - Edit
  - AskUserQuestion
```

OhMyAgent should not assume those tools exist. If a skill needs interactive
questions, file edits, web search, or other runtime capabilities, adapt the
instructions to OhMyAgent/Pi tools or document the limitation in
`metadata.ohmyagent.notes`.

## Importing

OhMyAgent's Settings -> Skills import flow should use this repository as the
default official source. Third-party GitHub repositories can still be imported,
but they should be shown as community/unverified until compatibility is checked.
