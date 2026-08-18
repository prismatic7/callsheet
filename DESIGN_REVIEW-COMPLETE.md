# DESIGN REVIEW — callsheet v0.1.0 (complete)

**Date:** 2026-08-18
**Scope:** Full design review of the shipped v0.1.0 build — visual system, interaction
grammar, accessibility, feature completeness against the signed-off contract
(`DESIGN.md`, `DESIGN-SKETCH.md`, `CONTRACT.md`, `PRODUCT.md`).
**Method:** Static code review of `src/` (React/TS) and `src-tauri/src/` (Rust),
cross-checked against the design contract. No live render (desktop Tauri app).
**Verdict:** **Strong, coherent, on-voice.** The design system is genuinely
implemented, not just documented. The "presence not pressure" principle holds in
the shipped UI. The main gaps are **feature-completeness** — three signed-off
features are unreachable in the build — plus a handful of interaction and
accessibility refinements. No P0 (blocking) issues found.

---

## 1. Executive summary

Callsheet is a rare case where the design contract and the implementation are in
real alignment. The colour grammar (pastel fill + same-hue accent border) is
implemented exactly as specified, with the uniqueness invariant enforced in the
backend and proven by tests. The keyboard grammar is thorough and genuinely
usable. The inline markdown editor is a hand-rolled contentEditable↔markdown
bridge with caret preservation, list/quote exit logic, and paste handling — this
is hard to get right, and it is tested.

The phase-7 fix pass (undo, calm errors, title/favicon, tokenised HSL, keyboard
hint, ghost-label contrast, safety comments) all landed and are verified in the
code. The build is clean and the working tree is committed.

