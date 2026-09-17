# Strategic Indicator Console

A single-file, offline-capable console for tracking geopolitical indicators and running
conditional analysis against them. No build step, no dependencies, no network calls.

**Live:** enable GitHub Pages (see below) and it serves from the repository root.

---

## What it does

112 indicators across 12 theatres. Each carries a **trigger threshold** — a specific,
falsifiable condition — rather than a vague topic. You mark each as Dormant, Watch or
Triggered, and the engine reports what the combination implies.

Output is three things:

1. **Completed correlation patterns.** 13 named syndromes that fire only when specific
   combinations are met. The correlation is the finding, not the individual triggers.
2. **"One trigger away."** Patterns with all but one condition satisfied, naming the
   specific indicators that would complete them. This is the collection-tasking output.
3. **Ranked watch-next**, weighted by pattern-completion value, downstream chains, and
   axis saturation — plus best / most likely / worst case narrative.

State (statuses and notes) lives in the browser's `localStorage`. It is never transmitted.
Use Export / Import to move state between devices or hand it to another analyst.

---

## Setup on GitHub Pages

1. Create a repository. Public is fine; the tracking data is not in the repo.
2. Upload `index.html` (and this README). The filename matters — Pages serves
   `index.html` at the bare URL.
3. Settings → Pages → Source: **Deploy from a branch** → branch `main`, folder `/ (root)`.
4. Wait ~60 seconds. The URL appears at the top of that same settings page:
   `https://<username>.github.io/<repo>/`
5. On iOS, open in Safari → Share → **Add to Home Screen** for a standalone app icon.

To edit afterwards: open `index.html` in GitHub, click the pencil, commit. The live site
rebuilds in about 30 seconds, and you get version history on every change to the framework.

**Note on iOS:** opening the file locally from the Files app will *not* work. iOS Safari
does not execute JavaScript in `file://` documents. It must be hosted. On macOS, Windows
and Linux it opens directly from disk with no server.

---

## The model

### Pressure axes

Every indicator carries weights onto one or more of nine axes. Triggered counts full
weight; Watch counts 40%.

| Axis | Meaning |
|---|---|
| `fiscal` | US fiscal and monetary credibility |
| `choke` | Maritime chokepoint security |
| `alliance` | Alliance cohesion and extended deterrence |
| `prolif` | Proliferation pressure |
| `fragment` | State fragmentation risk |
| `resource` | Resource and minerals contest |
| `blocs` | Monetary and trade bloc realignment |
| `water` | Water and climate conflict |
| `escal` | Great-power escalation |

An axis above 80% of the highest-scoring axis is treated as *saturated* and adds an
escalation clause to the narrative.

### Syndromes

A syndrome is defined as an array of OR-sets. Every set must contain at least one
**triggered** indicator for the pattern to fire. If all but one set is satisfied — or all
are satisfied counting Watch — it appears under "One trigger away."

The 13 patterns: Safe-haven break · Two-chokepoint squeeze · Extended-deterrence collapse ·
Northern flank hollowing · Deniable probe sequence · Sanctions overreach to rails exit ·
Water casus belli · Nile basin cascade · Chinese succession contest · Iranian fracture in
progress · Sahel state failure · Hemispheric drift · Resource access lock-out.

### Theatres

US fiscal & monetary (12) · US trade & sanctions (9) · Middle East chokepoints (12) ·
Iran internal (8) · Europe & NATO (12) · Russia, Arctic & GIUK (9) · UK constitutional (6) ·
Indo-Pacific (12) · China internal (9) · South Asia & water (10) · Africa (12) ·
South America (10) · Global systemic (8) · Bloc realignment (6)

---

## Editing the framework

Everything is plain JavaScript objects near the top of `index.html`. No build step —
edit and reload.

**Add an indicator:**

```js
{id:"X01", t:"Theatre name",
 n:"Indicator name",
 s:"What it signals — why an analyst should care.",
 g:"The trigger threshold. Specific and falsifiable.",
 ax:{alliance:4, prolif:2},     // axis weights, roughly 1–5
 nx:["Y02","Z03"]}              // indicators that rise in priority once this fires
```

`t` can be an existing theatre or a new one; theatres are derived from the data.

**Add a syndrome:**

```js
{id:"key", name:"Display name",
 need:[["A01","A02"],["B01"]],  // outer = AND, inner = OR
 txt:"What the completed pattern means.",
 worst:"The clause appended to the worst-case paragraph."}
```

**Adjust narrative:** `AXTXT` holds `likely` / `best` / `worst` / `hi` strings per axis.
These are written as sentence fragments and are joined by the engine, so keep them
grammatically continuous — no leading capital, no trailing period, except `hi`, which is
a standalone sentence.

---

## Known limitations

The engine is **deterministic and rule-based**. It reports what a combination implies
under the model's assumptions. It is not a forecast, and it has no knowledge beyond the
weights and syndrome definitions encoded here.

The syndrome coupling is the most contestable part. If you disagree with which indicators
should be treated as correlated, change `SYN` — that is the intended use. The built-in
falsification note exists because a tool that only confirms its own logic is worse than
no tool.

Source reliability and collection tier are deliberately **not** modelled. That judgement
belongs to the analyst at the point of collection, not to a dropdown.

Indicator content reflects the situation as of **17 September 2026** and will need
maintenance. Thresholds referencing "the pre-February-2026 baseline" or similar will need
rebasing as conditions change.

---

## Licence

Do what you like with it.
