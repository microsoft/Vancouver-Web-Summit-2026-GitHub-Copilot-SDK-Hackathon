# APM Feedback — Wanda (CM Labs)

**Project:** Wanda — AI Data Engineer for Microsoft Fabric
**Repo:** https://github.com/Timbermitch/wanda
**Submitter:** Matthew Arrogante

## What worked

- The `apm.yml` schema was easy to write by hand. Clear separation between metadata, runtime config, and capabilities.
- Linking to `AGENTS.md` and `mcp.json` from a single manifest is great — it keeps the agent's persona, tools, and runtime co-located.
- The capabilities and permissions sections felt natural for describing what Wanda does (read-only Fabric investigation) without overpromising.

## What was unclear

- Documentation for `apm.yml` is still light. I had to look at examples in the APM repo to figure out the expected fields.
- Wasn't obvious whether `permissions` are enforced at runtime or just declarative metadata.
- Versioning convention (`apiVersion: apm.github.com/v1`) — I copied this from examples but couldn't find a definitive reference.

## What would be useful next

- A schema definition (JSON Schema or similar) so editors can validate `apm.yml` while you write it.
- Example manifests for common agent shapes — read-only investigation agent, write-capable workflow agent, etc.
- Tighter integration with the Copilot SDK so the SDK can read `apm.yml` directly to discover MCP servers and persona.

## Overall

Useful piece of the puzzle. The single-manifest approach is the right idea — it makes agents portable and reproducible. Mostly needs more docs and tooling around the schema to feel production-ready.
