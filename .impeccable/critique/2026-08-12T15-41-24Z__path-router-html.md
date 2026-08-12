---
target: main page
total_score: 24
max_score: 40
na_heuristics: 
p0_count: 2
p1_count: 3
timestamp: 2026-08-12T15-41-24Z
slug: path-router-html
---
Method: dual-agent (A: design review, isolated · B: detector + browser evidence, isolated)

## Design Health Score

| # | Heuristic | Score | Key Issue |
|---|-----------|-------|-----------|
| 1 | Visibility of System Status | 2 | The auto-import blocks ~60 s behind a counter that climbs `4/10`, resets to `1/8` and changes vocabulary mid-flight — a retry index presented as progress. No cancel, no elapsed time. |
| 2 | Match System / Real World | 3 | Three names for two things: *Set start / Set finish*, *Options from A to B*, pins reading *A/B*. "Junction snap", "Drop via" are implementation vocabulary. |
| 3 | User Control and Freedom | 2 | No cancel for the 60 s import; `Erase saved` is irreversible with no confirm and outside undo; `Cmd/Ctrl+Z` explicitly excluded; no redo. |
| 4 | Consistency and Standards | 2 | `Cmd+Z` dead; rename is an unhinted `ondblclick` → native `prompt()`; three unit conventions (`4.16 km`, `4.16km`, `+972m`). |
| 5 | Error Prevention | 2 | Placing B silently downloads ~9000 ways with no consent; `Erase saved` unconfirmed; Precision silently switches the basemap. |
| 6 | Recognition Rather Than Recall | 2 | The only documentation sits below every setting and is measurably cut off at an 879px viewport; double-click-to-rename is signposted nowhere. |
| 7 | Flexibility and Efficiency | 2 | Three shortcuts total; no keyboard path to draw, select or move a point; no duplicate-and-offset, which is literally the left-vs-right-kerb workflow. |
| 8 | Aesthetic and Minimalist Design | 3 | Genuinely restrained and internally consistent, but 23–24 controls visible at once with zero progressive disclosure. |
| 9 | Error Recovery | 3 | Error copy is excellent and specific (`leg 2 does not connect (Precision snap is 0.5 m)`) but renders inside the distance column at 10px rose, and offers no recovery action. |
| 10 | Help and Documentation | 3 | Per-mode hint prose is well written and contextual — and placed where it is never seen without scrolling past every setting. |
| **Total** | | **24/40** | **Acceptable — significant improvements needed** |

No heuristic scored n/a; this is an Operate surface and all ten apply.

## Design Specificity Verdict

**Authored for this product in its top third, category-interchangeable below that — and the composition gives its best interaction design to the wrong feature.**

Product-specific and not liftable elsewhere: the readout stack (33px tabular measurement + unit + a caption carrying the point count); the vertex-vs-via silhouette distinction (circle vs 45°-rotated square, told apart by shape so they survive arbitrary satellite terrain); Network/Precision promoted from a numeric parameter into a named, badged, persistent mode; the candidate delta column; `FREEHAND_SIMPLIFY_M = 0.5` with arc-chord error maths in the comment; vertex handles growing above zoom 19.

Generic: everything below "Measurement mode" — the basemap select and nine ghost buttons in 2- and 3-column grids. This is the stock Leaflet-demo control stack in a dark skin. The path list row (dot · name · value · ×) is the most generic list row in existence.

**The specificity failure that matters is conceptual.** The only ranking UI in the app — sorted rows, mint BEST badge, delta column — serves routing on borrowed OSM data. Hand-traced comparison, the reason the product exists, gets a plain unranked list. The scarcest token in the design system (Verdict Mint = "this is the shortest") is spent entirely on the secondary feature. A director reading only the composition would conclude this is an OSM route planner with a drawing tool bolted on — the inverse of the truth.

**Deterministic scan.** `detect.mjs` exit 2, 14 findings, all advisory: 6 × `design-system-color`, 8 × `design-system-font-size`. Triaged: 8 false positives (the panel and marker shadows, the 27px mobile measurement, and three 10px values are all documented in DESIGN.md prose the detector cannot read — it matches only the machine-readable header), 3 real, 2 real-but-marginal. The real ones: `#FFFFFF` at lines 617, 882 and 1050 — a low-opacity white halo marking the live line (active path, active route, in-progress freehand ink). Disciplined, repeated, load-bearing, and absent from DESIGN.md entirely. Marginal: a 13px `×` glyph at lines 84 and 120, a sixth type size off the documented ramp.

**Browser detector, 36 findings**: 14 undersized-ui-text, 12 low-contrast, 4 tiny-text, plus 6 singles. Three are verifiably not page facts — `marquee` (0 `<marquee>` elements), `gradient-text` (0 elements with background-clip:text), and `overused-font: roboto 64%` (no webfont is declared; Roboto is what this machine resolved `system-ui` to). The user-visible overlay banner surfaces **only those three artifacts** and none of the 33 real element-level findings.

