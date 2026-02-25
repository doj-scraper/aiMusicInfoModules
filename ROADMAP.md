# Project Architect // Roadmap

> A living document tracking the evolution from structural tool to full-featured browser IDE.

## Current State

Project Architect v2 is a single-file browser tool (~1,680 lines) providing:

- **Visual file tree** with drag-and-drop reordering
- **Full-screen code editor** with line numbers, auto-save, tab-to-spaces
- **Live HTML preview** with inline resource resolution and console interception
- **ZIP export** for project distribution
- **Boilerplate injection** for rapid prototyping

---

## Roadmap Overview

```
Phase 1: Core Editor Upgrades ───────────────→ Q2 2025
Phase 2: Project-Wide Features ──────────────→ Q3 2025
Phase 3: Language Intelligence ──────────────→ Q4 2025
Phase 4: Enhanced Preview & DevEx ───────────→ Q1 2026
Phase 5: Persistence & Collaboration ────────→ Q2 2026
```

---

## Phase 1: Core Editor Upgrades
**Goal:** Transform the textarea into a code editor people want to use daily.

| Feature | Description | Priority | Status |
|---------|-------------|----------|--------|
| **Syntax Highlighting** | Integrate Prism.js or CodeMirror 6 with a dark theme matching Ghost Labs aesthetic | P0 | 🔲 |
| **Multi-File Tabs** | Keep multiple files open with tab switching; persist unsaved changes per tab | P0 | 🔲 |
| **Search & Replace** | In-file search (`Ctrl+F`) and replace (`Ctrl+H`) with regex support | P1 | 🔲 |
| **Auto-Indentation** | Smart indent on Enter based on bracket context | P1 | 🔲 |
| **Bracket Matching** | Highlight matching braces/parentheses | P1 | 🔲 |
| **Go to Line** | Jump to specific line number (`Ctrl+G`) | P2 | 🔲 |
| **Undo/Redo Stack** | Better undo history beyond native textarea | P2 | 🔲 |

**Technical Notes:**
- Must maintain single-file architecture
- CodeMirror 6 modular bundles allow tree-shaking for size
- Consider `contenteditable` vs `textarea` tradeoffs for highlighting

---

## Phase 2: Project-Wide Features
**Goal:** Make navigation effortless in large projects.

| Feature | Description | Priority | Status |
|---------|-------------|----------|--------|
| **Global Search** | Find across all files (`Ctrl+Shift+F`) with file filter and regex | P0 | 🔲 |
| **Breadcrumb Navigation** | Path breadcrumb above editor for quick folder jumping | P1 | 🔲 |
| **Quick Open** | `Ctrl+P` fuzzy finder for files by name | P1 | 🔲 |
| **File Outline** | Mini-sidebar showing function/class definitions in current file | P2 | 🔲 |
| **Minimap** | Sublime/VS Code style code minimap | P3 | 🔲 |
| **Recently Closed** | Reopen recently closed tabs (`Ctrl+Shift+T`) | P2 | 🔲 |

**Technical Notes:**
- Global search requires flat file index maintained in memory
- Outline parsing needs simple regex for JS function detection
- Minimap can be canvas-based for performance

---

## Phase 3: Language Intelligence
**Goal:** Reduce context-switching to documentation.

| Feature | Description | Priority | Status |
|---------|-------------|----------|--------|
| **Basic Linting** | JS/HTML error indicators using ESLint-like rules (no server) | P1 | 🔲 |
| **Emmet Support** | Expand CSS selectors to HTML (`div>p*3` → actual markup) | P1 | 🔲 |
| **Auto-completion** | Keyword/snippets completion with tab triggers | P2 | 🔲 |
| **Code Folding** | Collapse functions/blocks with gutter indicators | P2 | 🔲 |
| **Color Previews** | Show color swatches next to CSS hex/rgb values | P3 | 🔲 |
| **JSDoc Hints** | Basic hover info for JSDoc-annotated functions | P3 | 🔲 |

**Technical Notes:**
- Use `@codemirror/lint` for client-side linting
- Emmet can use `@emmetio/codemirror-plugin`
- No LSP server — keep everything client-side

---

## Phase 4: Enhanced Preview & DevEx
**Goal:** Professional-grade preview and editing experience.

| Feature | Description | Priority | Status |
|---------|-------------|----------|--------|
| **Device Simulation** | Toggle mobile/tablet viewport sizes in preview panel | P1 | 🔲 |
| **Split Editor** | Side-by-side code + preview (instead of tab switching) | P1 | 🔲 |
| **Live Reload** | Auto-refresh preview on file changes (for non-active file edits) | P2 | 🔲 |
| **Diff View** | Compare file versions before/after changes | P3 | 🔲 |
| **Import from URL** | Fetch and edit files from raw GitHub/URL | P2 | 🔲 |
| **Export Formats** | Export as single HTML file (inline everything) or GitHub repo structure | P3 | 🔲 |

**Technical Notes:**
- Device simulation: CSS transforms on iframe container
- Split editor: Resizable panes using CSS Grid + draggable splitter
- Diff view: Use `diff-match-patch` library

---

## Phase 5: Persistence & Collaboration
**Goal:** Projects survive browser restarts and can be shared.

| Feature | Description | Priority | Status |
|---------|-------------|----------|--------|
| **localStorage Persistence** | Auto-save project to browser storage with load/save slots | P0 | 🔲 |
| **GitHub Gist Integration** | One-click publish to Gist, import from Gist URL | P1 | 🔲 |
| **Project Templates** | Save/load entire project blueprints as JSON | P2 | 🔲 |
| **Session Recovery** | Restore open tabs and unsaved changes on reload | P2 | 🔲 |
| **Export/Import JSON** | Full project state as portable JSON (for version control) | P2 | 🔲 |

**Technical Notes:**
- localStorage quota: ~5-10MB, compress with `lz-string`
- Gist API requires Personal Access Token (user-provided)
- Session recovery: store tab state + unsaved buffer hash

---

## Architecture Constraints

All enhancements must respect these principles:

1. **Single File** — Everything compiles to `architect-vx.html`
2. **No Build Step** — Users can edit directly and refresh
3. **No Server** — All features work with `file://` protocol
4. **Browser-Native** — ES2023+, File API, localStorage only
5. **Size Budget** — Keep under 2MB total (images embedded as data URIs)

---

## Contributing

When implementing features:

1. Open an issue describing the feature and linking to roadmap item
2. Maintain the existing module pattern (IIFE with revealing module)
3. Follow color system defined in `:root` CSS variables
4. Update this roadmap with status change (🔲 → 🚧 → ✅)
5. Test across Chrome, Firefox, Safari

---

## Backlog (Nice to Have)

- **WASM Support** — Run compiled WASM modules in preview
- **WebRTC Sync** — Real-time collaborative editing (ambitious)
- **Plugin System** — Load external editor extensions
- **Theme Customization** — User-defined color schemes
- **Vim/Emacs Keybindings** — Modal editing options

---

*Last updated: 2026-02-25*
*Maintained by: Ghost Labs*
