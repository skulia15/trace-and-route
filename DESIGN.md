---
name: Trace & Route
description: A survey instrument for measuring the exact length of a hand-traced line.
colors:
  harbour-ink: "#0B171D"
  harbour-ink-raised: "#12242D"
  selected-slab: "#16303B"
  glacier-ice: "#E4EFF3"
  fjord-slate: "#5C7A88"
  fjord-slate-text: "#93B2C0"
  hairline-deep: "#1E3A47"
  hover-slab: "#183441"
  hover-slab-quiet: "#1B2E38"
  trace-magenta: "#FF3D7F"
  survey-amber: "#FFC24B"
  verdict-mint: "#4BE0A8"
  borrowed-blue: "#4E7A8F"
  warning-rose: "#FF7DA6"
  warning-rose-edge: "#4A2437"
  live-halo: "#FFFFFF"
  pin-letter: "#3A2600"
  shadow-panel: "rgba(0,0,0,.55)"
  shadow-pin: "rgba(0,0,0,.5)"
  shadow-marker: "rgba(0,0,0,.6)"
  path-cyan: "#4FC3E8"
  path-periwinkle: "#8FA6FF"
  path-ember: "#FF9F6E"
  path-orchid: "#C48FFF"
  path-lime: "#B6E04B"
  candidate-blue: "#7FB2FF"
  candidate-gold: "#FFD24B"
typography:
  measurement:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "33px"
    fontWeight: 600
    lineHeight: 0.9
    letterSpacing: "-0.02em"
    fontFeature: "tabular-nums"
  title:
    fontFamily: "ui-sans-serif, system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif"
    fontSize: "11.5px"
    fontWeight: 700
    letterSpacing: "0.17em"
  body:
    fontFamily: "ui-sans-serif, system-ui, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif"
    fontSize: "12px"
    fontWeight: 400
    lineHeight: 1.55
  label:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "9px"
    fontWeight: 400
    letterSpacing: "0.19em"
  data:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "11px"
    fontWeight: 400
    letterSpacing: "0.05em"
    fontFeature: "tabular-nums"
  data-small:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "10px"
    fontWeight: 400
    letterSpacing: "0.05em"
    fontFeature: "tabular-nums"
  comparison:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "15px"
    fontWeight: 600
    letterSpacing: "-0.01em"
    fontFeature: "tabular-nums"
  measurement-compact:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "27px"
    fontWeight: 600
    lineHeight: 0.9
    letterSpacing: "-0.02em"
    fontFeature: "tabular-nums"
  glyph:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "14px"
    fontWeight: 400
  glyph-touch:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "16px"
    fontWeight: 400
  map-control:
    fontFamily: "ui-monospace, 'SF Mono', 'Cascadia Mono', 'Roboto Mono', Menlo, Consolas, monospace"
    fontSize: "15px"
    fontWeight: 400
rounded:
  chassis: "2px"
  dot: "50%"
spacing:
  hairline: "6px"
  tight: "8px"
  step: "12px"
  panel: "13px"
  section: "14px"
components:
  button-default:
    backgroundColor: "{colors.harbour-ink-raised}"
    textColor: "{colors.glacier-ice}"
    typography: "{typography.body}"
    rounded: "{rounded.chassis}"
    padding: "8px 6px"
  button-default-hover:
    backgroundColor: "#183441"
  button-active:
    backgroundColor: "{colors.glacier-ice}"
    textColor: "{colors.harbour-ink}"
    rounded: "{rounded.chassis}"
    padding: "8px 6px"
  button-ghost:
    backgroundColor: "transparent"
    textColor: "{colors.fjord-slate}"
    rounded: "{rounded.chassis}"
    padding: "7px 4px"
  button-ghost-hover:
    backgroundColor: "{colors.harbour-ink-raised}"
    textColor: "{colors.glacier-ice}"
  list-row:
    backgroundColor: "transparent"
    textColor: "{colors.glacier-ice}"
    typography: "{typography.body}"
    padding: "7px 9px"
  list-row-selected:
    backgroundColor: "{colors.selected-slab}"
    textColor: "{colors.glacier-ice}"
  badge-best:
    backgroundColor: "{colors.verdict-mint}"
    textColor: "{colors.harbour-ink}"
    rounded: "{rounded.chassis}"
    padding: "1px 4px"
  select:
    backgroundColor: "{colors.harbour-ink-raised}"
    textColor: "{colors.glacier-ice}"
    typography: "{typography.body}"
    rounded: "{rounded.chassis}"
    padding: "7px 6px"
  waypoint-pin:
    backgroundColor: "{colors.survey-amber}"
    textColor: "#3A2600"
    rounded: "{rounded.dot}"
    size: "20px"
