# Project Architect // Ghost Labs

A single-file visual tool for designing project file structures, editing code with live preview, and exporting as ZIP archives.

## Overview

Project Architect is a browser-based structural tool that enables rapid prototyping of project architectures. It provides a visual tree interface for creating folders and files, a full-featured code editor with syntax-aware editing, live HTML preview with console interception, and one-click ZIP export.

**Key Capabilities:**
- Visual file/folder tree management with drag-and-drop reordering
- Full-screen code editor with line numbers, auto-save, and tab-to-spaces
- Live HTML preview with inline resource resolution (JS/CSS)
- Ghost Console HUD that intercepts iframe console messages
- Boilerplate injection system for common code patterns
- Import from indented text format (tree-style)
- Export entire structure as ZIP file

## Technology Stack

| Category | Technology |
|----------|------------|
| **Core** | Vanilla JavaScript ES2023+ (no build step) |
| **Styling** | TailwindCSS v3 (via CDN) |
| **External Dependencies** | JSZip 3.10.1 (ZIP generation), Google Fonts (Space Mono, Syne) |
| **Runtime** | Any modern browser with File API and ES6 support |

## Project Structure

This is a **single-file application**. All HTML, CSS, and JavaScript are contained in:

```
architect-v2.html    # Complete application (~1680 lines)
```

No build tools, bundlers, or package managers are required.

## Module Architecture

The JavaScript is organized into self-contained modules using IIFE pattern:

| Module | Purpose |
|--------|---------|
| `StateManager` | Single source of truth for file structure. Folders = objects, Files = strings |
| `FileSystemUtils` | Path manipulation, sanitization, extension detection |
| `ImportParser` | Parses indented text (2 spaces or tabs) into nested objects |
| `Boilerplates` | Template library for code injection (HTML shell, ES modules, Web Audio, etc.) |
| `DragDropManager` | HTML5 drag-and-drop for tree reordering |
| `TreeRenderer` | Pure DOM renderer - creates visual tree from state |
| `EditorController` | Ghost Editor lifecycle: open, edit, auto-save, close |
| `PreviewController` | Blob URL materializer for HTML preview with resource inlining |
| `ConsoleService` | iframe console interceptor via postMessage |
| `Toast` | Notification utility |
| `UIController` | Event wiring, lifecycle bootstrap, user actions |

## Data Model

The file structure uses a simple object-based tree:

```javascript
// Folders are objects
{ 'src': { 'components': {}, 'app.js': '' } }

// Files are strings (content)
{ 'README.md': '# Project' }
```

Paths use `/` delimiter. Root path is `/`. Current path and selected path are tracked separately.

## Code Style Guidelines

### JavaScript Conventions
- **Module Pattern**: IIFE with revealing module pattern
- **Naming**: PascalCase for modules, camelCase for methods/variables, `_prefix` for private
- **Quotes**: Single quotes for strings, backticks for templates
- **Formatting**: 4-space indentation, semicolons required
- **Comments**: Section dividers with `// ----`, JSDoc for public methods
- **Strict Equality**: Always use `===` and `!==`

### CSS Conventions
- **Custom Properties**: CSS variables defined in `:root` for design tokens
- **Naming**: kebab-case for classes
- **Structure**: Organized by component sections with visual dividers
- **Shadow Effects**: Neumorphic design using `box-shadow` combinations

### Color System (Design Tokens)
```css
--bg:        #0e0e10    /* Background */
--surface:   #161618    /* Card surfaces */
--raised:    #1c1c1f    /* Elevated elements */
--border:    rgba(255,255,255,0.06)
--coral:     #FF6B47    /* Primary action */
--gold:      #E8C547    /* Secondary/Accent */
--purple:    #8B6FE8    /* Selection/Focus */
--green:     #4ADE80    /* Success */
--red:       #F87171    /* Error */
--dim:       #5a5a6a    /* Muted text */
--text:      #d4d4df    /* Primary text */
--text-muted:#7a7a8a    /* Secondary text */
```

## Development Workflow

### Running Locally

Simply open `architect-v2.html` in any modern browser:

```bash
# No server required, but recommended for full functionality
python -m http.server 8080
# Then open http://localhost:8080/architect-v2.html
```

### Making Changes

1. Edit `architect-v2.html` directly
2. Refresh browser to test
3. No build step or compilation required

