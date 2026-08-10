# Trace & Route — Handoff Spec

*For a coding agent picking this up cold. The vision lives in `product-plan-v2.md`; this document is the buildable part. Read section 1 before touching anything.*

---

## 1. The one thing to understand

This app measures the **exact length of a line the user traced by hand**, so they can compare two routes that ordinary maps treat as identical.

Concrete example, and the reason the project exists: walking to preschool, the left side of the street and the right side of the street take the same bend at different radii. The outside of the bend is genuinely longer. Google reports one number for both, because it measures the street centreline. This app reports two numbers, and the difference might be twenty metres.

Everything below serves that. If a change would make a traced line measure as anything other than exactly what the user drew, it is wrong, no matter how sensible it looks.

**The user's trace is ground truth.** Never smooth, simplify, snap-to-road, or "correct" a drawn line.

---

## 2. Current state of the prototype

Single file: `path-router.html`. ~40 KB, self-contained, Leaflet 1.9.4 from CDN, no build step. Open it in a browser and it runs. Opens on Reykjavík.

### Works today

| Area | State |
|---|---|
| Click drawing | Every click extends the active path. Toggle to append at start or end. "New path" begins a separate one. |
| Point editing | Drag vertices, click to select, delete point (button or Delete key), insert a vertex by clicking the line, split a path at a point. |
| Path list | Name, colour, length, point count. Rename on double-click, delete inline. Imported OSM paths collapse into one row. |
| Basemaps | Street, terrain, light, dark (default), satellite (Esri). |
| OSM import | Overpass API for the current viewport. Two presets: bike & foot, all roads. Imported ways are read-only, route normally. |
| Graph | Noding and snapping, grid-indexed, rebuilt lazily when paths change. Snap tolerance slider, 1–40 m, default 10. |
| Routing | Dijkstra across the whole network, drawn plus imported. |
| Comparison | Multiple candidates between one A and B, ranked by distance, shortest tagged BEST, others show +delta. |
| Variants | Copy a candidate, then click or drag via handles to pull it onto different paths. Re-solves live, ~70 ms throttle. |
| Persistence | GeoJSON save and load. Undo, 50 steps, `z` key. |

### Does NOT exist yet, despite being discussed

- **Freehand drag-to-trace.** Not implemented at all. Only click drawing exists. The plan mentions freehand; treat that as a requirement, not a description.
- **Precision toggle.** Not implemented.
- **IndexedDB persistence.** Nothing survives a page reload except manual GeoJSON export.
- **GPX export.**
- **Mobile layout.** There is a media query; it has not been used in anger.

### Known weak points

- Dijkstra's frontier is a linear scan. Fine now; it will be the first thing to feel slow when dragging a via across a wide "all roads" import. Replace with a binary heap.
- Graph rebuild is single-threaded on the main thread. Move to a Web Worker if it passes ~200 ms.
- Overpass is a free shared service and throttles. Handle failure gracefully.

---

## 3. Glossary

Terms have specific meanings here. An agent will guess wrong otherwise.

- **Path** — a polyline the user drew, or a way imported from OSM. The thing that gets edited. Paths are the source of truth.
- **Graph** — nodes and edges derived from all paths. Rebuilt when paths change. **Never edited directly.**
- **Noding** — where two segments cross, split both at the intersection and give them a shared node, so a route can turn there.
- **Snapping** — endpoints within a tolerance collapse into one shared node, so two separately-drawn paths connect.
- **Candidate** — one way of getting from A to B. Defined by an ordered list of **via points**, not by fixed geometry. The network is solved between consecutive vias.
- **Via** — a waypoint a candidate must pass through. Dragging one re-routes the legs either side of it while the rest of the candidate holds its shape. A candidate with zero vias is the app's own optimal route.
- **Leg** — the solved geometry between two consecutive stops (A→via1, via1→via2, via2→B).

---

## 4. Data model

```
Path      { id, name, points[], source: drawn|osm|gps, color }
Graph     { nodes[], adjacency }                  // derived, never edited
Candidate { id, name, vias[], legs[], distance, color }
Comparison { A, B, candidates[] }                 // optional to save, low priority
```

The via-based candidate model is load-bearing. It is what makes "copy the app's route, drag one bit of it, keep the rest" work without special cases. Do not replace candidates with stored geometry.

---

## 5. What to build, with acceptance criteria

Ordered. Each item is done when its criteria are unambiguously met.

### 5.1 Freehand drag-to-trace

Fast way to lay down many points at once, because clicking thirty points per curve is tedious.