**The one structural weakness:** three features that are core to the signed-off
design — **window presence modes, custom activity types, and agent ghost cards** —
are defined in the code but **unreachable in the UI**. The most distinctive idea
in the whole design (the agent's faint ghost-card presence) cannot be
demonstrated in the shipped build. This is the highest-priority finding.

---

## 2. What's working well

### 2.1 The design system is real, not decorative
- **Colour grammar implemented faithfully.** Each activity type is a
  low-saturation pastel fill (`hsl(…, 92–94%)`) framed by a same-hue accent
  border at 70% lightness, derived via `deriveBorder()` in `tauri.ts`. The pastel
  reads as fill, not blur — exactly the DESIGN.md intent.
- **Colour uniqueness is a hard invariant, enforced in Rust.** `colour_allocator`
  allocates from a 14-colour pool distinct from the five seeds, and the
  exhaustion fallback still never reuses a colour. Test-proven.
- **Type recedes behind colour.** 13px / 1.4 / weight 400, system UI stack. The
  typography is genuinely quiet; it does not compete with the colour grammar.

### 2.2 "Presence not pressure" holds in the shipped UI
- No notifications, no alert tones, no analytics, no time-blocking — the hard
  constraints are respected.
- Calm error copy (`calmError` in `App.tsx`) maps failures to on-voice messages
  ("Couldn't save this card. It's still here — try again.") and keeps technical
  detail in the console. This is exactly the right register.
- The empty state is on-voice: "A quiet day. No cards yet." with a quiet hint.

### 2.3 The keyboard grammar is a genuine strength
Tab/Shift+Tab navigation, Cmd+Shift+↑↓ reorder, Enter edit, Esc save+blur,
Cmd+Left/Right day nav, Cmd+Z undo, and Cmd+C/X/V clipboard are all implemented
and correctly gated so the editor owns the keyboard while editing. This is a
desktop app that rewards keyboard users — appropriate for a day-board.

### 2.4 The inline markdown editor is the standout craft
The contentEditable↔markdown bridge (`editor.ts`) preserves caret position across
inline transforms, handles list/quote exit on empty lines, splits blocks on
Enter, merges on Backspace, and handles multi-line paste inside lists. The
`dangerouslySetInnerHTML` usage is safe (HTML escaped before transform,
test-proven) and now documented with comments. This is production-grade work.

### 2.5 Accessibility baseline is solid
- `:focus-visible` rings on all focusable elements.
- `role="list"` / `role="listitem"` on the card stack.
- `aria-label` on icon-only buttons (nav, add, delete, grab).
- `prefers-reduced-motion` support.
- Keyboard navigation is a first-class grammar, not an afterthought.

---

## 3. Findings by severity

### P1 — Feature-completeness gaps (signed-off features unreachable)

**1. Window presence modes are not wired into the UI.**
`setWindowPresence` is defined in `tauri.ts` and the backend command
`set_window_presence` exists (normal / statusbar / dock), but **no UI calls it**.
The three presence modes — a core DESIGN.md feature ("the app is there when you
look for it; it never demands") — are completely unreachable. The user cannot
minimise to the menu bar or toggle the dock.
*Fix:* add a quiet presence control (e.g. a small menu or a status-bar affordance)
that calls `setWindowPresence`. This is the single most important missing piece.

**2. Custom activity types have no UI.**
`createActivityType` / `deleteActivityType` are defined in `tauri.ts` and the
backend allocator works (tested), but **nothing in the UI calls them**. A user
cannot add a custom type, so the entire colour-allocator machinery is dead
surface. Custom types are a signed-off requirement.
*Fix:* add a quiet "add activity type" affordance (e.g. in the sidebar or a
small type menu) wired to `createActivityType`.

**3. Agent ghost cards are unreachable.**
Ghost cards render only when `isGhost` cards exist in the DB, but **nothing
creates them** — the agent layer (Hermes/Noema) that would propose them is not
wired. The most distinctive idea in the design ("faint dashed-border ghost cards
that never ping") cannot be demonstrated in the shipped build.
*Fix:* wire the agent proposal path (or at minimum a manual "propose" affordance
for testing) so ghost cards are demonstrable. This is the feature that makes
callsheet *callsheet*.

### P1 — Dead code / unused surface

**4. `move_card` command + `moveCard` wrapper are dead.**
The frontend reorders via `persistOrder` (reindexing all positions through
`saveCard`), never calling `move_card`. The command exists in the contract and
backend but is unused. Either wire it or remove it to avoid contract drift.

**5. `.day-pane--over` class is defined but never applied.**
The drag-over feedback on the day pane is dead CSS. Either apply it during
template/card drag-over or remove it.

**6. `deriveTextSecondary` is defined but never used.**
Minor dead code in `tauri.ts`.

### P2 — Interaction / UX

**7. No way to change a card's activity type after creation.**
Once a card is created with a type, there is no affordance to re-shade it. The
template editor has a type selector, but cards do not. For a colour-grammar app,
re-shading a card is a natural need (a card planned as "research" that becomes
"making").
*Fix:* add a type picker to the card (e.g. a small colour-dot menu on hover).

**8. No "today" shortcut.**
DayNav has prev/next but no way to jump back to today. For a day-board, a
"Today" affordance (button or Cmd+T) is a natural expectation after navigating
away.

**9. Delete affordance is small and hidden.**
The delete button is a 20px target that only appears on hover/selection
(opacity 0 → 1). Backspace-to-delete is fast but easy to mis-tap. The Cmd+Z undo
mitigates this well, but the delete button itself is below comfortable click
size and gives no destructive-action signal (see 12).

### P2 — Accessibility

**10. Colour is the primary grammar with no non-colour redundancy.**
PRODUCT.md itself flags this: "future phases must ensure activity types remain
distinguishable by more than hue alone." The accent border is same-hue (so it
does not help distinguish types), and there is no icon, label, or pattern per
type. A colour-vision-deficient user would struggle to tell Research (slate)
from Admin (sand), or Making (sage) from Body (rose).
*Fix:* add a per-type non-colour cue — a small glyph, a label chip, or a
distinct border pattern — so types are distinguishable without hue.

**11. Grab handle is a 14px target.**
Small for a drag handle, though acceptable for a mouse-driven desktop app. Worth
noting; not blocking.

### P2 — Visual / craft

**12. Delete hover gives no destructive signal.**
`.card__action--delete:hover` is identical to `.card__action:hover` (both set
`color: ink; border-color: hover-border`). For a calm app this is arguably
intentional, but a destructive action with no visual distinction is a minor
concern. A quiet red or a more assertive border on the delete hover would signal
destruction without shouting.

**13. Delete button uses a "×" text character, not an SVG.**
Inconsistent with the grab handle, which uses an inline SVG. Minor; a proper
icon would be crisper at 20px.

### P3 — Polish

**14. Redundant CSS rule.** `.card__action--delete:hover` duplicates
`.card__action:hover` (lines 351–354 vs 347–350). Collapse into one rule.

**15. Sidebar width transition** — documented as accepted (phase-6 detector
advisory). Fine; the comment in `App.css` explains the rationale.

---

## 4. Verified phase-7 fixes (all landed)

| Fix | Status | Evidence |
|---|---|---|
| Undo for delete (Cmd+Z) | ✅ | `App.tsx` `lastDeleted` + Cmd+Z handler |
| Calm error copy | ✅ | `calmError()` maps save/delete/load |
| Title + favicon | ✅ | `index.html` `<title>callsheet</title>` + inline SVG |
| Tokenised HSL | ✅ | `:root` custom properties in `App.css` |
| Keyboard hint | ✅ | Empty-state hint in `App.tsx` |
| Ghost-label contrast | ✅ | `--ghost-label: hsl(0,0%,40%)` ≈ 5.4:1 on paper (passes 4.5:1) |
| Dead nav props | ✅ | `canGoPrev`/`canGoNext` removed |
| `dangerouslySetInnerHTML` comments | ✅ | `Card.tsx`, `GhostCard.tsx` |

---

## 5. Prioritised action list

| # | Severity | Action | Effort |
|---|---|---|---|
| 1 | P1 | Wire window presence modes into the UI | M |
| 2 | P1 | Add custom activity-type UI (create/delete) | M |
| 3 | P1 | Wire agent ghost-card proposal path | M |
| 4 | P1 | Wire or remove `move_card`; apply or remove `.day-pane--over` | S |
| 5 | P2 | Add per-card activity-type picker | M |
| 6 | P2 | Add "Today" shortcut | S |
| 7 | P2 | Add non-colour redundancy for activity types (a11y) | M |
| 8 | P2 | Give delete hover a destructive signal | S |
| 9 | P3 | Collapse redundant CSS; use SVG for delete | S |

---

## 6. Bottom line

Callsheet is a well-crafted, on-voice day-board. The design system is genuinely
implemented, the keyboard grammar is excellent, and the inline editor is
standout craft. The build is clean and the phase-7 fixes are all verified.

The work that remains is **completing the signed-off vision**: the three
unreachable features (presence modes, custom types, ghost cards) are the
difference between a good day-board and the distinctive "quiet day-board with an
agent presence" that the design promises. Close those gaps and the app fully
becomes what its design says it is.
