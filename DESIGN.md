---
name: callsheet
description: A calm, minimal desktop day-board — colour-coded cards for today, presence not pressure.
colors:
  research-slate-fill: "hsl(215, 15%, 93%)"
  research-slate-border: "hsl(215, 15%, 70%)"
  making-sage-fill: "hsl(120, 15%, 92%)"
  making-sage-border: "hsl(120, 15%, 70%)"
  teaching-ochre-fill: "hsl(38, 30%, 93%)"
  teaching-ochre-border: "hsl(38, 30%, 70%)"
  body-rose-fill: "hsl(350, 20%, 94%)"
  body-rose-border: "hsl(350, 20%, 70%)"
  admin-sand-fill: "hsl(30, 10%, 93%)"
  admin-sand-border: "hsl(30, 10%, 70%)"
  ink: "hsl(0, 0%, 6%)"
  ink-secondary: "hsl(0, 0%, 40%)"
  paper: "hsl(0, 0%, 97%)"
  ghost-border: "hsl(0, 0%, 60%)"
  hairline: "hsl(0, 0%, 88%)"
  focus-ring: "hsl(215, 15%, 50%)"
  caret: "hsl(215, 15%, 40%)"
  scrollbar-thumb: "hsl(0, 0%, 80%)"
  scrollbar-thumb-hover: "hsl(0, 0%, 70%)"
  blockquote-border: "hsl(0, 0%, 70%)"
  ghost-hover-border: "hsl(0, 0%, 45%)"
  ghost-hover-text: "hsl(0, 0%, 30%)"
  ghost-label: "hsl(0, 0%, 55%)"
  template-hover-border: "hsl(0, 0%, 75%)"
typography:
  body:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif"
    fontSize: "13px; smaller than default, dense and quiet"
    fontWeight: 400
    lineHeight: 1.4
rounded:
  card: "12px"
  control: "6px"
spacing:
  card-gap: "10px"
  column-width: "560px"
  sidebar-width: "220px"
---

# Design System: callsheet

<!-- SEED: established with the user before implementation; re-run /impeccable document once there's code to capture the actual tokens and components. -->

## Overview

**Creative North Star: "The Quiet Day-Board"**

Callsheet is a calm, minimal desktop day-board. Its visual world is built on one idea: the app holds the day, it never pings. The surface is a single centred vertical column of cards for today — a quiet linear sequence with no timeline, no hour-grid, no chrome that demands attention. Colour is the grammar: each activity type shades its cards with a low-saturation pastel fill, framed by a slightly darker accent border of the same hue, so the pastel reads as fill, not blur. Type is smaller than default, dense and quiet, receding behind the colour. The board is a dumb surface — cards, colours, drag-drop, markdown, day navigation — and the agent's presence is a faint dashed-border ghost card at the bottom of the stack that never pings or notifies.

**Key Characteristics:**
- Single centred vertical column of cards; no timeline or hour-grid.
- Low-saturation pastel fills (HSL ~92–94% lightness) as the colour grammar.
- Each card framed by a slightly darker accent border (same hue, ~70% lightness).
- Smaller-than-default, dense, quiet typography that recedes behind colour.
- Faint dashed-border ghost cards for agent proposals; never pings or notifies.
- Three window-presence modes: window open (normal order), status bar, dock toggle.

## Colors

The palette is a set of low-saturation pastels, one per activity type, each with a matching darker accent border of the same hue. Colour is the grammar — it communicates structure, not decoration.

### Primary

The five seed activity types are the primary palette. Each is a fill/border pair sharing a hue.

- **Research Slate** (`hsl(215, 15%, 93%)` fill / `hsl(215, 15%, 70%)` border): the research activity type. Cool, quiet slate.
- **Making Sage** (`hsl(120, 15%, 92%)` fill / `hsl(120, 15%, 70%)` border): the making activity type. Soft green.
- **Teaching Ochre** (`hsl(38, 30%, 93%)` fill / `hsl(38, 30%, 70%)` border): the teaching activity type. Warm ochre.
- **Body Rose** (`hsl(350, 20%, 94%)` fill / `hsl(350, 20%, 70%)` border): the body activity type. Muted rose.
- **Admin Sand** (`hsl(30, 10%, 93%)` fill / `hsl(30, 10%, 70%)` border): the admin activity type. Neutral sand.

### Neutral

