# Agent Instructions

This repository is the official OhMyAgent skills hub. It contains curated
`SKILL.md` packages that are reviewed for the OhMyAgent web app and runtime.

Read `docs/github-workflow.md` before opening PRs.

## Mission

Only add skills that are safe and useful inside OhMyAgent. Many community skills
target Claude Code, Codex, Cursor, or other agents directly. Do not copy those
skills blindly. Adapt them so their runtime assumptions match OhMyAgent, then preserve attribution and source links.

## First Steps For Coding Agents

1. Check open work before editing:

   ```bash
   gh issue list --repo oma-home/ohmyagent-skills-hub --state open --limit 50
   ```

2. Read the target issue. It should identify the source repository, source skill
   path, adaptation goals, known incompatible fields, and acceptance criteria.
3. Inspect the source `SKILL.md` and supporting files before changing this repo.
4. Create or update files under `skills/<skill-name>/`.
5. Verify the skill can be discovered by OhMyAgent's official skills import
   flow.

If there is no issue for a requested third-party skill, create one first. The
issue is the review record for why the skill belongs in the official hub.

## Directory Layout

Use the community skill package shape:

```text
skills/
  <skill-name>/
    SKILL.md
    references/
    scripts/
    assets/
```

Only `SKILL.md` is required. Keep supporting files beside it and reference them
with relative paths from `SKILL.md`.

Do not place skills at the repository root. Do not use `.claude/skills`,
`.agents/skills`, or other agent-specific install directories in this repo.

## SKILL.md Frontmatter

Every skill must include only the standard fields by default:

```yaml
---
name: example-skill
description: A concise description of when the skill should be used.
---
```

Add optional OhMyAgent metadata only when it carries useful review information,
such as source attribution, official level, tool expectations, or adaptation
notes:

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

Rules:

- `name` must match the folder name unless there is a strong migration reason.
- `description` must explain when an agent should use the skill.
- If present, `metadata.ohmyagent.level` should be `official` only after
  adaptation.
- If present, `metadata.ohmyagent.tools` lists the OhMyAgent tools the skill
  expects.
- Preserve useful source metadata, but do not rely on runtime-specific fields
  from other agents.

## Supported Tool Vocabulary

OhMyAgent currently recognizes these runtime tools:

- `bash`
- `edit`
- `find`
- `grep`
- `ls`
- `read`
- `write`
- `web_extract`: extract a URL as markdown/text, using Jina Reader and direct
  fetch fallbacks.
- `web_search`: web search with provider fallback and source remarks for cost
  tracking.
- `subagent`: run focused helper agents for parallel source inspection,
  independent review, and scoped validation tasks.

Do not declare unsupported tool requirements such as Claude Code's
`AskUserQuestion`, `WebFetch`, `TodoWrite`, or other agent-specific tool names.
If the source skill depends on such a tool, rewrite the instructions into a
prompt-only workflow or map it to a supported OhMyAgent tool. Document any
remaining limitation in `metadata.ohmyagent.notes`.

If a source skill uses subagents, agent workers, or parallel reviewers, adapt
that behavior to OhMyAgent's `subagent` tool. Do not copy
Claude/Codex-specific subagent prompts, role names, protocols, or benchmark
harnesses as required behavior.

## Migration Checklist

When porting a community skill:

1. Record the source repository and source path in the GitHub issue.
2. Check license and attribution requirements.
3. Read all referenced files, scripts, and assets.
4. Remove or adapt unsupported frontmatter fields.
5. Replace unsupported tool assumptions with OhMyAgent-compatible behavior.
6. Keep the skill focused. Do not import unrelated skills from the same source
   repository.
7. Add `metadata.ohmyagent` and note whether the skill is prompt-only or uses
   tools.
8. Verify supporting file paths are relative and stay inside the skill folder.
9. Test discovery through OhMyAgent before closing the issue.

## Dependency Policy

Avoid adding dependencies. Skills should be prompt-only unless scripts are
clearly necessary.

If a skill needs a script:

- Keep scripts under `skills/<skill-name>/scripts/`.
- Keep generated data, caches, and temporary files out of git.
- Do not install Python dependencies globally with `pip`.
- Use `uv` for Python dependencies:

  ```bash
  uv init
  uv add <package>
  uv sync
  uv run python scripts/<script>.py
  ```

- Use `npx` only for one-off JavaScript CLI execution during development. Do not
  vendor `node_modules/` or commit generated package caches.
- If a CLI is required at runtime, document the exact command and reason in the
  skill's `metadata.ohmyagent.notes`.

## Official Import Contract

OhMyAgent discovers this repo through GitHub tree/blob APIs and imports full
skill folders into its configured skills home:

```text
OHMYAGENT_SKILLS_HOME/<skill-name>/SKILL.md
```

Default `OHMYAGENT_SKILLS_HOME` in the app is `skills/`. The app imports only
from this official hub by default. Third-party repositories should go through an
adaptation issue here before becoming available to users.

## Verification

Before committing:

```bash
find skills -name SKILL.md -print
```

For each changed skill:

- Confirm `SKILL.md` has `name` and `description`.
- Confirm no unsupported tool is declared as required.
- Confirm referenced files exist.

If you also have the OhMyAgent app checked out, verify discovery through its
Settings -> Skills -> Import flow. Do not install unreviewed third-party skills
directly into production.

## GitHub Issues

Use issues as the source of truth for migration work. A good issue includes:

- source repo URL
- source skill path
- source license/attribution notes
- unsupported tools or frontmatter fields found
- target OhMyAgent skill name
- prompt-only vs tool-using decision
- test examples or expected behavior

Close the issue only after the skill is committed, pushed, and verified.
