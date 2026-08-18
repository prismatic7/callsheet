# Product

<!-- impeccable:product-schema 1 -->

## Platform

desktop (macOS, Tauri v2 native window)

## Stack

React 19 + TypeScript (strict) + Vite 6 frontend; Rust (Tauri v2) backend; SQLite via rusqlite (to be added in phase 4 — recon confirmed rusqlite NOT yet in Cargo.toml). Package manager: npm. Build: `npm run tauri dev` (dev), `npm run tauri build` (release). Tests: Vitest (frontend), `cargo test` from `src-tauri/` (backend).

## Users

A single primary user (Chris) running a personal day-board on a desktop Mac. The user is a maker/researcher/teacher who plans a day across five activity types — research, making, teaching, body, admin — and wants a quiet, always-available surface to hold the day's cards without the app demanding attention.

## Product Purpose

Callsheet is a calm, minimal desktop day-board: a single pane of colour-coded cards for today, with basic markdown in cards, drag-and-drop reordering, and day navigation. It holds the day's plan and lets the user move through it; it never pings, reminds, or pressures. Success means the user can see and shape the day at a glance and trust the board to stay out of the way.

## Positioning

A day-board that is a *dumb surface*: cards, colours, drag-drop, markdown, day navigation — no intelligence in the UI. The agentic layer (Hermes/Noema) reads the same SQLite store to notice patterns and propose activity types as faint ghost cards; the board never knows it is being watched. The governing principle is *presence not pressure* — the app holds, it never pings.

## Operating Context

- The board shows the whole day at once as a single centred vertical column of cards — no timeline, no hour-grid.
- Five seed activity types: Research, Making, Teaching, Body, Admin. Users can add custom activity types.
- Durable vs provisional split: a collapsible left sidebar lists durable repeatable templates (the call board); dragging a template into the day pane instantiates a provisional card that can be edited/deleted without touching the template.
- Cards support basic markdown (H1–H3, bold, italic, bullets, blockquotes); single click edits raw text, blur renders and saves.
- Cards behave like text for the clipboard: Cmd+C / Cmd+V / Cmd+X on selected cards.
- Reordering via grab handle (mouse) and keyboard (Tab/Shift+Tab navigate, Cmd+Shift+Up/Down shift order, Enter edit, Esc save+blur).
- Day navigation via left/right gutter arrows and Cmd+Left/Right; a quiet centred date header.
- Window presence has three modes: window open (normal window order, never floats above others), status bar (menu bar), dock toggle (hide/show in dock).
- The agentic layer (Hermes/Noema) reads the same SQLite store to propose activity types as faint dashed-border ghost cards at the bottom of the stack; a click commits them; they never ping or notify.

## Capabilities and Constraints

- **Confirmed capabilities:** colour-coded cards by activity type; basic markdown in cards; drag-and-drop reordering; day navigation; custom activity types with auto-assigned pastel colours; collapsible sidebar of durable templates; clipboard card ops; three window-presence modes; agent-proposed ghost cards.
- **Hard constraints (the design):** No reminders, no notifications, no alert tones, no analytics, no time-blocking. The governing principle is *presence not pressure* — the app holds, it never pings.
- **Technical constraints:** Tauri commands return `Result<..., String>`; all state is scoped by the day/date — no cross-day bleed; SQLite writes go through parameterized queries — never string-concatenate SQL; TypeScript strict mode is the gate (`npm run build` must pass); never commit secrets (use env vars / `.env`, gitignored).
- **Undecided / to be resolved in later phases:** SQLite schema and persistence layer (phase 4); exact component architecture; test harness wiring (Vitest and Rust tests are documented but not yet installed).

## Brand Commitments

- Name: **callsheet** (lowercase, productName `callsheet`, identifier `com.chris.callsheet`).
- Voice: calm, minimal, quiet. The design language is calm and minimal — colour is the grammar (cards shade by activity type), not decoration.
- The governing principle *presence not pressure* is a binding product commitment: the app holds, it never pings.

## Evidence on Hand

- `DESIGN-SKETCH.md` — the signed-off design sketch (v2, includes Chris's amendments: accent borders, custom activity types with non-reused auto-colours, window presence modes, smaller text, clipboard card ops, ghost-card approval). This is the authoritative visual contract for phase 3.
- `AGENTS.md` — product intent and coding conventions.
- `DEVISING.md` — phase 2 devising record (design questions and constraints).
- `RECON.md` — phase 0 recon (stack reality, gaps).
- No existing UI to extract tokens from — the scaffold is the stock greet demo. This is a from-sketch design capture.

## Product Principles

1. **Presence not pressure.** The app holds the day; it never pings, reminds, notifies, or demands attention. It is there when you look for it.
2. **The board is a dumb surface.** Cards, colours, drag-drop, markdown, day navigation — no intelligence in the UI. The agent is the stage manager, never the board.
3. **Colour is the grammar.** Cards shade by activity type; colour communicates structure, not decoration.
4. **Durable vs provisional.** Repeatable templates live in the sidebar; today's placements are provisional cards that can be changed freely.
5. **Calm and minimal.** Dense, quiet, small text; type recedes behind colour. The constraints are the design.

## Accessibility & Inclusion

No product-specific accessibility requirement was established beyond the general desktop-app baseline (keyboard navigation is a confirmed interaction grammar: Tab/Shift+Tab, Cmd+Shift+Up/Down, Enter, Esc). Colour is used as the primary grammar, so future phases must ensure activity types remain distinguishable by more than hue alone (e.g. the accent border and text affordances).