- **Ink** (`hsl(0, 0%, 6%)`): primary text on pastel fills.
- **Paper** (`hsl(0, 0%, 97%)`): the day-pane background behind the cards.
- **Ghost Border** (`hsl(0, 0%, 60%)`): the faint dashed border of agent-proposed ghost cards.

### Named Rules

**The Colour-Uniqueness Rule.** A colour is never reused across activity types. Custom activity types are auto-assigned a pastel from the palette pool, and the allocator tracks the used set so no two types ever share a colour. This is a hard invariant, not a preference.

**The Accent-Border Rule.** Every card is framed by a slightly darker accent border derived from the same hue as its fill (lower lightness, ~70%). The pastel reads as fill, not blur. The border is the frame that makes the fill legible.

## Typography

**Display Font:** system UI stack (no display face — the board is quiet, no display voice).
**Body Font:** system UI stack: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif`.

**Character:** Smaller than default, dense and quiet. Type recedes behind the colour — it is the quiet carrier of the day's content, not a display voice. **Implemented at 13px / line-height 1.4 / weight 400** (resolved during implementation, phase 4).

### Hierarchy

- **Body** (400, smaller than default, line-height ~1.4): the card content. Dense and quiet; type recedes behind colour.
- **Date Header** (quiet, centred): the day's date, displayed as a quiet centred header. No emphasis beyond the day itself.

### Named Rules

**The Type-Recedes Rule.** Type is smaller than default and dense; it recedes behind the colour. The colour grammar carries the structure; the type carries the content quietly.

## Layout

A single centred vertical column of cards represents the day as a quiet linear sequence. There is no timeline, no hour-grid, no multi-column board. A collapsible left sidebar lists durable repeatable templates (the call board); dragging a template into the day pane instantiates a provisional card. Day navigation uses left/right gutter arrows and Cmd+Left/Right, with a quiet centred date header. **Implemented measurements (resolved during implementation, phase 4):** column width 560px centred; card gap 10px; sidebar width 220px (collapsible).

## Elevation & Depth

The system is flat by default — depth is conveyed by the accent border framing each card, not by shadows. **Implemented: no shadows on cards** (resolved during implementation, phase 4); depth comes solely from the accent border.

## Shapes

The form language is to be resolved during implementation. The confirmed invariant is that cards are framed by a slightly darker accent border of the same hue as their fill; the exact corner radius is not specified in the signed-off sketch. **Implemented: card radius 12px** (resolved during implementation, phase 4).

## Components

No components exist yet — this is a from-sketch capture. The confirmed component-level rules are:

### Cards
- **Background:** the activity type's pastel fill.
- **Border:** a slightly darker accent border of the same hue (~70% lightness) framing the card.
- **Content:** basic markdown (H1–H3, bold, italic, bullets, blockquotes); single click edits raw text, blur renders and saves.
- **Clipboard:** cards behave like text — Cmd+C / Cmd+V / Cmd+X on selected cards.
- **Reorder:** grab handle for mouse drag; keyboard Tab/Shift+Tab navigate, Cmd+Shift+Up/Down shift order, Enter edit, Esc save+blur.

### Ghost Cards (agent proposals)
- **Style:** faint dashed-border card at the bottom of the stack.
- **Behavior:** a click commits the proposal; they never ping or notify.

### Sidebar (durable templates)
- **Style:** collapsible left sidebar listing durable repeatable templates (the call board).
- **Behavior:** dragging a template into the day pane instantiates a provisional card that can be edited/deleted without touching the template.

## Do's and Don'ts

### Do:
- **Do** use the low-saturation pastel fills (HSL ~92–94% lightness) as the colour grammar for activity types.
- **Do** frame every card with a slightly darker accent border of the same hue (~70% lightness).
- **Do** keep type smaller than default, dense, and quiet — type recedes behind colour.
- **Do** keep the day surface a single centred vertical column with no timeline or hour-grid.
- **Do** show agent proposals as faint dashed-border ghost cards that never ping or notify.

### Don't:
- **Don't** reuse a colour across activity types — colour uniqueness is enforced by the allocator.
- **Don't** add reminders, notifications, alert tones, analytics, or time-blocking. The app holds, it never pings.
- **Don't** let the window float above other windows in normal open mode — it holds its place, never covers.
- **Don't** use colour as decoration; colour is the grammar that communicates structure.