---

# Design System: Trace & Route

## Overview

**Creative North Star: "The Survey Instrument"**

This is not a map application with a measuring feature. It is a measuring instrument that happens to show a map. The distinction governs every decision: the map is the specimen under the lens, and the chrome is the barrel of the instrument — dark, matte, and deliberately uninteresting, so that nothing competes with the traced line or the number it produces.

The register is a piece of field equipment. Everything is tight, flat, and mono-labelled. Corners are cut at 2px because a 2px corner reads as a machined edge while a rounded one reads as software. Every number is monospaced with tabular figures, because digits that shift position while a distance updates during a via drag would undermine the one thing this tool sells: that you can trust the measurement to the metre. The ground is a near-black teal-navy — the color of a chart table under low light, chosen so satellite imagery at zoom 19–20 reads at full contrast against it.

Density is a feature, not a compromise. Controls are packed, quiet, and muted until touched; the readout is the only element permitted to be loud. Depth comes from tonal steps in the ink, not from shadow — surfaces stack by getting lighter, and hairlines do the separating.

**Key Characteristics:**
- Near-black teal-navy ground with an ice-white figure; two accents, sparingly used
- Monospace for every number, label, and unit; sans only for prose and controls
- 2px corners universally; no other radius exists
- 1px hairlines as the primary separator, in place of shadow
- Tabular numerals wherever a value can change in place
- Instrument-tight density: 6–14px spacing steps, nothing larger

## Colors

A cold, low-light palette: a near-black teal-navy ground carrying ice-white text, two saturated accents that appear only where they carry meaning, and one green reserved exclusively for a verdict.

### Primary
- **Trace Magenta** (`#FF3D7F`): The line currently under measurement. The active candidate route, the selected vertex, the route distance in the readout. This is the "you are measuring this" color and it appears nowhere decorative.
- **Survey Amber** (`#FFC24B`): Fixed points. The A and B pins, path endpoints, the selected via, the focus ring, and the toast's left edge. Where magenta means *measuring*, amber means *anchored*.

### Secondary
- **Verdict Mint** (`#4BE0A8`): Reserved entirely for the answer. The BEST badge and the winning candidate's delta. It appears at most once per screen, and its scarcity is what makes the ranking readable at a glance.

### Tertiary
- **Borrowed Blue** (`#4E7A8F`): Imported OSM ways only, rendered at 2px and 75% opacity against drawn paths' 3–4px. Deliberately the dullest thing on the map.
- **Warning Rose** (`#FF7DA6`) with **Warning Rose Edge** (`#4A2437`): Destructive hover states and failed-route text. Never a resting state — this color only appears under the cursor or on an error.

### Neutral
- **Harbour Ink** (`#0B171D`): The ground. Page background, the map's backdrop before tiles load, and the panel fill at 94% opacity.
- **Harbour Ink Raised** (`#12242D`): One tonal step up. Button and select fills, toast and busy-indicator grounds, list-row hover.
- **Selected Slab** (`#16303B`): Two steps up. The selected row in the path and candidate lists — the only "you are here" surface.
- **Glacier Ice** (`#E4EFF3`): All primary text, path vertices, and the active-mode button fill.
- **Fjord Slate** (`#5C7A88`): Every mono label, unit, caption, disabled control, and non-winning delta. Roughly half the text on screen is this color, by design.
- **Hairline Deep** (`#1E3A47`): Every 1px border and divider in the application.

### Path Palette
Drawn paths cycle through **Path Cyan** (`#4FC3E8`), **Path Periwinkle** (`#8FA6FF`), **Path Ember** (`#FF9F6E`), **Path Orchid** (`#C48FFF`) and **Path Lime** (`#B6E04B`). Candidates cycle through a separate sequence led by Trace Magenta, then **Candidate Blue** (`#7FB2FF`), **Candidate Gold** (`#FFD24B`), Path Orchid, Path Ember and Path Cyan.

**Neither sequence may contain an accent.** Both once did — the old path palette held Survey Amber, so a drawn path was the same colour as the A/B pins and the readout tinted itself with the "anchored" accent; the candidate palette held Verdict Mint, so the second option drew mint on the map while losing by 972 m, directly contradicting the mint badge on the winner. The old Path Teal (`#37D6BE`) was also close enough to mint on satellite imagery to be read as it, and was shifted cool to cyan.

