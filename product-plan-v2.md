# Trace & Route — Plan (v3)

*Personal tool. Not for sale. Rewritten around what it's actually for.*

---

## 1. What it is

> A map where I draw the paths that aren't mapped, and measure the *exact* length of a route — down to which side of a curve I walk — then compare candidates from A to B, including one the app works out itself.

The point is resolution. Google Maps works at *street* resolution: it measures from the middle of the road, and it doesn't know half the paths here. I need *sub-street* resolution — the difference between the inside and the outside of a bend, measured to the metre.

---

## 2. The two problems, precisely

**Problem one — missing paths.** Real, walkable, rideable paths that Google's data simply doesn't contain, or has but won't route on. Solved by drawing them and importing OSM.

**Problem two — resolution.** Even where the path exists, the measurement is too coarse. Google gives the distance along the street centreline. But walking my daughter to preschool, the left side of the street versus the right side takes a curve at a different radius — the outside of the bend is genuinely longer. I want the exact length of *my* line, not the street's.

Problem two is the one that makes this worth building. It's the thing nothing else does.

---

## 3. How precision actually works

There's no clever geometry here, and that's the point. **Distance is just the sum of the straight segments between points. More points on a curve means a more accurate length.** Four points cut the corner; thirty points converge on the true curve.

So precision is not a special mode with special maths — it's simply placing points as tightly as the accuracy I need, on a satellite image I can zoom into far enough to place them exactly.

What follows from that:

- **The user's trace is ground truth.** The app never smooths, second-guesses, or "corrects" a drawn line. What I placed is what gets measured.
- **No simplification on clicked points.** Every point I place is kept, exactly.
- **Freehand keeps its density.** Drag-to-trace is the fast way to lay down many points at once; its simplification is tuned right down so it preserves the curve rather than thinning it away.
- **Refine after tracing.** After a rough freehand pass I can drag any point onto the exact spot, insert new points where a tight bend needs more density, and delete strays. Coarse pass, then fine pass.
- **Satellite is the tool of record** for precision work — the only basemap detailed enough to place a line on the correct side of a bend.

---

## 4. The comparison loop

1. Set point A and point B.
2. The app solves its own optimal route across everything it knows — my drawn paths plus imported OSM ways.
3. I draw candidate routes by hand between the same A and B — left side, right side, the cross-over.
4. Every option is listed side by side with its exact distance, shortest highlighted, others shown as a delta against the best.
5. I can copy the app's route into an editable variant, drag part of it onto a different path via handles, and watch the number change live.

---

## 5. Snap tolerance and precision work

Snapping collapses nearby endpoints into one shared node — essential for connecting a wider network, where a forgiving ~10 m tolerance is right.

But at curve-precision scale, 10 m is enormous: it would weld a left-side and a right-side path into one and destroy the very difference I'm measuring. So:

- **A precision toggle** that drops snap tolerance to near zero (and sensibly defaults the basemap to satellite). Two jobs, two setups: forgiving "network" mode for connecting routes, tight "precision" mode for measuring one curve against another.

---

## 6. What's in, what's out

**In**
- Click drawing (surgical) and freehand drawing (fast), both dense and un-simplified
- Edit any path: drag points, insert points mid-line, delete points — usable at satellite zoom
- Automatic noding and snapping, with an adjustable tolerance and a precision toggle
- OSM import for the viewport so the map isn't empty on day one
- A→B solve across the whole network
- Multiple named candidates between the same A and B, ranked by exact distance
- Copy the solved route into an editable variant, drag it, re-solve live
- Satellite basemap as the precision tool of record
- Local persistence, GeoJSON export

**Later**
- GPS recording — genuinely optional, because I trust the trace. Nice for effort-free capture, not load-bearing for accuracy.
- Surface and gradient in the cost function
- Crowd heatmap of where people draw paths

**Never (for this)**
- Accounts, sync, sharing, moderation
- Turn-by-turn navigation
- Monetisation, growth, platform decisions

---

## 7. Cost function

**Distance is the only cost for now.** Not time, not surface, not hills. The structure allows those later without a rewrite, but they are not the product. Which side of the curve is shorter is a pure-distance question, and that's exactly the question this answers.

---

## 8. Data model

```
Path      { id, name, points[], source: drawn|osm|gps, surface? }
Graph     { nodes[], edges[] }          // derived from paths, never edited directly
Candidate { id, name, vias[], legs[], distance }   // a way from A to B
Comparison { A, B, candidates[] }       // optional to save, not a priority
```

Paths are what I edit. The graph is rebuilt from them when they change. A candidate is defined by its **via points**, not its geometry — the network is solved between consecutive vias, so dragging a via re-routes that segment while the rest holds its shape. A candidate with no vias is the app's own optimal route.

**Noding** — where two segments cross, split both and share a node.
**Snapping** — endpoints within tolerance collapse to one node. Adjustable, near-zero in precision mode, with a visible circle under the cursor so I can see what will fuse before I click.

---

## 9. Implementation notes

Carried over from the prototype, unchanged in substance:

- **Rendering** — Leaflet is fine until a wide OSM import makes it stutter; MapLibre if so.
- **Projection** — intersection maths in a locally-projected plane (longitude × cos lat). At 64°N, degrees of longitude are 44% the length of latitude. Distances always haversine on the original coords.
- **Graph build** — flatten paths to segments, grid-index at ~200 m, test only same-cell pairs, split at crossings, snap endpoints via a second grid at 2× tolerance. Full rebuild, marked dirty, executed lazily. Web Worker if it exceeds ~200 ms.
- **Routing** — Dijkstra now; binary heap frontier and A\* heuristic when the network grows (the first thing to feel slow when dragging vias over a big import).
- **Line handling** — clicked points never simplified; freehand simplification tolerance kept tight enough to preserve curves.
- **Storage** — IndexedDB. Stable UUIDs and `updatedAt` on every record, so sync isn't painted out later.
- **Tiles** — public tiles fine at personal scale; satellite imagery is the one that matters here. Self-hosted Protomaps Iceland extract only if I want proper offline.

---

## 10. Build order

**Done — drawing, routing, comparison, editable variants.**
Click and freehand drawing, noding, Dijkstra routing, candidate comparison with distance ranking, copy-and-drag variants with live distance.

**Now — precision essentials**
Precision toggle (near-zero snap + satellite default), confirm clicked points are never simplified, confirm freehand keeps curve density, make point editing comfortable at satellite zoom.

**Then — persistence and mobile**
IndexedDB autosave, one-handed phone layout, GPX export.

**Later — GPS recording**
Optional capture-and-review. Not required for trustworthy measurement, since the trace is trusted.

**Someday — heatmap**
Only if there's ever more than one person's data.

---

## 11. Done means

I settle a real question — left side or right side of the street to preschool — and the app gives me the exact length of each line I traced, close enough to trust over a difference of a few metres.
