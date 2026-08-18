# RECON — callsheet Phase 0

Read-only survey of the callsheet repo (worktree `project/callsheet` @ `c8f9cad`,
main repo `callsheet` @ `c8f9cad`). No code changed.

## 1. What's in place

**Scaffolding (stock Tauri v2 + React 19 template, committed on main `c8f9cad`):**
- Frontend: `src/` — `App.tsx` (default greet demo), `App.css`, `main.tsx`, `vite-env.d.ts`, `assets/react.svg`
- Backend: `src-tauri/` — `src/lib.rs` (single `greet` command), `src/main.rs`, `Cargo.toml`, `build.rs`, `tauri.conf.json`, `capabilities/default.json`, `icons/`
- Config: `package.json`, `tsconfig.json`, `tsconfig.node.json`, `vite.config.ts`, `.gitignore`, `index.html`
- `README.md` (378B, minimal)

**Governance (Phase 1 — VERIFIED committed on main):**
- `AGENTS.md` — committed ✓
- `.agents/` — committed ✓ (GEMINI.md, `rules/architecture.md`, `rules/format.md`, `rules/security.md`, `rules/testing.md`, `workflows/docs.md`, `workflows/onboard.md`, `workflows/review.md`)
- Verified via `git ls-files main -- AGENTS.md .agents` → all 9 files tracked. Phase-1 claim holds.

## 2. What's missing

- **Design truth: NONE.** No `PRODUCT.md`, no `DESIGN.md`, no `.impeccable/` — in **both** the worktree and the main repo. `AGENTS.md`/`GEMINI.md` reference these as if they exist ("Design truth is committed before build"), but they are absent. **This is the single biggest gap.**
- **SQLite layer: absent.** `rusqlite` is not in `Cargo.toml`; no `db.rs`; no persistence. AGENTS.md claims "SQLite via rusqlite" but it is not installed.
- **Tests: absent.** No Vitest, no `test` script in `package.json`, no Rust tests. AGENTS.md documents `npm run test` / `cargo test` but neither is wired.
- **CI: none.** No `.github/` directory, no workflows.
- **Harness dirs:** `.agent/` and `.github/` do not exist. `.opencode/` exists but is **untracked** (runtime telemetry only — see §4).
- **App logic:** `App.tsx` is the untouched greet demo — no cards, no day pane, no drag-drop, no markdown, no day navigation.

## 3. Risks

1. **Design truth absent but referenced.** AGENTS.md asserts PRODUCT.md/DESIGN.md/.impeccable exist. Any agent reading AGENTS.md will assume design truth is committed. Phases 3/6/7 (design lane) have **no source to bind to** — they must author it, not consume it.
2. **Stack claims vs reality drift.** AGENTS.md documents rusqlite + Vitest as present; neither is installed. Agents may write code against a stack that isn't there.
3. **No CI gate.** Nothing enforces `npm run build` / `cargo check` on push; regressions silent.
4. **Untracked harness/telemetry.** `.opencode/` (overclock usage) is untracked — correct to keep out of git, but note it's not reproducible across worktrees.
5. **`src-tauri/Cargo.lock` untracked** in the main repo — should be committed for reproducible Rust builds.

## 4. Untracked files (worktree)

- `.opencode/` — overclock runtime telemetry (`.installed`, `usage.json`). Correctly untracked; not design truth.
- `PIPELINE.md`, `TASK.md` — pipeline artifacts, untracked by design.
- (Main repo additionally: `src-tauri/Cargo.lock` untracked.)

## 5. Suggested structure (for later phases)

```
PRODUCT.md            # product intent (MISSING — author in phase 3)
DESIGN.md             # design north star (MISSING — author in phase 3)
.impeccable/          # design system artifacts (MISSING — author in phase 3)
src/
  components/         # Card, DayPane, etc.
  lib/                # markdown, drag-drop, day state
src-tauri/src/
  db.rs               # SQLite via rusqlite (ADD)
  commands.rs         # Tauri commands (currently inline in lib.rs)
.github/workflows/    # CI: build + type-check + cargo check (ADD)
vitest.config.ts      # frontend tests (ADD)
```

## 6. Phase 3/6/7 needs

- **Phase 3 (design capture):** Author `PRODUCT.md`, `DESIGN.md`, `.impeccable/` from scratch — there is no existing design truth to capture. Commit them so worktree agents can see them (FLEET 2026-08-10: untracked docs are invisible to worktree agents).
- **Phase 6/7 (review):** No CI and no test harness exist; review gates must add both, or review is manual-only.
- **Phase 4–7 binding:** No design system source exists to bind to; phases must create it first.

## 7. Acceptance criteria check

- [x] Report lists what's in place vs missing — §1, §2
- [x] Report flags risks and suggests a structure — §3, §5
- [x] Report states design-truth state (files present, tracked, design system source) — §2, §6
- [x] Report verifies phase-1 governance is committed — §1 (verified on main)
- [x] Report lists which phases 3/6/7 will need what — §6