**Two tooling failures worth recording:** the Impeccable live server returns a "Not found" body for project files (the first injection round was run against that page and discarded), and it serves `detect.js` without `charset=utf-8`, so Chrome decodes latin-1, corrupts a regex literal and fails to parse the file. Both were worked around (static server + explicit UTF-8 fetch/decode); a native 414px viewport was unobtainable, so mobile numbers come from a same-origin 414×896 iframe.

## Overall Impression

The instrument is real. The readout, the mode gating, the Precision badge and the freehand tolerance constant are the work of someone who understands that this tool sells trust in a number. Then the panel below the fold turns into a generic GIS control stack, and the one screen that should answer *"how much longer is the outside of the bend?"* never answers it.

Single biggest opportunity: **point the candidate list's anatomy at the Paths list.** The delta column, the sorting and the BEST badge already exist 200 lines away in `candPanel()`. Pointing them at drawn paths converts this from a map with a measuring feature into the comparison instrument the product description promises.

## What's Working

**The mode segment as a real gate.** `setMode()` hides tool groups outright rather than disabling them, swaps the header tag, changes the map cursor and rewrites the hint. The three modes have genuinely disjoint verbs — a map click means three different things — so hiding is honest rather than coy. This single decision is why 24 simultaneous controls remain survivable.

**The candidate row anatomy.** Dot · name (ellipsis) · length · right-aligned 44px-minimum tabular delta · ×, sorted by length, exactly one mint badge. The fixed-width right-aligned delta makes magnitudes stack into a scannable column instead of floating at variable offsets. The best-designed component in the file — which is why it is a shame it serves the secondary feature.

**Precision made visible rather than configured.** A named mode, a persistent amber tag, a slider that disables itself and reads `0.5 m`, an automatic switch to satellite, and an error that names the cause at the moment it bites. It converts a silent parameter capable of destroying a measurement into a state you can see.

## Priority Issues

### [P0] Two hand-traced paths cannot be compared, and the headline rounds the difference away
**Why it matters:** this is the product. The concrete case is "the outside of the bend is 20 m longer." The Paths list gives no delta, no ranking, no badge. `fmt()` returns `(m/1000).toFixed(2)` above 1000 m, so headline resolution above 1 km is **10 m — half the magnitude the product exists to detect**. Two paths 20 m apart at 4 km read `4.16km` and `4.18km` in 10px slate, unranked, adjacent, with nothing marking the winner.
**Fix:** give `rowFor()` the candidate row's anatomy — right-aligned tabular delta against the shortest *drawn* path, a mint BEST badge, rows sorted by length. Change the readout to whole metres with a group separator (`4 163 m`) and demote km to the caption.
**Suggested command:** `/impeccable shape`

### [P0] Placing B silently downloads ~9000 ways and paints them over the specimen, blocking ~60 s with no cancel
**Why it matters:** measured live: 60 s wall-clock, 8998 ways, 600.7 km, and the satellite imagery you trace against became an unreadable blue mesh across the whole city. DESIGN.md's own Borrowed Data Rule says imported geometry must never outrank the user's line; here it outranks the map itself. The progress counter climbs to `4/10`, then resets to `1/8` under a different label.
**Fix:** confirm before auto-loading and name the area; render the OSM layer at 1px/45% and hide it below zoom 15; add Cancel to the busy chip (the `AbortController` already exists); replace the retry index with elapsed time.
**Suggested command:** `/impeccable harden`

### [P1] The accessibility floor is below AA on roughly half the text, and the core loop is mouse-only
**Why it matters:** `--slate #5C7A88` on the ink ground computes to **3.98:1** — failing AA at the 9–10.5px sizes it is used at (`.lbl`, `.cap`, `.dlt`, `.p .mt`, the hint, ghost-button labels, the Leaflet attribution), dropping to **3.02:1** on a selected row and **2.86:1** under button hover. Both assessments measured this independently and agreed. Exactly one live region exists (`<output id="snap-out">`), so a screen reader is told when the *snap slider* moves and never when *the distance being measured* changes. Every one of the 27 interactive controls is under 44×44; the two `×` delete affordances are **12×13** and are `<span>`s with only a `title`. Path and candidate rows are `<div onclick>`, unreachable by keyboard; vertices are created `keyboard:false`, so the Delete/Escape shortcuts operate on selections that cannot be made without a mouse. `user-scalable=no` removes the standard remedy.
**Fix:** lift `--slate` to ≈6:1 for text under 12px; `role="status"` on the readout, toast and busy chip; real `<button>`s with accessible names for every `×`; `aria-pressed` on the four toggle pairs; focusable rows with arrow-key traversal; drop `maximum-scale`/`user-scalable`.
**Suggested command:** `/impeccable audit`

### [P1] The snap-preview circle tells the user their trace is being snapped
**Why it matters:** the amber dashed circle and highlighted vertex follow the cursor throughout Add mode, and the hint says "a highlighted point will fuse with your next one." Points are in fact stored verbatim; the radius only affects graph node fusion **for routing**. The single most important guarantee in this product is "we never move your line", and the most persistent live feedback in the drawing mode implies the opposite.
**Fix:** rename the section "Junction tolerance — routing only"; rewrite the hint to "Points this close are treated as one junction *when routing*. Your line is never moved."; draw the circle only in Route mode or once a routable graph exists.
**Suggested command:** `/impeccable clarify`

