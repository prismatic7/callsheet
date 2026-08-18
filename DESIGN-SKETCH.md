# callsheet Design Sketch (v2 — Chris amendments applied)

- **Day Surface**: A single centered vertical column of cards, representing a quiet linear sequence. No timeline hour-grids.
- **Colour Grammar**: Low-saturation pastel backdrops (HSL) indicating activity types without shouting. **Cards are framed by a slightly darker accent border** derived from the same hue (lower lightness), so the pastel reads as fill, not blur.
  - Seed types — Research: Slate (`hsl(215, 15%, 93%)` fill / `hsl(215, 15%, 70%)` border) · Making: Sage (`hsl(120, 15%, 92%)`) · Teaching: Ochre (`hsl(38, 30%, 93%)`) · Body: Rose (`hsl(350, 20%, 94%)`) · Admin: Sand (`hsl(30, 10%, 93%)`)
- **Custom activity types**: Users can add activity types. New types get an auto-assigned pastel colour from the palette pool. **Colour uniqueness is enforced — a colour is never reused across types** (allocator tracks the used set).
- **Durable vs Provisional**: Collapsible left sidebar lists durable repeatable templates (the call board). Dragging a template into the day pane instantiates a provisional instance. Provisional cards can be deleted/modified without touching the template.
- **Window presence (three modes)**: The app has a presence toggle —
  1. **Window open**: a normal window in normal window order. It does NOT float above other windows until focused — it holds its place, never covers.
  2. **Status bar**: minimise to the macOS menu bar (status bar).
  3. **Dock toggle**: hide or show in the dock.
  The app is there when you look for it; it never demands. (Presence not pressure.)
- **Typography**: Smaller text than default. Cards are dense and quiet; type recedes behind the colour.
- **Markdown Scope**: H1-H3, bold, italic, bullet lists, blockquotes. Single click edits raw text; focus loss (blur) renders and saves.
- **Clipboard (card-level)**: Standard text manipulations on selected cards — `Cmd+C` copy, `Cmd+V` paste, `Cmd+X` cut. Cards behave like text.
- **Drag-Drop & Keyboard**: Grab handle for mouse drag-to-reorder. Keyboard: `Tab`/`Shift+Tab` navigates; `Cmd+Shift+Up/Down` shifts vertical order; `Enter` begins editing; `Esc` saves and blurs.
- **Day Navigation**: Left/Right border gutter arrows to slide days. `Cmd+Left/Right` shifts active date. Date display is a quiet center header.
- **Agent Presence**: Faint dashed-border "ghost cards" proposed at the bottom of the stack. A click commits them; they never ping or notify.

## Chris sign-off (2026-08-18)

- Pastels approved; accent border added to frame cards.
- Custom activity types required, auto-assigned pastel, colours never reused.
- Sidebar collapsible confirmed. Window presence: open (normal order) / status bar / dock toggle.
- Pushback: smaller text. Clipboard standard manipulations required.
- Ghost cards: "Love the ghost cards."
