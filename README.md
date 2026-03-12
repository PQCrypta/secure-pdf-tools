# Secure PDF Tools

**The Private, Server-Side PDF Toolkit — Free, Fast, and Zero-Retention.**
No uploads to third-party clouds. No accounts. No tracking. Ever.

Every operation runs entirely on the server. Files are processed and immediately deleted — nothing is stored, logged, or shared.

**Live:** [pqcrypta.com/pdf](https://pqcrypta.com/pdf)

---

## Tools (28 Operations)

### Core Manipulation

| Tool | Link | Description |
|---|---|---|
| **Merge PDFs** | [/pdf/tools/merge.php](https://pqcrypta.com/pdf/tools/merge.php) | Combine up to 20 PDFs into one. Drag thumbnails to reorder pages before merging. |
| **Split PDF** | [/pdf/tools/split.php](https://pqcrypta.com/pdf/tools/split.php) | Split by every page, fixed interval, custom page ranges, or interactive cut-point selection. |
| **Reorder Pages** | [/pdf/tools/reorder.php](https://pqcrypta.com/pdf/tools/reorder.php) | Drag-and-drop page thumbnails to rearrange, then export. |
| **Delete Pages** | [/pdf/tools/delete-pages.php](https://pqcrypta.com/pdf/tools/delete-pages.php) | Click thumbnail grid to select pages for removal. |
| **Extract Pages** | [/pdf/tools/extract-pages.php](https://pqcrypta.com/pdf/tools/extract-pages.php) | Click thumbnail grid to select pages to keep; auto-compresses ranges (e.g. 1-3,5,7-9). |
| **Rotate Pages** | [/pdf/tools/rotate.php](https://pqcrypta.com/pdf/tools/rotate.php) | Rotate all, odd, even, or a custom range of pages. Supports 90°/180°/270° and arbitrary decimal angles. Live canvas preview of the first page. |
| **Compress PDF** | [/pdf/tools/compress.php](https://pqcrypta.com/pdf/tools/compress.php) | Five quality presets plus custom DPI slider (50–600). Optional metadata stripping, linearization, and stream recompression. Live before/after split-canvas preview rendered from page 1 when a file is selected — original on the left, simulated compressed on the right. Shows original vs. compressed size after download. |
| **Repair PDF** | [/pdf/tools/repair.php](https://pqcrypta.com/pdf/tools/repair.php) | Reconstruct corrupted or malformed PDFs via Ghostscript. |
| **Flatten PDF** | [/pdf/tools/flatten.php](https://pqcrypta.com/pdf/tools/flatten.php) | Flatten form fields and annotations into the page content layer. |
| **Grayscale / B&W** | [/pdf/tools/grayscale.php](https://pqcrypta.com/pdf/tools/grayscale.php) | Convert to grayscale or pure black-and-white. Live before/after split-canvas preview rendered from page 1 — color on the left, grayscale simulation on the right. |

### Format Conversion

| Tool | Link | Description |
|---|---|---|
| **PDF → Word** | [/pdf/tools/convert.php](https://pqcrypta.com/pdf/tools/convert.php) | Export to editable `.docx`, `.odt`, `.rtf`, or `.txt` via LibreOffice. Format fidelity indicator shows star ratings (out of 4) for all four formats, highlighting the active selection with a description of expected quality. |
| **PDF → Excel** | [/pdf/tools/pdf-to-excel.php](https://pqcrypta.com/pdf/tools/pdf-to-excel.php) | Export tables to `.xlsx` via LibreOffice. |
| **PDF → Images** | [/pdf/tools/to-images.php](https://pqcrypta.com/pdf/tools/to-images.php) | Render pages to PNG or JPEG at 72–600 DPI. Select all pages or a custom range. JPEG quality slider when JPEG format is chosen. Download as ZIP. |
| **PDF/A Archive** | [/pdf/tools/pdfa.php](https://pqcrypta.com/pdf/tools/pdfa.php) | Convert to PDF/A-1b, PDF/A-2b, or PDF/A-3b for long-term archival. |
| **Word → PDF** | [/pdf/tools/word-to-pdf.php](https://pqcrypta.com/pdf/tools/word-to-pdf.php) | Convert `.doc` / `.docx` / `.odt` via LibreOffice. Format fidelity indicator shows expected output quality for the uploaded file type. |
| **Excel → PDF** | [/pdf/tools/excel-to-pdf.php](https://pqcrypta.com/pdf/tools/excel-to-pdf.php) | Convert `.xls` / `.xlsx` / `.ods` via LibreOffice. Sheet selector — fetches sheet names from the uploaded file and lets you pick which sheet(s) to convert. |
| **PowerPoint → PDF** | [/pdf/tools/ppt-to-pdf.php](https://pqcrypta.com/pdf/tools/ppt-to-pdf.php) | Convert `.ppt` / `.pptx` / `.odp` via LibreOffice. Slide selector — fetches slide titles from the uploaded file and lets you choose which slides to include. |
| **Images → PDF** | [/pdf/tools/image-to-pdf.php](https://pqcrypta.com/pdf/tools/image-to-pdf.php) | Pack JPEG / PNG / WebP / BMP / TIFF images into a single PDF via ImageMagick. |
| **HTML → PDF** | [/pdf/tools/html-to-pdf.php](https://pqcrypta.com/pdf/tools/html-to-pdf.php) | Upload `.html` / `.htm` and convert via LibreOffice. |

### Protection & Security

| Tool | Link | Description |
|---|---|---|
| **Protect PDF** | [/pdf/tools/protect.php](https://pqcrypta.com/pdf/tools/protect.php) | Dual-mode protection: **Standard** (AES-256-CBC server-side) or **PQC** (client-side quantum-safe encryption). See details below. |
| **Unlock PDF** | [/pdf/tools/unlock.php](https://pqcrypta.com/pdf/tools/unlock.php) | Remove password protection (owner password required). |
| **Redact PDF** | [/pdf/tools/redact.php](https://pqcrypta.com/pdf/tools/redact.php) | Two modes: text-pattern redaction (with multi-pattern list, case sensitivity, whole-word matching) or mouse-drawn region redaction on a canvas preview. Custom fill colour. |

### Content & Annotation

| Tool | Link | Description |
|---|---|---|
| **Add Watermark** | [/pdf/tools/watermark.php](https://pqcrypta.com/pdf/tools/watermark.php) | Stamp text watermarks. 8-position placement, opacity, rotation angle, font size, font style, hex colour. Apply to all, odd, even, or custom page ranges. Live canvas preview — page 1 is rendered and the watermark text is drawn over it in real time as you adjust text, opacity, size, colour, and position. |
| **Sign PDF** | [/pdf/tools/sign.php](https://pqcrypta.com/pdf/tools/sign.php) | Three signature input methods: draw (canvas with touch support), type (text-rendered), or upload an image. Place on first/last/custom page with x/y/size controls. Optional cryptographic metadata. |
| **Edit PDF** | [/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php) | Full page-by-page visual editor with 14 annotation tools (see below). |
| **Compare PDFs** | [/pdf/tools/compare.php](https://pqcrypta.com/pdf/tools/compare.php) | Visual diff of two PDFs. Configure DPI (72/96/150/300) and sensitivity. Side-by-side page 1 canvas previews render immediately when each file is selected. Outputs a highlighted diff PDF with change regions marked. |
| **Extract Text** | [/pdf/tools/extract-text.php](https://pqcrypta.com/pdf/tools/extract-text.php) | Export all text to `.txt`. Options: layout preservation, text encoding, custom page range. |
| **PDF Info** | [/pdf/tools/pdf-info.php](https://pqcrypta.com/pdf/tools/pdf-info.php) | Display full metadata: title, author, subject, keywords, creator, producer, page count, dimensions, PDF version, encryption status, form type, tagged flag, page rotation, fast web view optimisation, creation and modification dates, permission flags. Shows a quick canvas preview of page 1 alongside the metadata. |

### Automation

| Tool | Link | Description |
|---|---|---|
| **Workflow Builder** | [/pdf/tools/workflow.php](https://pqcrypta.com/pdf/tools/workflow.php) | Chain operations visually: add steps from the picker, configure per-step parameters, drag to reorder. Save named workflows to localStorage; **Load** replaces current steps, **+ Append** joins a saved workflow onto the end of the current one — enabling complex pipelines composed from saved building blocks. Export / import workflows as JSON. Runs the full sequence on one or more uploaded PDFs. |

---

## Edit PDF — 14 Annotation Tools

[/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php)

The editor renders each page to a canvas and applies all changes server-side via PyMuPDF.

| Tool | Description |
|---|---|
| **Text** | Click to place a text box. Font family (Helvetica / Times / Courier), font size, bold, italic, left/centre/right alignment, colour. |
| **Draw** | Freehand pen with configurable line width (slider with live preview) and colour. |
| **Line** | Straight line with stroke width and colour. |
| **Arrow** | Directional arrow annotation. |
| **Rectangle** | Rectangle with fill/outline toggle, independent fill colour, fill opacity, stroke width, and colour. |
| **Ellipse** | Ellipse with fill/outline toggle, independent fill colour, fill opacity, stroke width, and colour. |
| **Highlight** | Translucent highlight overlay with configurable opacity. |
| **Whiteout** | Solid white box to cover content. |
| **Strikethrough** | Strikethrough line over selected text area. |
| **Underline** | Underline over selected text area. |
| **Image** | Upload and position an image on the page. |
| **Signature** | Open a signature canvas modal (draw with mouse or touch), place result on page. |
| **QR Code** | Generate a QR code from any URL or text string (via `edit-qr-generate` API), set size, and place on page. |
| **Stamp** | Insert one of 12 built-in stamps (DRAFT, APPROVED, REJECTED, CONFIDENTIAL, TOP SECRET, VOID, COPY, FINAL, REVISED, REVIEW, NOT APPROVED, PAID) or type a custom stamp text. |

### Additional Edit Features

- **Page numbering** — auto-add page numbers with position (top/bottom/left/right), format (1 / Page 1 / 1 of 10), start number, font size, and colour
- **Headers & footers** — insert header and footer text with alignment, font size, and colour
- **Insert blank page** — add a blank page before or after the current page; also supports creating a new blank PDF from scratch
- **Page rotation** — per-page rotation in degrees
- **Undo / redo** — per-page history stack
- **Zoom controls** — adjustable canvas zoom
- **Stroke style** — solid, dashed, dotted, or dash-dot per annotation
- **Annotation opacity** — global opacity slider applied to highlights and filled shapes
- **Preferences persistence** — last-used tool, colour, font family, font size, line width, fill mode, and zoom are saved in a cookie

---

## Protect PDF — Dual Encryption Modes

[/pdf/tools/protect.php](https://pqcrypta.com/pdf/tools/protect.php)

### Standard Mode (AES-256-CBC, server-side)

- Open password (user password)
- Owner password
- Granular permission flags: print, copy, modify, annotate, form fill, accessibility, assembly

### PQC Mode (client-side quantum-safe)

- Sub-modes: key-pair or passphrase
- 31 post-quantum algorithms (see table below)
- Key generation in-browser via `@noble/post-quantum`
- Private key display with copy-to-clipboard and download buttons
- Encryption runs client-side before any data leaves the browser

---

## Post-Quantum Encryption (31 Algorithms)

| Category | Algorithms |
|---|---|
| NIST Standards | `ml-kem-1024`, `post-quantum`, `hybrid`, `classical` |
| NIST 2025 Code-Based | `hqc-128`, `hqc-192`, `hqc-256` |
| FN-DSA Stack | `fn-dsa-512-compact`, `fn-dsa-1024-security`, `fn-dsa-dual-signature`, `fn-dsa-fp-hardened`, `fn-dsa-transition-stack`, `fn-dsa-zk-stack` |
| Max-Secure Variants | `max-secure-crypto-agile`, `max-secure-hybrid-transition`, `max-secure-lightweight`, `max-secure-pqc-zk`, `max-secure-pure-pq`, `max-secure-stateless` |
| Multi-Algorithm | `multi-algorithm`, `multi-kem`, `multi-kem-triple`, `lattice-code-hybrid` |
| Advanced | `ai-synthesized-crypto-agile`, `entropy-orchestrated`, `quantum-lattice-fusion`, `quantum-resistant-consensus`, `pq3-stack`, `quad-layer`, `post-zk-homomorphic` |

---

## Sign PDF — Three Input Methods

[/pdf/tools/sign.php](https://pqcrypta.com/pdf/tools/sign.php)

| Method | Description |
|---|---|
| **Draw** | Freehand signature on a canvas. Full touch support for mobile. Clear and redraw. |
| **Type** | Typed name rendered as a signature image via ImageMagick. |
| **Upload** | Upload a pre-drawn signature image. |

**Placement controls:** page (first / last / custom page number), horizontal position, vertical position, signature size (points).

**Cryptographic metadata (optional, expandable):** signer name, email, reason, location, date stamp. Supports auto-generated or custom certificate (`cert_source: auto | own`, `cert_file`, `cert_password`).

---

## Split PDF — Four Split Modes

[/pdf/tools/split.php](https://pqcrypta.com/pdf/tools/split.php)

| Mode | Parameter | Description |
|---|---|---|
| All pages | `split_mode=all` | One file per page |
| Custom ranges | `split_mode=range` + `page_ranges` | Comma-separated ranges e.g. `1-3,5,7-9` |
| Interval | `split_mode=interval` + `interval` | Every N pages |
| Cut points | `split_mode=custom` + `split_after` | Click scissors (✂) between page thumbnails in the preview to set split points |

---

## Watermark — Position Options

[/pdf/tools/watermark.php](https://pqcrypta.com/pdf/tools/watermark.php)

`position`: `diagonal` | `center` | `top-left` | `top-right` | `bottom-left` | `bottom-right`

`pages`: `all` | `odd` | `even` | `range` (+ `page_range`)

Additional parameter: `font_style`

---

## Redact PDF — Two Modes

[/pdf/tools/redact.php](https://pqcrypta.com/pdf/tools/redact.php)

**Text pattern mode:**
- Add multiple patterns to a list; each can be removed individually
- `case_sensitive` toggle
- `whole_word` toggle
- `fill_color` — hex colour for redaction boxes

**Region drawing mode:**
- Draw rectangles directly on a canvas page preview to mark redaction areas

---

## Rotate PDF — Page Selection

[/pdf/tools/rotate.php](https://pqcrypta.com/pdf/tools/rotate.php)

`pages`: `all` | `odd` | `even` | `range` (+ `page_range`)

`angle`: `90` | `180` | `270` | any decimal value (custom mode)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Server | PHP 8.4 |
| HTTP | Apache with HTTP/2 |
| Styling | Vanilla CSS (CSS variables, Grid, custom animations) |
| Scripts | Vanilla ES6 JavaScript modules (`type="module"`, no framework) |
| PDF engines | Ghostscript, Poppler, qpdf, LibreOffice, PyMuPDF, ImageMagick |
| PQC crypto | `@noble/post-quantum` |
| PDF.js | Mozilla PDF.js (page preview rendering in-browser) |
| Colour theme | Amber `#ff8c00` + gold `#ffd700` on deep navy `#040810` |

---

## File Structure

```
pdf/
├── index.php                  # Hub — tool cards, search, drag-drop, IndexedDB file transfer
├── api.php                    # Single POST endpoint — all 37 operations
├── css/
│   ├── pdf.css                # Complete UI styles
│   ├── pdf-background.css     # Canvas container
│   └── pdf-cursor.css         # Ink trail cursor
├── js/
│   ├── pdf.js                 # Hub init, search, drag-drop routing
│   ├── pdf-background.js      # Floating docs + particle animation
│   ├── pdf-cursor.js          # Cursor ink trail
│   ├── pdf-processing.js      # Global 3D processing overlay + cross-page hub-drop file delivery via IndexedDB
│   ├── pdf-page-preview.js    # Shared ES module: PdfPagePreview, PdfSplitPreview, PdfReorderPreview, PdfMergePreview, renderSinglePagePreview
│   └── tools/                 # All tool scripts are ES modules (type="module")
│       ├── upload.js          # PdfUploadUtil — shared XHR upload handler
│       ├── merge.js           # Thumbnail preview + drag reorder
│       ├── split.js           # Cut-point preview + range/interval modes
│       ├── compress.js        # DPI slider, before/after split-canvas preview, size comparison
│       ├── convert.js         # PDF → Word/ODT/RTF/TXT + format fidelity star indicator
│       ├── watermark.js       # 8-position, per-page, font style, live canvas watermark preview
│       ├── sign.js            # Draw/type/upload tabs, canvas, crypto metadata
│       ├── rotate.js          # Canvas preview, odd/even/range, decimal angles
│       ├── protect.js         # Dual-mode AES + PQC, key management
│       ├── unlock.js
│       ├── to-images.js       # Format, DPI, JPEG quality, page range
│       ├── extract-text.js    # Layout, encoding, page range
│       ├── extract-pages.js   # Thumbnail selection, range compression
│       ├── pdf-info.js        # Full metadata display + quick page 1 canvas preview
│       ├── flatten.js
│       ├── grayscale.js       # Before/after split-canvas preview (color vs. grayscale)
│       ├── repair.js
│       ├── reorder.js         # Drag-and-drop thumbnail grid
│       ├── delete-pages.js    # Thumbnail click selection
│       ├── pdfa.js            # Level selector (1b/2b/3b)
│       ├── word-to-pdf.js     # Format fidelity indicator
│       ├── excel-to-pdf.js    # Sheet selector (fetches sheet names from uploaded file)
│       ├── ppt-to-pdf.js      # Slide selector (fetches slide titles from uploaded file)
│       ├── image-to-pdf.js
│       ├── pdf-to-excel.js
│       ├── html-to-pdf.js
│       ├── redact.js          # Text patterns + region drawing mode
│       ├── compare.js         # Side-by-side page 1 canvas previews, DPI + sensitivity controls
│       ├── edit.js            # 14-tool canvas editor, bold/italic text, fill opacity, annotation opacity
│       └── workflow.js        # Visual step builder, drag reorder
├── scripts/                   # Python helpers (PyMuPDF operations)
└── tools/                     # PHP tool pages
    ├── _tool_head.php         # Shared header (CSP nonces, nav with PDF Home link)
    ├── _tool_foot.php         # Shared footer (cache-busted pdf-processing.js)
    ├── merge.php
    ├── split.php
    ├── compress.php
    ├── convert.php
    ├── watermark.php
    ├── sign.php
    ├── rotate.php
    ├── protect.php
    ├── unlock.php
    ├── to-images.php
    ├── extract-text.php
    ├── extract-pages.php
    ├── pdf-info.php
    ├── flatten.php
    ├── grayscale.php
    ├── repair.php
    ├── reorder.php
    ├── delete-pages.php
    ├── pdfa.php
    ├── word-to-pdf.php
    ├── excel-to-pdf.php
    ├── ppt-to-pdf.php
    ├── image-to-pdf.php
    ├── pdf-to-excel.php
    ├── html-to-pdf.php
    ├── redact.php
    ├── compare.php
    ├── edit.php
    └── workflow.php
```

---

## License

Copyright © PQ Crypta. All rights reserved.
