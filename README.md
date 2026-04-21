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

## Quick Start

Simply open `architect-v2.html` in any modern browser:

```bash
# No server required, but recommended for full functionality
python -m http.server 8080
# Then open http://localhost:8080/architect-v2.html
```

## License

See the [LICENSE](LICENSE) file for details. 
