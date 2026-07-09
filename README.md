# seedance-shotlist-director

Claude Code skill: turns a script / scene / treatment into a director's shotlist — a single self-contained HTML production board with copy-ready **Seedance 2.0** prompts.

Based on the skill shipped by [Higgsfield AI](https://higgsfield.ai) in the tutorial ["3-Step Workflow To Make Ultra-Realistic AI Ads"](https://www.youtube.com/watch?v=3rDs6FhFoUQ), heavily extended.

## History

- **Commit 1** — the original Higgsfield `SKILL.md`, verbatim (only the claude.ai output paths adapted for local Claude Code).
- **Commit 2** — the improved version, driven by a user-pain review of the real production loop.

## What the improved version adds

| Area | Improvement |
|---|---|
| Consistency | `@asset` reference convention (`@hero`, `@s_hero_wet`) restored from the original Higgsfield workflow + **Asset Checklist** rendered before any generation |
| Editing | **ENDS ON handoff frames** — every prompt declares its exact final frame so clips cut together; match-cut design between scenes |
| Credits | **Risk badges** (🟢🟡🔴) with attempt estimates, "generate high-risk first" strategy, **final-cut targets** (15s gen → ~3s final) and a runtime/budget summary |
| Iteration | **Repair Guide** in the HTML: symptom → fix table for failed generations, "change one thing per retry" rule |
| Tracking | Binary checkbox → per-prompt **status** (⬜/🔄/⚠️/✅), **keeper timecode** and **notes** fields, all persisted |
| Sessions | **Project Bible** — embedded JSON (characters, assets, style, scene map) so a fresh Claude session restores full context from the file alone |
| Bugs | localStorage keys namespaced by project slug (no state bleed between shotlists); clipboard fallback (no silent copy failures); HTML-entity escaping in prompts |
| Style | Style prefix split into fixed CORE + **per-scene lighting** (the original hard-coded contre-jour globally); dialogue rules fixed (diegetic dialogue explicitly allowed and directed) |
| UX | Scene descriptions in the user's language (prompts stay English, as Seedance requires) |

## Install (Claude Code)

Put `SKILL.md` into a folder named `seedance-shotlist-director` under your skills directory:

```
~/.claude/skills/seedance-shotlist-director/SKILL.md
```

Then ask Claude: *"сделай шотлист"* / *"make a shotlist"* with your script.
