# OhMyAgent Skills Hub

Official skills for OhMyAgent.

This repository is the curated source for skills that are expected to work well
inside OhMyAgent. Community skills often target
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
---
name: example-skill
description: A concise description of when the skill should be used.
metadata:
  ohmyagent:
    level: official
    tools: []
    notes: ""
---
```

Guidelines:

- `level` should be `official` for reviewed skills.
- `tools` lists the OhMyAgent tools the skill expects.
- Agent-specific fields from other runtimes may be preserved for provenance,
  but this official hub should adapt runtime assumptions to OhMyAgent.

OhMyAgent currently recognizes these runtime tools:

- `bash`
- `edit`
- `find`
- `grep`
- `ls`
- `read`
- `write`
- `web_extract`
- `web_search`
- `subagent`

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
instructions to OhMyAgent tools or document the limitation in
`metadata.ohmyagent.notes`.

If a source skill uses agent workers or parallel review, map that behavior to
OhMyAgent's `subagent` tool. Do not preserve Claude-, Codex-, or
Cursor-specific subagent protocols as required runtime behavior.

## Importing

OhMyAgent's Settings -> Skills import flow should use this repository as the
default official source. Third-party GitHub repositories can still be imported,
but they should be shown as community/unverified until reviewed.
