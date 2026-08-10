# CLAUDE.md — Trace & Route

Read `handoff.md` in full before writing any code. `product-plan-v2.md` is background vision; `handoff.md` is the spec.

## The rule that overrides everything

This app measures the **exact length of a line the user traced by hand**. Left side vs right side of the same street takes a bend at different radii, and the difference is the product.

**The user's trace is ground truth.** Never smooth, simplify, snap-to-road, average, or "correct" a drawn line. If a change would make a traced line measure as anything other than exactly what the user drew, it is wrong — no matter how sensible it looks. This is the single most likely way to break this project while appearing helpful.

## The codebase

`path-router.html` — one self-contained file, ~40 KB, Leaflet 1.9.4 from CDN, no build step, no dependencies, no package.json. Open it in a browser and it runs.

Keep it that way unless explicitly told otherwise. Single file, no bundler, no framework. If it must be split, ask first.

## Working agreement

- Work through `handoff.md` §5 in order. One item per commit.
- Each §5 item has explicit acceptance criteria. Meet them literally; don't reinterpret.
- `handoff.md` §6 is settled — implement it, don't relitigate it.
- `handoff.md` §7 is genuinely open — raise those, don't silently decide them.
- Freehand drawing (§5.1) does **not** exist yet despite what the plan implies. Build it.
- Don't do §5.7 performance work until something is actually slow.

## Verify before claiming done

No test harness exists. Verify by hand in a browser, and say what you checked:

- Syntax: extract the script and `node --check` it.
- Every `getElementById` id resolves in the HTML.
- The specific acceptance criterion for the item, exercised at satellite zoom 19–20 where this app is actually used.

Don't report an item complete on the strength of the code reading correctly.

## Glossary

Terms are specific here — see `handoff.md` §3. Especially: a **candidate** is defined by its **via points**, not by stored geometry. That model is load-bearing; don't replace it.
