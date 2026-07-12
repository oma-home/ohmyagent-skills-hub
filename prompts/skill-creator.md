# Skill Creator Prompts

Use these prompts to quickly create or adapt OhMyAgent skills. They distill the useful parts of Anthropic's `skill-creator` workflow without carrying over Claude/Codex-specific runtime assumptions, eval viewers, or benchmark loops.

## Core Principles

- Write for OhMyAgent, not for Claude, Codex, Cursor, or another agent runtime.
- Put trigger logic in `description`; the body is loaded only after the skill triggers.
- Keep `SKILL.md` lean. Move long details to `references/`, deterministic helpers to `scripts/`, and output templates to `assets/`.
- Declare only supported tools:
  - `bash`, `edit`, `find`, `grep`, `ls`, `read`, `write`
  - `web_extract`, `web_search`, `subagent`
- OhMyAgent supports a subagent tool. Use subagents directly for parallel source inspection, independent review, and quick forward checks when that improves quality. Do not copy Claude/Codex-specific subagent prompts, role names, or benchmark harnesses.
- If a source skill depends on unsupported tools, rewrite the behavior as prompt-only guidance or document the limitation in `metadata.ohmyagent.notes`.
- Do not add heavy evaluation, benchmark, browser, or platform-CLI requirements unless the user explicitly asks for that infrastructure.

## Prompt: Create A New Skill

```text
You are creating an official OhMyAgent skill.

Goal:
Create a concise, OhMyAgent skill package under `skills/<skill-name>/`.

First infer the missing details from the user's request. Ask at most one short clarifying question only if the answer materially changes the skill's scope.

Design the skill using this package shape:

skills/
  <skill-name>/
    SKILL.md
    references/
    scripts/
    assets/

Only `SKILL.md` is required. Add `references/`, `scripts/`, or `assets/` only when they remove real complexity or preserve necessary reusable material.

Use minimal frontmatter by default:

---
name: <skill-name>
description: <what this skill does and when an agent should use it>
---

If optional metadata is useful for source attribution, official review state, tool expectations, or adaptation notes, use an expanded frontmatter block:

---
name: <skill-name>
description: <what this skill does and when an agent should use it>
metadata:
  source:
    repository: <source repo, if migrated>
    path: <source path, if migrated>
    url: <source URL, if migrated>
    license: <source license, if known>
  ohmyagent:
    level: official
    tools: []
    notes: ""
---

Rules:
- `name` must match the folder name.
- `description` must include trigger contexts, not just a summary.
- If metadata declares tools, tool names must come only from `bash`, `edit`, `find`, `grep`, `ls`, `read`, `write`, `web_extract`, `web_search`, and `subagent`.
- Use OhMyAgent subagents when useful to inspect source files, critique the draft skill, or check likely trigger/near-miss prompts. Keep subagent prompts short and task-local.
- Keep the body focused on purpose, operating mode, workflow, resource navigation, validation, and safety boundaries.
- Do not include unsupported or foreign runtime assumptions such as Claude-specific subagent roles, Codex-only tools, browser viewers, hidden task lists, or global dependency installs.

Before finishing, verify:
- `find skills -name SKILL.md -print` discovers the new skill.
- Any referenced files exist and are relative to the skill folder.
- No dependency folders, caches, generated files, or temporary files are included.
```

## Prompt: Adapt A Third-Party Skill

```text
You are adapting a third-party skill into the official OhMyAgent skills hub.

Follow the repository rules:
1. Check open issues first.
2. If no issue exists for this source skill, create one before editing.
3. Read the source `SKILL.md` and every referenced file needed for the adaptation.
4. Preserve source repository, source path, source URL, and license metadata.
5. Adapt the skill for OhMyAgent instead of copying platform-specific behavior.

For each source assumption, decide:
- Keep: compatible with OhMyAgent and useful.
- Rewrite: useful behavior but names/tools/runtime assumptions are wrong.
- Omit: tied to another platform, too heavy, unsafe, or unnecessary.
- Document: limitation remains and belongs in `metadata.ohmyagent.notes`.

Use only supported tool declarations:
`bash`, `edit`, `find`, `grep`, `ls`, `read`, `write`, `web_extract`, `web_search`, and `subagent`.

Use OhMyAgent subagents directly when they help with adaptation, especially to:
- inspect different source subdirectories in parallel,
- review whether the adapted skill still contains foreign runtime assumptions,
- check whether the `description` is likely to trigger on intended prompts and avoid near-misses.

Do not preserve unsupported required tools such as `AskUserQuestion`, `WebFetch`, `TodoWrite`, Claude/Codex-specific subagent protocols, eval viewers, browser opening, or platform CLIs. If the workflow benefits from those concepts, rewrite them as OhMyAgent-native prompt guidance or subagent review steps.

Output an OhMyAgent skill under `skills/<skill-name>/` with concise instructions. Include OhMyAgent metadata only when it records useful source, tool, or adaptation information.
```

