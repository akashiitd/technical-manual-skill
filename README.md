# Technical Manual Skill

Create dense technical manuals grounded in the actual specification and source code, with worked examples and diagrams.

Use it to learn a GitHub repository, runtime, file format, protocol, algorithm, or research topic deeply enough to inspect artifacts, trace behavior, debug failures, and understand implementation choices.

The skill asks the agent to pin source revisions, follow important code paths, distinguish requirements from implementation behavior, and explain mechanisms through concrete specimens. Diagrams support specific claims. Captures, calculations, illustrative output, and unexecuted examples are identified separately. Generic praise and filler are removed.

The [reference-manual audit](docs/reference-manual-audit.md) records a full extracted-text review of 450 pages across the Parquet, GGUF, and Pi Durable manuals, with selected visual inspection. The resulting requirements cover configuration precedence, support versus use, failure boundaries, observer semantics, typed round trips, cross-layer costs, and actionable debugging. They apply where relevant to the subject.

## Install

With the [Agent Skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add akashiitd/technical-manual-skill --skill technical-manual
```

Select your agent or agents when prompted. For a project installation targeting all five native agents:

```sh
npx skills add akashiitd/technical-manual-skill \
  --skill technical-manual \
  --agent claude-code codex pi opencode cline
```

For global installation, add `--global`. Installer locations follow the installed CLI version; if a skill does not appear, use the direct copy instructions below and check the agent's current documentation. T3 Code is configured through its selected provider, rather than a separate `--agent t3-code` target.

### Direct installation

Clone the repository and copy the **entire** `skills/technical-manual` directory, including `references/`, to one of these locations. Paths ending in `technical-manual/` are the destination directory itself.

| Agent | Project destination | User destination | Invocation |
| --- | --- | --- | --- |
| Claude Code | `.claude/skills/technical-manual/` | `~/.claude/skills/technical-manual/` | `/technical-manual` |
| Codex | `.agents/skills/technical-manual/` | `~/.agents/skills/technical-manual/` | `$technical-manual` |
| Pi | `.agents/skills/technical-manual/` | `~/.agents/skills/technical-manual/` | `/skill:technical-manual` |
| OpenCode | `.opencode/skills/technical-manual/` | `~/.config/opencode/skills/technical-manual/` | Ask it to use the `technical-manual` skill |
| Cline | `.cline/skills/technical-manual/` | `~/.cline/skills/technical-manual/` | `/technical-manual`, or request it by name |
| T3 Code | Use the selected provider's project path above | Use the selected provider's user path above | Use the provider's supported invocation, or request the skill by name |

Example for Claude Code on macOS/Linux, from the target project's root:

```sh
git clone https://github.com/akashiitd/technical-manual-skill.git
mkdir -p .claude/skills
cp -R technical-manual-skill/skills/technical-manual .claude/skills/
```

On Windows, use the same project paths with PowerShell:

```powershell
git clone https://github.com/akashiitd/technical-manual-skill.git
New-Item -ItemType Directory -Force .claude/skills | Out-Null
Copy-Item -Recurse technical-manual-skill/skills/technical-manual .claude/skills/
```

Choose a destination where `technical-manual` does not already exist. To update an existing installation, review and replace its skill directory rather than nesting another copy inside it. Install once per agent; duplicate names can create ambiguous discovery. Reload or restart the agent if needed; Pi supports `/reload`. In Cline, check that the discovered skill is enabled in the Skills tab.

### T3 Code

T3 Code runs providers installed on the machine. Install the skill for the provider and working directory used by the T3 session. With Codex, use the Codex path; with Claude Code, use the Claude path; with OpenCode, use the OpenCode path.

Provider discovery and slash-command presentation can vary by T3 Code version. If the UI does not expose a skill command, send a normal message asking the provider to load the skill, or use the explicit file-reading fallback below. This repository does not claim a separate T3-native skill loader.

## Use

For Claude Code:

```text
/technical-manual Create a detailed manual for https://github.com/owner/repo.
Use the latest stable release and pin its commit. I know Python and SQL,
but not this project's internals. Emphasize the execution path, data model,
failure recovery, configuration defaults, and extension points.
Include diagrams, reproducible examples, and exact source citations.
Save editable Markdown and diagram source; create a PDF if tools allow.
```

For Codex, replace the first token with `$technical-manual`. For Pi, use `/skill:technical-manual`. For OpenCode or any agent that selects skills by description, start with `Use the technical-manual skill to ...`.

Other useful requests:

```text
Use the technical-manual skill to explain the SQLite WAL format.
Ground it in the official format documentation and implementation.
Work through a small real database and explain recovery boundaries.
```

```text
Use the technical-manual skill to teach speculative decoding.
Ground it in the original papers and a representative implementation.
Show the derivation, numerical examples, and assumptions behind speedups.
```

```text
Use the technical-manual skill to continue the existing manual.
Read its checkpoint first. Preserve pinned versions and figure numbering,
then complete the next unfinished chapter and update verification notes.
```

```text
Use the technical-manual skill to audit this draft. Correct unsupported
claims, version mixing, missing mechanism steps, and decorative diagrams.
Preserve valid technical detail and identify unresolved evidence gaps.
```

## Any agent: explicit file-reading or plain-prompt fallback

Native skill discovery is convenient, but the workflow is ordinary Markdown. After cloning this repository, give any coding agent with file access this request:

```text
Read technical-manual-skill/skills/technical-manual/SKILL.md and its
references/manual-standard.md. Apply that workflow to [SUBJECT OR REPO].
My background is [BACKGROUND]. Emphasize [AREAS]. Write the manual with
diagrams and checkable primary-source citations in [OUTPUT DIRECTORY].
```

Adjust the clone path if needed. For an agent without file access, paste the [complete manual standard](skills/technical-manual/references/manual-standard.md) into chat and fill in its input fields. This fallback works as a prompt; it does not imply that the agent has browsing, execution, rendering, or enough context to finish a large manual in one response.

## What it produces

Depending on the request and available tools:

- An editable manual with an orientation, mechanisms, implementation traces, failure cases, practical choices, and reference material.
- Editable, numbered diagrams beside the explanations they support.
- A source manifest with pinned revisions and a claim ledger.
- Reproduction scripts, small fixtures, and captured outputs when examples are executed.
- A rendered PDF when supported, with a verification note that states what was actually checked.

The skill does not impose a page count, fabricate measurements, or claim an unexecuted example passed. A compact subject gets a compact manual; a complex subject may need multiple chapters and saved checkpoints.

## Requirements and limits

The package is instruction-only: no hooks, remote services, executable installer, bundled binaries, or required API keys. An agent needs access to the primary sources to ground its claims. Execution tools are needed for captured examples and locally measured results; rendering tools are needed for a verified PDF. Existing tools and equivalent local workflows are acceptable.

Installation paths and invocations were checked against the linked documentation on **2026-10-06**. Validation performed: the skill-creator frontmatter validator, relative-link and metadata checks, CLI skill discovery, and a copy installation targeting Claude Code, Codex, Pi, OpenCode, and Cline in an isolated temporary project. The copied instruction and reference files were checked against the originals. This verifies package structure and installer behavior; it does not establish end-to-end manual generation inside every listed application. T3 Code guidance is based on its documented provider model.

The attached reference manuals that motivated this workflow are not redistributed here. Source repositories, specifications, and quoted excerpts retain their own licenses. This repository's MIT license covers the skill package.

## Package layout

```text
skills/technical-manual/
  SKILL.md                       Portable entry point and workflow
  references/manual-standard.md  Detailed requirements and plain-prompt fallback
  agents/openai.yaml             Optional Codex UI metadata
```

## Compatibility sources

- [Agent Skills specification](https://agentskills.io/specification)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Codex skills](https://developers.openai.com/codex/skills)
- [Pi skills](https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/skills.md)
- [OpenCode skills](https://opencode.ai/docs/skills/)
- [Cline skills](https://docs.cline.bot/customization/skills)
- [T3 Code providers](https://github.com/pingdotgg/t3code)
- [Agent Skills CLI installation and supported agents](https://github.com/vercel-labs/skills)

## Contributing

For an instruction change, include a concrete request and the failure it addresses. Preserve accurate evidence labels, portable paths, and user scope. Avoid adding generic rules for hypothetical problems. Check frontmatter and relative links before submitting a change; record the agent and version for any behavioral compatibility test.

## License

[MIT](LICENSE).