### Named Rules

**The Two Accents Rule.** Magenta is what is being measured; amber is what is fixed. A control that is neither gets no accent at all. Nothing on this surface is colored to look interesting.

**The Verdict Green Rule.** Verdict Mint means "this is the shortest". It never appears as a general success, confirmation, or positive state, and it never enters a cycling palette. If two things on screen are mint, one of them is wrong.

**The Live Halo Rule.** Pure white (`#FFFFFF`) at very low opacity, and only there, marks the line that is live right now: an 8px/16% halo under the active path, a 9px/14% halo under the winning route, and the 3px/90% dashed ink of a freehand drag in progress. White is never a text or fill colour anywhere in this system; it exists solely as the "this is the one you are working on" underlay, legible over any basemap.

**The Slate Split Rule.** `fjord-slate` (`#5C7A88`) is 3.98:1 on the ground — acceptable for a rule, a dot or a disabled swatch, and below AA for text at the 9–11px sizes this panel is built from. Text therefore takes `fjord-slate-text` (`#93B2C0`, 7.1:1). No text in this system is set in `fjord-slate`.

**The Borrowed Data Rule.** Imported OSM geometry is always duller, thinner, and more transparent than anything the user drew. Borrowed data must never outrank your own line, visually or otherwise.

**The Two Palettes Rule.** Paths and candidates draw from separate color sequences. They are different kinds of object — one is geometry you own, the other is a proposed way through it — and unifying the sequences would erase that.

## Typography

**Display / Data Font:** the platform monospace stack (`ui-monospace`, SF Mono, Cascadia Mono, Roboto Mono, Menlo, Consolas)
**Body Font:** the platform sans stack (`ui-sans-serif`, system-ui, Segoe UI, Roboto, Helvetica, Arial)

**Character:** No webfonts, by choice, and the choice is not thrift. This is a single self-contained file with no build step that must open and run instantly from a local disk; a font request is a dependency the instrument does not need. The platform's own monospace is the right face regardless — it is the face the user's terminal, editor, and coordinate readouts already use, so a measurement rendered in it reads as a system value rather than as marketing.

### Hierarchy
- **Measurement** (mono, 600, 33px, line-height 0.9, `-0.02em`, tabular): the current path length. The single largest element in the interface, and the only one at that scale.
- **Title** (sans, 700, 11.5px, `0.17em`, uppercase): the application name. Deliberately smaller than the number it sits above.
- **Body** (sans, 400, 12px): control labels, list-row names, hint prose at line-height 1.55.
- **Label** (mono, 400, 9px, `0.19em`, uppercase, Fjord Slate): every section heading in the panel. Engraved-nameplate register.
- **Data** (mono, 400, 9.5–11.5px, tabular): distances, deltas, point counts, snap radius, units, captions.

### Named Rules

**The Tabular Numerals Rule.** Every number that can change in place carries `font-variant-numeric: tabular-nums`. A distance updating live during a via drag must not make its own digits jitter — the stability of the figure is part of the claim that the figure is trustworthy.

**The Mono-for-Measured Rule.** If a value was measured, counted, or computed, it is monospaced. If it was typed or named by a person, it is sans. There is no third case.

**The Small-Title Rule.** The product name is set smaller than the measurement. Nothing in the chrome may out-scale the number the instrument produces.

**The Metres Rule.** Every distance the user traced or the app solved is rendered in whole metres, with a thin space grouping the thousands (`4 163 m`). Kilometres are forbidden for a measurement: `(m/1000).toFixed(2)` quantises to 10 m, which is coarser than the difference this instrument exists to detect — the outside of a bend running twenty metres longer than the inside. The one exception is the imported-network row, which reports how much borrowed road is loaded rather than a measurement, and is set in whole kilometres.

**The Signed Difference Rule.** A comparison is always signed and always in metres: `+84 m`, `−12 m`, `±0 m`. A true minus sign, not a hyphen.

## Layout

A single fixed application shell: the map fills the viewport absolutely, and one 326px panel floats over its top-left corner with a 12px inset, capped at `calc(100vh - 24px)` and scrolling internally. The body never scrolls; `overflow: hidden` is set on `html, body`.