## Prompt: Draft The SKILL.md Body

```text
Draft the body of `SKILL.md` for an OhMyAgent skill.

Use this structure when it fits:

# <Skill Title>

## Purpose
State what the skill helps the agent do and the boundaries of the task.

## Operating Mode
Explain the default behavior: prompt-only, tool-using, script-assisted, web-assisted, etc.

## Workflow
List the main steps the agent should follow. Keep the steps concrete and short.

## Resource Navigation
Tell the agent when to read files in `references/`, run scripts in `scripts/`, or use assets in `assets/`. Do not mention directories that do not exist.

## OhMyAgent Compatibility
State any required OhMyAgent tools. Make unsupported capabilities optional or out of scope.

## Validation
List quick checks for this skill, focused on correctness and repository hygiene. For non-trivial skills, include one lightweight subagent review pass: ask a subagent to read the skill as a fresh reviewer and report OhMyAgent compatibility issues, missing files, and unclear trigger boundaries.

## Safety And Scope
State what the skill should not do, especially around credentials, network access, destructive actions, unsupported tools, or broad claims.

Style requirements:
- Be concise and imperative.
- Explain important constraints, but avoid generic filler.
- Do not add evaluation or benchmark infrastructure unless requested.
- Use subagents for review when helpful, but keep the skill itself usable without a custom benchmark harness.
- If the user asks in Chinese, write the skill in Chinese only if that matches the expected users; otherwise keep repo-facing docs in English.
```

## Prompt: Improve A Skill Description

```text
Improve this skill's `description` field for OhMyAgent triggering.

Input:
<paste current name, description, and a short summary of the skill body>

Return:
1. A revised description under 1024 characters.
2. A short explanation of what trigger contexts were added or narrowed.
3. Two prompts that should trigger the skill.
4. Two near-miss prompts that should not trigger the skill.

Rules:
- The description must say what the skill does and when to use it.
- Put trigger contexts in the description, not only in the body.
- Do not overbroaden the skill.
- Avoid platform-specific names unless this skill is explicitly for that platform.
```

## Prompt: OhMyAgent Compatibility Review

```text
Review this skill for OhMyAgent compatibility.

Check:
- `name` matches the folder name.
- `description` explains when to use the skill.
- If `metadata.ohmyagent.level` is present, it is `official` only after review.
- If tools are declared, they use only supported names:
  - `bash`, `edit`, `find`, `grep`, `ls`, `read`, `write`
  - `web_extract`, `web_search`, `subagent`
- Referenced files exist and are relative to the skill folder.
- Source metadata and license are preserved for migrated skills.
- Unsupported platform assumptions are removed, optionalized, or documented.
- Any subagent guidance is OhMyAgent-native and does not depend on Claude/Codex-specific subagent protocols.
- No `node_modules/`, caches, generated outputs, `.DS_Store`, or temporary files are included.

Return:
- Blocking issues
- Non-blocking improvements
- A corrected frontmatter block if needed
- The exact verification commands to run
- A short subagent review prompt if an independent pass would be useful
```

## Prompt: Subagent Review Pass

```text
You are reviewing an OhMyAgent skill draft as an independent reviewer.

Inputs:
- Skill folder or `SKILL.md` content
- Any referenced files that the main agent provides

Review for:
- Clear trigger description: what should trigger and what should not.
- OhMyAgent compatibility: no unsupported tool declarations or foreign runtime assumptions.
- Package hygiene: referenced files exist, paths are relative, no generated caches or dependency folders.
- Skill usefulness: the workflow is concrete enough to improve agent behavior.
- Scope control: the skill is not too broad, unsafe, or misleading.

Return:
1. Blocking issues.
2. Non-blocking improvements.
3. Any suspected over-trigger or under-trigger cases.
4. One concise recommendation: accept, revise, or reject.

Do not rewrite the whole skill unless asked. Focus on actionable review findings.
```

## Minimal Skill Template

```markdown
---
name: example-skill
description: Use when the user needs <task/context>. Helps OhMyAgent <specific capability> by following <workflow/domain>.
metadata:
  ohmyagent:
    level: official
    tools: []
    notes: "Prompt-only skill."
---

# Example Skill

## Purpose

Help OhMyAgent <do the task> in a repeatable way.

## Workflow

1. Clarify the input only when required.
2. Follow <domain-specific steps>.
3. Produce <expected output>.
4. Verify <basic correctness checks>.

## Boundaries

- Do not assume unsupported tools.
- Do not install global dependencies.
- Do not perform destructive or credential-sensitive actions unless the user explicitly requests and approves them.
```
