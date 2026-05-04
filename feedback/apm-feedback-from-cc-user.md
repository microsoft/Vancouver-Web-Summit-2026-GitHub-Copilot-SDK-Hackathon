# APM feedback: ocumentation gaps affecting Claude Code users

After authoring four APM kits for an autonomous-agent project and reading
the official docs at `https://microsoft.github.io/apm/` end to end, here
is what worked well plus two documentation gaps that hit a Claude Code
user. Many developers use Claude Code, and `target: claude` is one of
APM's named compile targets, so closing these gaps would help a wide
audience. The same gaps probably affect the other named targets too.
Claude Code is just the easiest place to make them concrete.

For the path-customization angle of the same surface, see
`microsoft/apm#891` ([FEATURE] Allow customizing skill integration target
directory).

## What worked well

A couple of pure-APM things that genuinely worked well in practice.

The self-defined MCP entries (`registry: false` under
`dependencies.mcp[]`) cover stdio commands, HTTP urls, env, headers,
version, and tools allow-list inline in `apm.yml`. That is the right
abstraction: a Claude Code user can compose a multi-MCP setup in one
manifest without touching an external registry.

The lockfile is also well-shaped. `apm.lock.yaml` captures `content_hash`
and `resolved_commit`, which is enough for reproducible installs without
inventing a parallel package registry, and `apm install` against an
existing lockfile being idempotent is the right default.

## 1. The reference page and the quick-start disagree about what `target: claude` emits

The manifest schema's compile-targets table at section 3.6 lists every
named target and what each one emits. For most targets the row spans
multiple deployment paths. `cursor` is listed as emitting to
`.cursor/rules/`, `.cursor/agents/`, and `.cursor/skills/`. `gemini`
emits `GEMINI.md` plus deployments under `.gemini/commands/`,
`.gemini/skills/`, and `.gemini/settings.json`. `windsurf` lists five
distinct paths.

The row for `claude`, in the same table, lists only `CLAUDE.md` at the
project root.

The quick-start page tells a different story. Under what to commit, it
lists `.claude/` deployed files (`agents/`, `commands/`, `skills/`,
`hooks/`) and notes the same rationale applies for Claude Code users.
Issue `microsoft/apm#891` confirms `.claude/skills/<name>/SKILL.md` is
also produced today. So in practice `target: claude` deploys to
`.claude/agents/`, `.claude/commands/`, `.claude/skills/`, and
`.claude/hooks/` like the other named targets, but the reference page
does not say so.

This is a docs gap rather than a tool gap. A Claude Code user who lands
on the manifest-schema reference will not learn that sub-agents, slash
commands, skills, or hooks are deployed at all, and will not learn what
frontmatter mapping is applied to each.

It would help a lot to bring the section 3.6 row for `claude` up to
parity with the other targets, listing all `.claude/<dir>/` deployment
paths and pointing at a per-primitive frontmatter-mapping reference.

## 2. The variable-interpolation matrix omits most named targets

The manifest schema's variable-interpolation table at 4.2.4 has four
columns total: Syntax, Source, VS Code, and Copilot CLI / Codex. Only
the last two are target columns. For `${input:<id>}` the Copilot CLI /
Codex cell says the syntax is not supported and points users to
`${VAR}` or `${env:VAR}` instead.

There is no row for `target: claude`, `target: cursor`, `target: gemini`,
`target: opencode`, or `target: windsurf`. So a kit author writing a
manifest with, say, `target: [claude, copilot, cursor]` cannot tell from
this table what happens to `${input:<id>}` on the Claude or Cursor
outputs.

The spec's own recommendation, just below the table, is to use `${VAR}`
or `${env:VAR}` in all new manifests because they work on every target
that supports remote MCP servers. That clause implies some named targets
do not support remote MCP servers at all, but the table does not say
which ones.

The fix is small: add columns for every value listed in 3.6's `target`
enum (`vscode`, `agents`, `copilot`, `claude`, `cursor`, `opencode`,
`codex`, `gemini`, `windsurf`). Where a runtime has no concept of
interactive prompts or remote MCP servers, mark the cell "not applicable",
but mark it.