The panel is a fixed vertical stack in a deliberate order: readout, mode switch, mode-specific tools, path list, then the settings that are set once and left alone (snap, basemap, import, persistence), closing with a contextual hint. Sub-groups use 2- and 3-column grids at a 6px gap. Lists cap their own height (190px for paths) and scroll internally rather than growing the panel.

Spacing runs on a tight five-step scale — 6, 8, 12, 13, 14px — and stops there. There is no large spacing step in this system, because there is no passage of the interface that wants to breathe.

**Responsive:** at ≤720px the panel unpins from the corner and becomes a bottom sheet — full width, square corners, no side or bottom borders, capped at 64vh — and the measurement drops from 33px to 27px. The map keeps the whole viewport behind it.

## Elevation & Depth

**Depth is tonal, not cast.** Surfaces stack by getting lighter in the same hue: Harbour Ink for the ground, Harbour Ink Raised for controls, Selected Slab for the selected row. Hairlines at Hairline Deep do the separating everywhere else. There is no shadow vocabulary to extend, and new components should reach for a tonal step and a 1px rule rather than inventing one.

Two shadows do exist, and both are map-plane separation rather than UI elevation: the panel's `0 18px 44px rgba(0,0,0,.55)` with a 10px backdrop blur, which lifts the chrome off live satellite imagery, and the 1–6px drops beneath map markers, which keep a vertex legible against arbitrary terrain. Both are about sitting *above the map*, not above the page.

### Named Rules

**The Tonal Step Rule.** Depth comes from `#0B171D` → `#12242D` → `#16303B`. A component that needs to feel raised takes the next tone up and a hairline, never a shadow.

**The Map-Plane Exception.** Shadow is permitted only where an element overlays live map imagery and would otherwise be unreadable against unpredictable terrain. Inside the panel, shadow is forbidden.

## Shapes

One radius: **2px**, on every button, panel, list container, input, badge, select, toast, and keycap. The only exception is `50%`, used for circular map markers and the color dots in list rows.

The form language is rectangular and machined. Borders are always exactly 1px in Hairline Deep. Grouped controls share a single outer border with 1px internal dividers and `overflow: hidden`, so a segmented control reads as one milled part rather than three adjacent buttons. Map markers are the only round objects in the system, and they carry a 2px `#0B171D` ring so they stay legible over any basemap.

## Components

### Buttons
- **Shape:** machined 2px corners, 1px Hairline Deep border.
- **Default:** Harbour Ink Raised fill, Glacier Ice text, 12px sans, 8px/6px padding.
- **Hover / Focus:** fill lifts to `#183441` over 120ms; focus-visible draws a 2px Survey Amber outline at 1px offset.
- **Active (`.on`):** inverts completely — Glacier Ice fill, Harbour Ink text, weight 600. The only inversion in the system, reserved for "this mode is on".
- **Ghost:** transparent, Fjord Slate text at 11px, brightening to Glacier Ice on hover. Used for every secondary action. Disabled drops to 35% opacity.
- **Warn:** ghost until hovered, then Warning Rose text on a `#4A2437` border. Destructive intent appears only under the cursor.

### Segmented Control
A single bordered strip with `overflow: hidden`, children at `flex: 1` with zero radius and 1px right dividers, last child undivided. Carries the primary mode switch. Reads as one part, not three.

### Comparison strip
The answer the instrument exists to give, and the reason the readout is a stack rather than a single number: how much longer the active trace is than the shortest one traced. A 15px tabular signed difference in Glacier Ice with a mono caption naming the line it is measured against (`+84 m · longer than Path 1`); when the active trace *is* the shortest, it collapses to the word `SHORTEST` in Verdict Mint. Hidden entirely until there are two measurable drawn paths, because before that there is nothing to compare.

The Paths list carries the same verdict: rows sort shortest first, the shortest wears a mint `SHORTEST` badge, and every other row carries its signed difference in a right-aligned 52px tabular column so magnitudes stack into a scannable line. This anatomy belongs to drawn paths first and to routed candidates second — the ranking of your own traces is the product; the ranking of borrowed routes is a convenience.

### Readout
The instrument's face, and the only component with real scale. It sits *outside* the scrolling region, so collapsing the panel on a phone hides the settings and keeps the measurement — the number may never leave the screen. A 33px tabular measurement baseline-aligned with an 11px Fjord Slate unit, a truncating mono caption beneath, and a hairline-topped strip carrying the solved route distance in Trace Magenta. When no route is set the strip takes a `.none` state that drops the value back to Fjord Slate.

