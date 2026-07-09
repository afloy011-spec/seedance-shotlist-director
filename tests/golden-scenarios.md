# Golden scenarios — regression tests for SKILL.md changes

Any edit to `SKILL.md` gets re-run against these three scenarios. Generate a shotlist for each, then walk the checklist below. A regression on a **blocking** criterion means the skill change is rejected.

---

## Scenario G1 — smoke (single scene, minimal)

> Ночь, круглосуточная прачечная. Пожилой мужчина смотрит, как крутится барабан. Заходит промокший курьер, садится рядом. Они не знакомы. Мужчина молча протягивает ему термос. Курьер колеблется, потом берёт.

**Stresses:** minimal input (does the director invent blocking, light, micro-acting?), a dialogue-free two-person emotional beat, one prompt with full 15s of air, contre-jour trap (night interior — default backlight must be replaced with laundromat practicals... which are ARTIFICIAL: the "natural light only" line must be adapted, not copy-pasted).

**Expected shape:** 1 scene, 1–2 prompts, 🟢/🟡 risk, wet-state asset for the courier (@courier_wet), thermos as hero prop with contact points.

## Scenario G2 — continuity marathon (8 scenes, state decay)

> Реклама рюкзака TrailPro. Героиня идёт соло-поход на 3 дня: сцена сборов дома; рассветный старт у трейлхеда; полуденный брод через реку (она промокает до пояса); привал с грозой (промокает целиком, ставит тент); утро 2-го дня — сохнет у костра, куртка на ветке; перевал в тумане; вершина на закате — достаёт из сухого отделения рюкзака сухую толстовку; финальный кадр — рюкзак у палатки под звёздами.

**Stresses:** 8 scenes, character state machine (dry → waist-wet → soaked → drying → dry-in-new-layer), each state = @asset variant; product in every scene without drift; lighting design across dawn/noon/storm/fog/sunset/night (six different Lighting lines — any two identical = fail); handoff chain across 10+ prompts; layout map for the campsite scene.

**Expected shape:** 8 scenes, 10–14 prompts, at least 4 named state variants of the heroine, ENDS ON chain unbroken, campsite layout map in the Asset Checklist.

## Scenario G3 — dialogue and rhythm shift (drama)

> Короткометражка. Автомастерская, вечер. Механик (женщина ~50, хозяйка мастерской) закрывает смену. Приходит её взрослый сын — впервые за два года. Долгий неловкий разговор: он просит денег, она отказывает, он взрывается, она остаётся спокойной и говорит: «Я тебе не банкомат. Я твоя мать. Заходи, когда поймёшь разницу». Он уходит. Она стоит одна, потом возвращается к машине, над которой работала.

**Stresses:** heavy dialogue (quoted lines with delivery direction, diegetic audio spec), rhythm shift inside one location (slow awkwardness → explosion → still aftermath — prompt splits must follow the DRAMA, not equal 15s arithmetic: the confrontation compressed, the aftermath given air), emotional carry between prompts, restraint rule (her calm > his shouting), single-location continuity (same tools, same car, same light decaying as evening deepens).

**Expected shape:** 1 location, 3–5 scenes/beats, the mother's key line gets its own prompt, aftermath prompt is a long held shot, 🟡 dominant risk profile.

---

## Criteria checklist

Blocking (any FAIL rejects the change):

- [ ] B1. Every character/prop/location referenced with @names; state variants have own @names
- [ ] B2. Every prompt ends with ENDS ON; consecutive prompts chain (ENDS ON N == opening frame N+1)
- [ ] B3. Lighting line designed per scene; no two scenes with different time-of-day/mood share one verbatim
- [ ] B4. Prompts are English regardless of input language; scene descriptions in the user's language
- [ ] B5. Copy-block is standalone (CORE + Lighting + Characters + Scene + CUTs + ENDS ON + SFX)
- [ ] B6. localStorage keys namespaced by slug; Project Bible JSON parses (`ConvertFrom-Json` / `JSON.parse`)
- [ ] B7. One checkbox per scene, statuses per prompt id
- [ ] B8. No abstract emotion without physical action ("she is sad" → fail); no unmotivated camera
- [ ] B9. Dialogue lines quoted with delivery direction; audio spec permits diegetic dialogue
- [ ] B10. G2 only: state machine correct in every prompt (wet where wet, dry where dry, no teleporting states)

Scored (grade 1–10, target ≥8 each):

- [ ] S1. Dramatic split — prompt boundaries follow beats, not arithmetic (G3 is the probe)
- [ ] S2. Risk badges plausible, with reasons; 🔴 generated-first advice present
- [ ] S3. Final-cut targets and runtime summary present and arithmetically consistent
- [ ] S4. Asset Checklist complete — nothing referenced in prompts is missing from it
- [ ] S5. Repair Guide present with symptom→fix mappings intact
- [ ] S6. Layout map described in the Asset Checklist where staging is complex (G2 campsite; G3 optional)
- [ ] S7. Match-cuts designed between scenes where the material allows
- [ ] S8. Center-safe composition noted in CUT framing (reframe-ready for 9:16/1:1)
- [ ] S9. Prompts are click-to-edit with persisted edits + Reset; non-English user gets a read-only language mirror per prompt (RU/EN toggle), Copy always copies English

Revision probe (run after any golden, using its output):

> «Разбей сцену X на две» — verify: scene numbers stable, slug unchanged, only the touched scene regenerated, handoff chain re-stitched, neighboring scenes byte-identical.
