# Paste this as your first message in Claude Code

---

Read `CLAUDE.md`, then `handoff.md` in full, then skim `path-router.html` to see what's actually there.

Before writing any code, tell me:

1. In two sentences, what this app measures and why an ordinary map can't do it.
2. Anything in the spec that's ambiguous, contradictory, or that you'd push back on.
3. Your plan for `handoff.md` §5.1 (freehand drag-to-trace) — note that it does not exist yet, only click drawing does.

Then stop. Don't start building until I've replied.

---

## Files to put in the working directory

- `CLAUDE.md` — persistent instructions, read automatically
- `handoff.md` — the spec
- `product-plan-v2.md` — background vision
- `path-router.html` — the prototype

## Suggested setup

```bash
mkdir trace-and-route && cd trace-and-route
# copy the four files in
git init && git add -A && git commit -m "Prototype and spec"
claude
```

Committing first matters: it gives you a clean diff of everything the agent changes, and a way back if it decides to be helpful about smoothing your traced lines.