### Adding New Boilerplate Templates

To add a new template to the inject bar:

1. Add entry to `Boilerplates` object (around line 863):
```javascript
'template-key': `/* template code */`,
```

2. Add button to inject bar in HTML (around line 589):
```html
<button class="btn-template" data-template="template-key">Label</button>
```

## Testing Strategy

No automated test suite exists. Testing is manual:

1. **File Operations**: Create folders/files, verify tree updates
2. **Drag & Drop**: Reorder nodes, verify state persistence
3. **Editor**: Open files, edit content, verify auto-save (400ms debounce)
4. **Preview**: Edit HTML, switch to preview tab, verify render
5. **Console**: Trigger console messages in preview, verify Ghost Console capture
6. **Import/Export**: Test tree text import, verify ZIP export contents

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Delete` / `Backspace` | Delete selected item (when not in input) |
| `Enter` (in folder input) | Create folder |
| `Enter` (in file input) | Create file |
| `Escape` | Close import modal |
| `Cmd/Ctrl + S` (in editor) | Explicit save + toast feedback |
| `Tab` (in editor) | Insert 2 spaces |

## Security Considerations

- **CSP-Safe**: Preview uses `postMessage` for console interception (no inline eval)
- **Sandboxed Preview**: iframe uses `sandbox="allow-scripts allow-same-origin"`
- **No Persistence**: All data stored in memory only (no localStorage)
- **Input Sanitization**: File/folder names sanitized to `[a-zA-Z0-9._\-]`
- **XSS Mitigation**: Console output HTML-escaped in Ghost Console

## Browser Compatibility

- Chrome 80+
- Firefox 75+
- Safari 13.1+
- Edge 80+

Requires support for:
- ES2020+ (optional chaining would work but isn't used)
- CSS Custom Properties
- CSS Grid and Flexbox
- File API and Blob URLs
- Drag and Drop API

## Future Extension Points

- Add more boilerplate templates to `Boilerplates`
- Extend file icon mapping in `TreeRenderer._fileIcon()`
- Add syntax highlighting via Prism.js or highlight.js
- Add localStorage persistence option
- Add GitHub Gist import/export
- Add multiple file editing with tabs

## Improvement Suggestions

### 1. Enhanced Editor Features
- **Syntax Highlighting**: Integrate a lightweight syntax highlighter like Prism.js to improve code readability
- **Line Numbers**: Currently present, but could include line highlighting for errors/warnings
- **Auto-completion**: Implement basic code completion for common elements
- **Bracket Matching**: Visual indication of matching parentheses, brackets, and braces
- **Multiple File Tabs**: Allow opening multiple files simultaneously in separate tabs

### 2. Improved File Management
- **Search and Replace**: Add global search across all files and find/replace within files
- **File Filtering**: Ability to filter files in the tree view
- **Recent Files**: Track recently opened files for quick access
- **File Templates**: Expand boilerplate templates to include more project types (React, Vue, Node.js, etc.)

### 3. Advanced Preview Capabilities
- **Device Simulation**: Add mobile/tablet preview modes
- **Split View**: Allow side-by-side editing and preview
- **Live Reload**: Auto-refresh preview when files are modified
- **Console Enhancements**: Better filtering and organization of console messages

### 4. Project Persistence
- **Local Storage**: Save projects to browser's local storage for persistence
- **Cloud Integration**: Add GitHub Gist integration for sharing projects
- **Version Control**: Simple version history within the editor
- **Project Templates**: Save and load complete project templates

### 5. Developer Experience Improvements
- **Keyboard Shortcuts**: Add more shortcuts for common actions
- **Snippets**: Code snippet support for faster development
- **Error Detection**: Basic syntax error checking without a full linter
- **Performance Monitoring**: Show performance metrics for the generated projects

### 6. UI/UX Enhancements
- **Dark/Light Theme**: Option to switch between themes
- **Customizable Layout**: Allow users to adjust panel sizes
- **Better File Icons**: More descriptive icons for different file types
- **Accessibility**: Improve keyboard navigation and screen reader support

### 7. Collaboration Features
- **Export Options**: Additional export formats (Git repository structure, different archive formats)
- **Sharing**: Easy sharing mechanisms for collaborative work
- **Import Enhancements**: Import from GitHub repositories or other sources
