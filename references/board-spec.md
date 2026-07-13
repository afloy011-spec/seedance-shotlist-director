# Board spec — HTML mechanics for shotlist.html

Read this file together with `html-template.html` every time you generate or re-generate the board. The template's CSS and JS are copied **verbatim** — you fill placeholders and assemble `{{SCENES_HTML}}` from the block pattern below. Do not rewrite or "improve" the JS: class names, data attributes, and localStorage keys are a contract with the user's saved state and with `scripts/validate.mjs`.

## Board structure (top to bottom)

1. **Title bar** — project name + **runtime summary** (`{{RUNTIME_SUMMARY}}`): target final-cut length · prompt count · total generation seconds. Example: "Target ad: ~30s · 6 prompts · 90s of generation". The **Export edits** button sits in the topbar right of the summary — never inside the help text.
2. **How to use** — a collapsed `<details class="top-block howto">`: a `<ul>` list (one topic per `<li>`, bold lead word) plus a `.kbd-row` of `<kbd>` chips. Never a wall of prose. Written in the user's language.
3. **Asset Checklist** — `<details class="top-block" open>`: every asset to build in Higgsfield BEFORE generating. Each asset is an `.asset-item` block: a head row (@name in monospace accent + a one-line build note in the user's language + a Copy button) and a `<pre class="asset-prompt">` with the copy-ready English generation prompt (patterns in `asset-prompts.md`):

   ```html
   <div class="asset-list">
     <div class="asset-item">
       <div class="asset-head"><b>@vera</b><span>героиня — сплит-лист (вариант A)</span>
         <span class="prompt-actions">
           <span class="edited-badge" data-asset-id="vera-sheet" hidden>edited</span>
           <button class="tool-btn reset-btn" data-asset-id="vera-sheet" hidden title="Вернуть исходный текст">Reset</button>
           <button class="tool-btn lang-btn" title="Показать перевод (Ctrl+Shift+L)" aria-pressed="false">RU</button>
           <button class="copy-btn" title="Скопировать промпт генерации ассета (Ctrl+Shift+C)">Copy</button>
         </span>
       </div>
       <pre class="asset-prompt" lang="en" data-asset-id="vera-sheet">Split-frame character sheet, plain solid grey background. LEFT panel: ...</pre>
       <div class="mirror-stale" data-asset-id="vera-sheet" hidden>Промпт был отредактирован — перевод соответствует исходной версии.</div>
       <pre class="prompt-mirror" hidden>[перевод на язык пользователя]</pre>
     </div>
     ...
   </div>
   ```

   Asset prompts are plain generation prompts — no Style CORE, no status row. They ARE first-class citizens of the editing machinery: `data-asset-id` (unique, kebab, no @), click-to-edit with Reset, a language mirror when the user's language isn't English, inclusion in Export edits (`=== Asset vera-sheet (edited) ===`). **Characters ship two asset-items each**: variant A — split-frame sheet, variant B — Higgsfield AI Cast card (ids `{name}-sheet` / `{name}-cast`, patterns in `asset-prompts.md`).
4. **Style Prefix (core)** — collapsible `<pre>`.
5. **Repair Guide** — collapsible symptom→fix table (content below).
6. **Scene list** — pattern below.
7. **Project Bible** — `<script type="application/json" id="project-bible">` at the end of `<body>` (schema below).

## Placeholders

| Placeholder | Content |
|---|---|
| `{{PROJECT_TITLE}}` | Project name, inferred from the script ("Untitled" if unclear) |
| `{{SLUG}}` | Short kebab slug from the title, e.g. `headphones-ad`. NEVER changes across revisions — it namespaces all saved state |
| `{{USER_LANG}}` | `<html lang>` = the user's language code (`ru`, `en`, …) |
| `{{RUNTIME_SUMMARY}}` | Runtime summary line (see above), arithmetically consistent with the prompt labels |
| `{{ASSET_CHECKLIST_HTML}}` | `.asset-list` of `.asset-item` blocks: head note in the user's language, generation prompt in English (see Board structure item 3) |
| `{{STYLE_PREFIX_TEXT}}` | The style CORE text (English) |
| `{{REPAIR_GUIDE_HTML}}` | `<table>` with the symptom→fix rows below, translated to the user's language |
| `{{SCENES_HTML}}` | Scene blocks (pattern below) |
| `{{PROJECT_BIBLE_JSON}}` | Valid JSON per the schema below; escape any `</script>` sequence as `<\/script>` |

**UI language:** the template ships its howto list, button `title` tooltips, status `<option>` labels, and the Export-edits JS strings (`'Нет правок'`, `'Скопировано: '`, `'Ошибка копирования'`, the payload header) in Russian. If the user's language is not Russian, translate those strings — and only those — when filling the template. Everything else in the JS stays byte-identical.

## Scene block pattern

```html
<div class="scene">
  <div class="scene-header">
    <input type="checkbox" data-scene="3">
    <div class="scene-num">3.</div>
    <div class="scene-desc">Анна выясняет отношения с Марко на кухне — первая трещина. <em>Match-cut in: дверь спальни → дверца холодильника.</em></div>
  </div>

  <div class="prompt-block">
    <div class="prompt-label">
      <span>Prompt 3a · 15s → ~4s final</span>
      <span class="badge risk-mid">tricky — two-person blocking</span>
      <span class="prompt-actions">
        <span class="edited-badge" data-prompt-id="3a" hidden>edited</span>
        <button class="tool-btn reset-btn" data-prompt-id="3a" hidden title="Вернуть исходный сгенерированный текст">Reset</button>
        <button class="tool-btn lang-btn" title="Показать перевод (Ctrl+Shift+L)" aria-pressed="false">RU</button>
        <button class="copy-btn" title="Скопировать промпт для Seedance (Ctrl+Shift+C)">Copy</button>
      </span>
    </div>
    <pre class="prompt" lang="en" data-prompt-id="3a">[FULL PROMPT — Style CORE, Lighting, Characters (@refs), Scene, CUTs, ENDS ON, SFX]</pre>
    <div class="mirror-stale" data-prompt-id="3a" hidden>Промпт был отредактирован — перевод ниже соответствует исходной версии. Нажми Export edits и попроси Claude обновить файл.</div>
    <pre class="prompt-mirror" hidden>[Полное зеркало промпта на языке пользователя — только для чтения; реплики остаются на английском с переводом в «…»]</pre>
    <div class="prod-row">
      <select data-prompt-field="status" data-prompt-id="3a">
        <option value="">не начат</option><option value="gen">генерится</option>
        <option value="retry">ретрай</option><option value="keeper">кипер</option>
      </select>
      <input class="keeper" data-prompt-field="keeper" data-prompt-id="3a" placeholder="keeper 0:04–0:09">
      <input class="notes" data-prompt-field="notes" data-prompt-id="3a" placeholder="notes…">
    </div>
  </div>
</div>
```

Rules:

- **One checkbox per scene**, even when split into 3a/3b/3c — `data-scene` is the scene number as a string. The user ticks scene 3 when all of 3a/3b/3c have keepers.
- **Scene descriptions in the user's language**; all prompt text English. Note designed match-cuts in the scene description (`<em>`).
- **Prompt label**: `Prompt {id} · 15s → ~{n}s final` + risk badge. Badge classes: `.risk-low` (safe) / `.risk-mid` (tricky) / `.risk-high` (high-risk) — colored text and border, **no emoji anywhere on the board**. State the risk reason in one clause inside the badge.
- **Escape `<`, `>`, `&`** in prompt text as HTML entities — the `<pre>` content must contain no raw markup.
- **Per-prompt controls** all carry `data-prompt-id`; every prompt has exactly one status `<select>`, one `.keeper` input, one `.notes` input (`data-prompt-field` = `status` / `keeper` / `notes`).
- **Language mirror**: when the user's language is not English, every prompt gets `<pre class="prompt-mirror" hidden>` with a faithful translation written at generation time (never machine-translated at runtime — the file stays offline). Dialogue lines stay English with a «…» translation in parentheses. The `.mirror-stale` notice div sits between prompt and mirror. If the user's language IS English, omit the mirror, the stale div, and the lang-btn.
- **All per-prompt buttons** (edited badge, Reset, RU/EN, Copy) sit together in `.prompt-actions` — never scattered across the label row.
- Every button has a `title` tooltip (with its shortcut where one exists).

## localStorage contract

Every key is prefixed `sd-{slug}-`:

| Key | Meaning |
|---|---|
| `sd-{slug}-scene-{n}-done` | scene checkbox (`1`/`0`) |
| `sd-{slug}-p-{promptId}-status` | status select value |
| `sd-{slug}-p-{promptId}-keeper` | keeper timecode text |
| `sd-{slug}-p-{promptId}-notes` | notes text |
| `sd-{slug}-p-{promptId}-edit` | locally edited prompt text (absence = unedited) |
| `sd-{slug}-a-{assetId}-edit` | locally edited asset generation prompt |

Keeping the slug stable across revisions is what preserves the user's saved statuses, keepers, notes, and local edits.

## Behavior shipped by the template JS (do not re-implement, just know it)

- **Copy** uses `navigator.clipboard` with a `document.execCommand('copy')` textarea fallback and visibly reports failure. Always copies the ENGLISH prompt, current edits included; ignores the mirror.
- **Editable prompts**: `pre.prompt` becomes `contenteditable="plaintext-only"` (fallback `true`); edits persist per prompt id; an "edited" badge + Reset button appear; Reset restores the generated original (captured at load, before applying saved edits). Paste is forced to plain text and drop is blocked.
- **Mirror toggle** (`.lang-btn`) swaps prompt/mirror visibility, exposes `aria-pressed`. When a local edit exists AND the mirror is visible, the `.mirror-stale` banner shows (the mirror translates the ORIGINAL text).
- **Export edits** collects every `sd-{slug}-p-*-edit` into one paste-ready `=== Prompt 1a (edited) === …` block and copies it — the bridge back to Claude for baking in and re-translating.
- **Shortcuts**: native editing shortcuts inside prompts; board-level Ctrl/Cmd+Shift+C (copy prompt under cursor/focus), Ctrl/Cmd+Shift+L (mirror toggle), Esc (blur). Listed in the howto.

## Repair Guide content

Adapt the wording to the user's language, keep the mappings:

| Symptom | Fix in the prompt |
|---|---|
| Face/identity drifts between cuts | Strengthen the Character anchor (more specific features), add/verify the @character reference, reduce cut count in this prompt |
| Product or prop floats / teleports | Add an explicit contact point ("cup ON the marble counter", "headphones sealed over ears"), mention the prop in every cut it's visible |
| Choreography turns to mush | Break movement into beat-by-beat instructions, one action per beat; consider one continuous cut instead of three |
| Plastic "AI-slop" skin / render look | Verify the full Style CORE copied (especially the Skin line); add scene-specific light-on-skin detail |
| Flat, dead acting | Add micro-beats: a breath, an eye-line shift, a swallow before the line; specify what the face does BEFORE the action |
| Wrong spatial layout / characters swap sides | Replace prose geography with a layout map reference; lock screen direction ("Anna frame-left, Marco frame-right, and they stay there") |
| Clip doesn't cut with the next one | Check ENDS ON ↔ next prompt's opening frame; regenerate the LATER clip to match, not the keeper |
| Motion too fast / rushed | The prompt is overpacked — move a cut to a new prompt (3b), let the remaining beats breathe |

One more rule for the user, stated in the guide: **change ONE thing per retry.** If a retry changes three variables, a success teaches nothing.

## Project Bible schema

Embedded as `<script type="application/json" id="project-bible">`. Purpose: a future Claude session restores full project context from the file alone. Required keys (validator-enforced): `title`, `slug`, `language`, `styleCore`, `characters`, `assets`, `scenes`. Shape:

```json
{
  "title": "TrailPro — 3-Day Solo",
  "slug": "trailpro-solo",
  "language": "ru",
  "targetRuntime": "~30s final · 12 prompts · 180s generation",
  "styleCore": "Style: 8K IMAX. ...",
  "characters": [
    { "ref": "@hero", "desc": "woman ~30, ...", "states": ["@hero_waistwet", "@hero_soaked", "@hero_dry2"],
      "genPrompt": "Split-frame character sheet, plain solid grey background. LEFT panel: ...",
      "aiCastPrompt": "Soul: 30yo athletic woman. ... (50–75 words)" }
  ],
  "assets": [
    { "ref": "@trailpro", "type": "prop", "build": "multi-angle product sheet",
      "genPrompt": "Product prop sheet / orthographic turnaround: a forest-green canvas daypack ..." },
    { "ref": "@campsite", "type": "location", "build": "¾ angle reference + overhead layout map",
      "genPrompt": "Dusk exterior, 3/4 angle with depth: a mountain campsite ..." }
  ],
  "scenes": [
    {
      "num": "1",
      "desc": "Сборы дома",
      "lighting": "Lighting: Natural light only — soft even morning daylight ...",
      "matchCutOut": "zip pull → zip pull of the tent",
      "prompts": [
        { "id": "1a", "risk": "safe", "riskWhy": "single character, slow blocking",
          "genSeconds": 15, "finalCutSeconds": 4,
          "endsOn": "her hand flat on the packed top compartment, eyes to the window" }
      ]
    }
  ],
  "bpm": null,
  "variants": []
}
```

- `bpm` + per-prompt bar plans and shipped aspect-ratio `variants` are included when relevant (see `extras.md`).
- Valid JSON, double-quoted keys, escape `</script>` as `<\/script>`.
- Prompt ids in the bible must exactly match the `data-prompt-id` set in the DOM.

## After writing the file

Run the validator if Node.js is available:

```
node <skill-dir>/scripts/validate.mjs shotlist.html
```

Fix every FAIL before presenting the board. If Node is unavailable, hand-check the blocking list in `tests/golden-scenarios.md`.