### [P1] The palette arrays break both of the design system's named colour rules
**Why it matters:** `CPAL[1]` is Verdict Mint, so the second candidate draws mint on the map — observed live as a mint route reading `+972 m`, i.e. the loser, while the mint BEST badge sat on the magenta one. `PALETTE[1]` is Survey Amber, so a drawn path is the same colour as the A/B pins, and `panel()` tints the 33px readout with `a.color` — the headline number rendered in the "anchored" accent. Both violations are in the arrays themselves, not in misuse.
**Fix:** remove `#4BE0A8` from `CPAL` and `#FFC24B` from `PALETTE`; shift `#37D6BE` cooler so it cannot read as mint on imagery. Keep the readout tinted by path colour, but from a palette containing neither accent. Also document the white halo idiom in DESIGN.md.
**Suggested command:** `/impeccable colorize`

## Persona Red Flags

**Alex (power user — the primary persona here, since this is explicitly a single-expert tool):** `Cmd+Z` deliberately ignored (`!e.metaKey && !e.ctrlKey`), so undo appears not to exist. No redo. Rename is an unhinted double-click into a native `prompt()`, silently truncated to 40 chars. No duplicate-and-offset — tracing the left kerb then the right means tracing the whole thing twice by hand. No delta between drawn paths: Alex's actual question, unanswered by the surface built to answer it. Three shortcuts total; none for mode switching or Precision.

**Sam (accessibility-dependent):** zero live regions for the measurement. Every vertex marker is `keyboard:false`. The three `×` controls are unfocusable spans. One `<h1>` and no other headings or landmarks; `#base` has no label association. `user-scalable=no` blocks pinch-zoom on 9px labels (WCAG 1.4.4). Errors render at 10px in rose inside the distance column — colour-only signalling in the smallest available type.

**Riley (stress tester):** `#rcap` renders `· · 0 via` — a double bullet and a singular noun for zero, in the one component whose job is to look precise. Candidate names chain unboundedly (`Optimal variant variant variant`) into an ellipsis. Three unit-spacing conventions for the same quantity. Folding the panel hides the measurement (verified: folded height 40.2px, `.readout` height 0). `Erase saved` has no confirmation and is outside undo. The busy chip abuts the Leaflet zoom control.

**Casey (mobile — secondary milestone, two hard failures):** the app boots folded on a phone, and folding hides the readout — so on launch a phone shows a 40px title bar, a map, and **no measurement anywhere on screen**. Expanded, the sheet takes 64vh, leaving 323px of map to trace on at zoom 19–20. `#fold` is 19×17 and is the only control available while collapsed.

## Minor Observations

- **[P2] Nine buttons inherit one heading that describes two of them.** `EXISTING PATHS FROM OPENSTREETMAP` sits above the two import buttons, then file I/O, two destructive actions and two viewport actions follow unlabelled. `Erase saved` is destructive, outside undo, and marked only by a hover colour. Three labelled groups (Network / File / Danger), with File and Danger behind a disclosure; move Find me and Fit to paths onto the map as Leaflet controls.
- No scale bar on the map — a measuring instrument giving no visual distance reference on the specimen.
- The 33px headline cycles through six palette colours, so the one element DESIGN.md calls "the only element permitted to be loud" has no stable identity.
- The range input's `accent-color: var(--ice)` makes the Junction snap track the brightest horizontal band in the panel — a set-once setting outshining the readout.
- Snap preview exists in Add mode only; in Edit mode, where dragging a vertex can equally fuse it, there is no preview.
- The busy chip has no minimum display duration, so a cache hit flashes `LOADING NETWORK…` for one frame.
- `.p .mt` (a metric) and `.lbl.mt` (a margin utility) share a class name.
- `kx`, the longitude scale factor, updates on `moveend` — a measurement constant tied to viewport position.
- Focus-visible styling is scoped to `button` only; the select, range, map and Leaflet zoom links fall back to the UA ring.
- `prefers-reduced-motion` is handled properly and universally — one of the few a11y details already right.

## Questions to Consider

1. Why does the shortest-path ranking UI serve OSM routing and not the user's own traces? Every mechanism the comparison needs already exists 200 lines away.
2. If the app were reduced to a single number, would it be `4.16 km` — or `+21 m`? Nowhere on this surface is a difference between two traces displayed.
3. Should the headline ever be in kilometres? `toFixed(2)` on km discards 10 m of the resolution the whole product claims.
4. Is a permanent snap-radius circle under the cursor compatible with "we never move your line"?
5. Should the imported OSM network be a *layer* rather than *paths*? Modelling borrowed data as first-class path objects is precisely what lets it drown the map.
6. Half the panel serves routing on borrowed data — a question Google already answers. Would the tool be sharper if Route were a second screen and the first screen were only trace, trace, compare?
7. Why is the best-written prose in the file placed below every setting, where it is measurably invisible?
