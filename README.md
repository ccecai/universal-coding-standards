# Universal Coding Standards

[English](README.md) | [简体中文](README.zh-CN.md)

Reusable coding conventions for Codex and Cursor across projects. The actual
instructions stay in English in [SKILL.md](SKILL.md); responses follow the user's
requested language. These are defaults, with explicit user requirements and
applicable project rules taking precedence.

## What it covers

- Focused changes, source ownership, module boundaries, and platform differences.
- Shared state, concurrency, input checks, resource lifetime, and critical actions.
- Initialization, validation gates, naming, constants, logging, and documentation.
- Compiler warnings, builds, final linkage checks, validation evidence, and commits.
- C/C++ headers, declarations, function complexity, macros, control flow, and data safety.

C/C++ and hardware rules apply only where relevant. The skill does not grant
tools, repository access, permission to commit, or permission to publish.

## Required companion skills

Install your trusted copies of **Ponytail** and **Caveman** next to this skill:

```text
~/.agents/skills/
├── universal-coding-standards/
│   ├── SKILL.md
│   └── agents/openai.yaml
├── ponytail/SKILL.md
└── caveman/SKILL.md
```

Ponytail owns implementation minimalism, root-cause analysis, necessary safeguards,
and the runnable-check baseline; its default mode is `full`. Caveman owns concise,
complete communication. This repository supplies the remaining engineering rules.
The two companion skills are required and are not bundled or installed by this repository.

## Install

The following commands require Git and a POSIX shell. Because this repository is
private, authenticate with an account that has access. Do not place tokens in clone
URLs. If the destination exists, inspect and back it up instead of overwriting it.

### Shared personal installation for Codex and Cursor

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/ccecai/universal-coding-standards.git \
  "$HOME/.agents/skills/universal-coding-standards"
```

Place Ponytail and Caveman in the sibling folders shown above before use. Current
Codex and Cursor documentation lists `~/.agents/skills` for user-level discovery.
If the skill does not appear, start a new session or restart the client. Local
installation does not install it into remote workers or a ChatGPT account.

For an existing installation under `~/.codex/skills`, keep using its actual path
instead of cloning a duplicate. Cursor also supports `~/.cursor/skills`; keep all
three skills together if using that location. For a project-only installation,
place the three folders under that project's `.agents/skills/`.

## Make it a default for every coding task

Discovery and permission for automatic selection do not guarantee use on every
task. In Codex, merge the following into the existing global `~/.codex/AGENTS.md`;
do not replace other instructions. If `CODEX_HOME` is customized, edit its guidance
file instead. An active `AGENTS.override.md` takes precedence at that level.

```markdown
For every task that writes, changes, fixes, refactors, reviews, or designs code,
read and follow these skills unless already loaded:
- ~/.agents/skills/universal-coding-standards/SKILL.md
- ~/.agents/skills/ponytail/SKILL.md
- ~/.agents/skills/caveman/SKILL.md
Default to Ponytail full mode. Higher-priority instructions, explicit user
requirements, and applicable project rules take precedence over these defaults.
```

Adjust the paths to the real installation.

### Cursor: enable across all projects

Open **Customize → Rules**, add or edit a **User Rule**, and merge the same
instruction above into it. User Rules apply to Agent across projects; a project
rule or project `AGENTS.md` alone does not enable a global default. Preserve other
user rules. Add this line for explicit cleanup requests:

```text
When I ask to "规范代码", "规范", "规范一下", or "按规范整理" in a coding task,
execute the Code normalization requests workflow in universal-coding-standards.
```

Cursor can also discover an existing `~/.codex/skills` installation. Reference its
actual files in the User Rule instead of creating a second copy. Confirm the rule
is listed under **User** and the skill appears under **Skills** in Customize.
These are local Agent defaults; remote environments need their own setup.

## Use it

After discovery, the skill is eligible for selection for coding, fixes, refactoring,
design, reviews, and dependency decisions. In Codex, invoke it explicitly with:

```text
$universal-coding-standards Apply the shared standards to this change.
```

In Cursor, type `/` and select `universal-coding-standards` from the command picker.
To check the setup, ask the agent to list its active global and project rules and
confirm that both companion files are readable. This skill adds no separate approval workflow.

### Normalize changed code

```text
规范代码
规范
规范一下这次改动的代码
按规范整理 src/example.c，保持功能不变
```

These coding requests execute corrections, not just a checklist explanation. A bare
`规范` in an active coding context also starts normalization. An explicit target
takes precedence; otherwise the agent checks staged, unstaged, and relevant new code,
then fixes changed lines and necessary surrounding code. It follows project tools
and conventions, preserves behavior and existing edits, and reports actual checks.
No changed code and no target means it asks for a target, not a repository-wide cleanup.
It does not stage or commit those edits. Quoted examples, negative requests, questions
about standards, and requests to edit this skill do not authorize code normalization.

## Files and updates

- `SKILL.md`: English instructions and applicability.
- `agents/openai.yaml`: display metadata and `allow_implicit_invocation: true`.
- `README.md` / `README.zh-CN.md`: English and Chinese installation and usage.
- `.gitignore`: local environment, credential-file, and cache exclusions.

For a clean installation cloned from this repository, run `git pull --ff-only` in
its directory to update. If local edits or divergent history prevent the update,
inspect the cause; do not reset or overwrite them. Update companions separately.

## Related skills and references

- [Code Review Skill](https://github.com/ccecai/code-review-skill)
- [Feature & Bug Fix Skill](https://github.com/ccecai/feature-bugfix-skill)
- [Codex skills documentation](https://learn.chatgpt.com/docs/build-skills)
- [Codex global and project guidance](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Cursor skills documentation](https://cursor.com/docs/skills)

This is an independent skill, not an official OpenAI or Cursor product.
