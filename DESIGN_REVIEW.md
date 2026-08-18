# DESIGN_REVIEW — callsheet Phase 6

**Date:** 2026-08-18
**Method:** Impeccable critique (degraded single-context — delegation provider 401) + audit + detector, run by Hermes. Open Design MCP run **not executed** — OD MCP not in active toolset this session (flag for next session).
**Critique snapshot:** `.impeccable/critique/2026-08-18T10-09-25Z__src-app-tsx.md`
**Scores:** Heuristics **31/40 (Good)** · Audit **15/20 (Good)** · Detector: 1 documented advisory (sidebar width transition)

## Consolidated fix list (numbered, prioritised)

### P1 — fix before release

1. **Unguarded delete** — `Card.tsx:99-102`: Backspace on a selected card deletes instantly, no confirm, no undo. A mis-tap loses a card permanently.
   - Fix: undo (snapshot last deleted card, Cmd+Z restores) — more on-voice than a modal.
2. **Raw error text in banner** — `App.tsx:414-416`: renders `String(e)` verbatim. Technical jargon, no recovery path.
   - Fix: map known errors to calm copy ("Couldn't save this card. It's still here — try again."), keep details in a log.

### P2 — fix in next pass

3. **Stock window title** — `index.html:7`: "Tauri + React + Typescript" + vite.svg favicon. tauri.conf.json is correct; the webview title leaks the template.
   - Fix: `<title>callsheet</title>`, replace/drop favicon.
4. **Hard-coded HSL not tokenised** — `App.css`: ~15 hard-coded HSL values (selection, focus-visible, hover states, caret, ghost hover, template hover, day-pane--over) outside `:root`.
   - Fix: promote to custom properties (`--accent-focus`, `--ghost-hover`, etc.).
5. **Keyboard grammar invisible** — no hint, no help, no first-run affordance. Excellent grammar, undiscoverable.
   - Fix: quiet "?" affordance or one-line hint in the empty state ("Tab to move between cards · Cmd+Shift+↑↓ to reorder").

### P3 — polish

6. **Sidebar width transition** — `App.css:341` animates `width` (detector finding). One-shot collapse; minor.
   - Fix: `grid-template-columns` animation or accept as documented.
7. **Ghost-card label contrast** — `.ghost-card__label` 11px uppercase `hsl(0,0%,55%)` on paper ≈ 3.2:1, below 4.5:1 for small text.
   - Fix: darken to ~40% lightness.
8. **`canGoPrev`/`canGoNext` hard-coded true** — `App.tsx:406-407`: disabled style never fires.
   - Fix: wire to actual bounds (or remove the dead props).
9. **`dangerouslySetInnerHTML` needs a comment** — safe (markdown escapes HTML, test-proven) but future maintainers need the rationale.
   - Fix: one-line comment.

## Triage

| # | Severity | Fix | Chris decision |
|---|----------|-----|----------------|
| 1 | P1 | Undo for delete | ⬜ |
| 2 | P1 | Calm error copy | ⬜ |
| 3 | P2 | Title + favicon | ⬜ |
| 4 | P2 | Tokenise HSL | ⬜ |
| 5 | P2 | Keyboard hint | ⬜ |
| 6 | P3 | Sidebar transition | ⬜ |
| 7 | P3 | Ghost label contrast | ⬜ |
| 8 | P3 | Dead nav props | ⬜ |
| 9 | P3 | Comment | ⬜ |

**Phase 7 (fix pass) implements the approved subset.**