### List Rows
Paths and candidates share one anatomy: a 9px color dot, a truncating name at `flex: 1`, mono metrics, and a mono `×` that turns Warning Rose on hover. Rows separate with 1px dividers, last row undivided; hover fills Harbour Ink Raised, selection fills Selected Slab and sets the name to weight 600. Candidates add a right-aligned 44px-minimum delta column so deltas stack into a readable column, and the winner takes the Verdict Mint BEST badge.

### Inputs
Range inputs use `accent-color: var(--ice)` at 18px height, paired with a right-aligned mono `<output>` at a 36px minimum so the value does not shift as digits change. Selects match button styling exactly — Harbour Ink Raised, 1px border, 2px corners, full width.

### Map Markers
- **Waypoint pin:** 20px Survey Amber circle, `#3A2600` mono letter, 2px Harbour Ink ring.
- **Vertex:** 11px Glacier Ice circle; 13px in Survey Amber at a path end; 15px in Trace Magenta with a 4px magenta halo when selected.
- **Via:** an 11px Glacier Ice square rotated 45°, growing to 14px in Survey Amber with a halo when selected. Diamond versus circle is how a via is told from a vertex at a glance.

### Drawers
Settings touched at most once a session — the OSM network buttons, file import and export, and the two destructive actions — live in `<details>` drawers under mono uppercase summaries with a `+` / `–` marker. Nine buttons under one heading that described two of them is what this replaced. The destructive drawer turns Warning Rose when open, so the danger is visible before the cursor arrives rather than only under it.

### Consent bar
Anything that fetches thousands of ways over a shared free service and paints them across the map asks first, in an inline bar over the map bottom — never a modal, because the area under discussion must stay visible behind the decision. It states the area in km², offers `Not now` first and `Load it` second, and until it is answered, not one network request is made.

### Toast & Busy
Both are Harbour Ink Raised chips with 1px borders and 2px corners. The toast is bottom-centered with a 2px Survey Amber left edge, fading in over 200ms; the busy indicator sits top-right and carries two things: **elapsed seconds** and a **Cancel**. Never a retry index — "4/10" resetting to "1/8" under a different label is bookkeeping shown by mistake, and it counts backwards. A minimum on-screen time of 350ms stops a cache hit strobing the chip for a single frame.

### Accessibility floor
Not decoration on top of the design, part of it: text never below 4.5:1; a visually-hidden live region announcing the solved distance, a nudged point and a finished import, kept off the readout's own region so a via drag cannot machine-gun the speech queue; `aria-pressed` on every toggle pair; every delete affordance a real `<button>` with an accessible name rather than a `<span>` carrying a `title`; both lists `role="listbox"` with roving `tabindex`, arrow traversal, `F2` to rename and `Delete` to remove; `[` and `]` stepping through a path's points with arrow keys nudging the selected one, so the point-level work is reachable without a mouse. Pinch-zoom is never disabled. On a touch viewport every control is at least 44px tall.

## Do's and Don'ts

### Do:
- **Do** use `font-variant-numeric: tabular-nums` on every value that can change in place.
- **Do** reach for a tonal step (`#0B171D` → `#12242D` → `#16303B`) plus a 1px hairline when a surface needs to feel raised.
- **Do** keep 2px corners on everything rectangular and 50% on map markers only.
- **Do** render imported OSM geometry thinner, duller, and more transparent than drawn paths.
- **Do** set every measured or computed value in the monospace stack, and every human-authored name in the sans stack.
- **Do** give grouped controls one shared outer border with 1px internal dividers.
- **Do** keep spacing on the 6 / 8 / 12 / 13 / 14px steps.
- **Do** ring map markers with 2px of Harbour Ink so they survive any basemap.

### Don't:
- **Don't** add a shadow inside the panel. Shadow is only for elements floating over live map imagery.
- **Don't** introduce a third radius, or soften 2px toward a rounded-card look.
- **Don't** use Verdict Mint for anything other than "this is the shortest".
- **Don't** apply an accent to a control merely to make it interesting; magenta means measuring and amber means anchored.
- **Don't** load a webfont. The single-file, no-build, opens-from-disk constraint is a design decision, not an oversight.
- **Don't** let any chrome element out-scale the 33px measurement.
- **Don't** merge the path palette and the candidate palette into one sequence.
- **Don't** add a large spacing step; this system has no passage that breathes.