- Press and drag on the map lays down a dense run of points following the cursor.
- Releasing ends the run; the points join the active path.
- Map panning must be suppressed while a freehand drag is in progress, then restored.
- **Simplification tolerance must be tight enough to preserve curve shape.** Start at 0.5 m and make it a named constant. If in doubt, keep more points, not fewer.
- Works on the satellite basemap at high zoom, which is where it will actually be used.

*Done when:* tracing a bend freehand at zoom 19 and then measuring it gives a length within a metre of the same bend traced by dense clicking.

### 5.2 No simplification of clicked points — verify and lock

- A point placed by clicking is **never** removed, merged, or moved by any automatic process.
- Add a regression check or an explicit comment at every place a path's point array could be rewritten.

*Done when:* placing 40 points along a curve and reloading yields exactly 40 points, in order, unmoved.

### 5.3 Precision toggle

Two jobs need two setups. Network work wants a forgiving snap; measuring one curve against another needs a tight one, because a 10 m snap will weld a left-side and a right-side path into a single line and destroy the measurement.

- A visible toggle with two states: **Network** and **Precision**.
- Network: snap tolerance ~10 m, current behaviour, slider available.
- Precision: snap tolerance near zero (≤ 0.5 m), basemap defaults to satellite on entering the mode.
- Switching modes re-solves candidates; it must not discard paths or candidates.
- The current mode is obvious at a glance.

*Done when:* two paths traced 3 m apart along the same street stay separate in Precision and are measured independently, and fuse in Network.

### 5.4 Snap preview

Snapping is invisible until it surprises you, and a surprise fusion silently ruins a measurement.

- While drawing, show a circle of the current snap radius under the cursor.
- Highlight any existing node that would be fused if the user clicks now.

*Done when:* the user can tell, before clicking, whether the next point will join an existing path or stay separate.

### 5.5 Editing comfortable at satellite zoom

Refining after a freehand pass is a stated core workflow: drag points onto the exact spot, insert points where a bend needs more density, delete strays.

- Vertex hit targets remain usable at zoom 19–20 without being so large they overlap on dense paths.
- Inserting a point on a line is easy and lands where clicked.
- Consider scaling vertex marker size with zoom.

*Done when:* a 40-point curve can be refined point-by-point at zoom 20 without mis-grabs.

### 5.6 IndexedDB persistence

- Autosave paths, candidates, A/B, and mode.
- Restore on load.
- Stable UUIDs and `updatedAt` on every record, purely so nothing is painted into a corner later.
- Keep GeoJSON export; add GPX export.

*Done when:* a page reload loses nothing.

### 5.7 Performance, when it bites

Do these when they are needed, not before.

- Binary heap for the Dijkstra frontier.
- Graph rebuild into a Web Worker if it exceeds ~200 ms.
- A\* with a haversine heuristic if routing itself gets slow.

---

## 6. Settled — do not relitigate

- **Personal tool, not for sale.** No accounts, no sync, no sharing, no moderation, no monetisation.
- **Distance is the only cost.** Not time, not surface, not hills. Structure the cost function so they can be added later; do not add them now.
- **The user's trace is ground truth.** No smoothing, no snap-to-road, no automatic correction.
- **Precision comes from point density**, not from clever geometry. Distance is the sum of straight segments; more points on a curve converge on its true length. There is no fancier maths to reach for.
- **Satellite is the precision basemap.**
- **GPS recording is optional and later.** The user trusts their own tracing; recording is a convenience, not the accuracy mechanism.
- **Candidates are defined by vias, not geometry.**
- Paths are edited; the graph is derived and never edited directly.

---

## 7. Genuinely open — raise these

- **Locked vs re-solving variants.** When a user drags one via on a copied route, should the untouched sections stay exactly as they were, or re-solve around the change? Locked is simpler and probably right, since the user chose those sections deliberately. Decide once it can be seen on screen.
- **One-way streets and directionality.** Everything is currently two-way and passable. This only matters if the app is used for cycling on roads with one-way restrictions; for footpath comparison it does not. Not yet decided.
- **Coverage.** Built and tested around Reykjavík. Whether it needs to work anywhere in Iceland is unresolved; nothing in the design prevents it.
- **Saved named comparisons.** "Home to work, three options" as a saved set. Nice-to-have, explicitly not a priority.

---

## 8. Definition of done for the whole thing

The user settles a real question — left side or right side of the street on the walk to preschool — and gets the exact length of each line they traced, trustworthy over a difference of a few metres.

If most solved routes end up using no user-drawn segments at all, the project has failed and is just a worse Komoot.
