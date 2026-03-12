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
| **Compress PDF** | [/pdf/tools/compress.php](https://pqcrypta.com/pdf/tools/compress.php) | Five quality presets plus custom DPI slider (50–600). Optional metadata stripping, linearization, and stream recompression. Shows original vs. compressed size after download. |
| **Repair PDF** | [/pdf/tools/repair.php](https://pqcrypta.com/pdf/tools/repair.php) | Reconstruct corrupted or malformed PDFs via Ghostscript. |
| **Flatten PDF** | [/pdf/tools/flatten.php](https://pqcrypta.com/pdf/tools/flatten.php) | Flatten form fields and annotations into the page content layer. |
| **Grayscale / B&W** | [/pdf/tools/grayscale.php](https://pqcrypta.com/pdf/tools/grayscale.php) | Convert to grayscale or pure black-and-white. |

### Format Conversion

| Tool | Link | Description |
|---|---|---|
| **PDF → Word** | [/pdf/tools/convert.php](https://pqcrypta.com/pdf/tools/convert.php) | Export to editable `.docx` via LibreOffice. |
| **PDF → Excel** | [/pdf/tools/pdf-to-excel.php](https://pqcrypta.com/pdf/tools/pdf-to-excel.php) | Export tables to `.xlsx` via LibreOffice. |
| **PDF → Images** | [/pdf/tools/to-images.php](https://pqcrypta.com/pdf/tools/to-images.php) | Render pages to PNG or JPEG at 72–600 DPI. Select all pages or a custom range. JPEG quality slider when JPEG format is chosen. Download as ZIP. |
| **PDF/A Archive** | [/pdf/tools/pdfa.php](https://pqcrypta.com/pdf/tools/pdfa.php) | Convert to PDF/A-1b, PDF/A-2b, or PDF/A-3b for long-term archival. |
| **Word → PDF** | [/pdf/tools/word-to-pdf.php](https://pqcrypta.com/pdf/tools/word-to-pdf.php) | Convert `.doc` / `.docx` / `.odt` via LibreOffice. |
| **Excel → PDF** | [/pdf/tools/excel-to-pdf.php](https://pqcrypta.com/pdf/tools/excel-to-pdf.php) | Convert `.xls` / `.xlsx` / `.ods` via LibreOffice. |
| **PowerPoint → PDF** | [/pdf/tools/ppt-to-pdf.php](https://pqcrypta.com/pdf/tools/ppt-to-pdf.php) | Convert `.ppt` / `.pptx` / `.odp` via LibreOffice. |
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
| **Add Watermark** | [/pdf/tools/watermark.php](https://pqcrypta.com/pdf/tools/watermark.php) | Stamp text watermarks. 8-position placement, opacity, rotation angle, font size, font style, hex colour. Apply to all, odd, even, or custom page ranges. |
| **Sign PDF** | [/pdf/tools/sign.php](https://pqcrypta.com/pdf/tools/sign.php) | Three signature input methods: draw (canvas with touch support), type (text-rendered), or upload an image. Place on first/last/custom page with x/y/size controls. Optional cryptographic metadata. |
| **Edit PDF** | [/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php) | Full page-by-page visual editor with 13 annotation tools (see below). |
| **Compare PDFs** | [/pdf/tools/compare.php](https://pqcrypta.com/pdf/tools/compare.php) | Visual diff of two PDFs. Configure DPI (72/96/150/300) and sensitivity. Side-by-side output with highlighted change regions. |
| **Extract Text** | [/pdf/tools/extract-text.php](https://pqcrypta.com/pdf/tools/extract-text.php) | Export all text to `.txt`. Options: layout preservation, text encoding, custom page range. |
| **PDF Info** | [/pdf/tools/pdf-info.php](https://pqcrypta.com/pdf/tools/pdf-info.php) | Display full metadata: title, author, subject, keywords, creator, producer, page count, dimensions, PDF version, encryption status, form type, tagged flag, page rotation, fast web view optimisation, creation and modification dates, permission flags. |

### Automation

| Tool | Link | Description |
|---|---|---|
| **Workflow Builder** | [/pdf/tools/workflow.php](https://pqcrypta.com/pdf/tools/workflow.php) | Chain operations visually: add steps from a dropdown, configure per-step parameters, drag to reorder, run the full sequence on one file in a single submission. |

---

## Edit PDF — 13 Annotation Tools

[/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php)

The editor renders each page to a canvas and applies all changes server-side via PyMuPDF.

| Tool | Description |
|---|---|
| **Text** | Click to place a text box. Font family (Helvetica / Times / Courier), font size, colour. |
| **Draw** | Freehand pen with configurable line width (slider with live preview) and colour. |
| **Line** | Straight line with stroke width and colour. |
| **Arrow** | Directional arrow annotation. |
| **Rectangle** | Rectangle with fill/outline toggle, stroke width, and colour. |
| **Ellipse** | Ellipse with fill/outline toggle, stroke width, and colour. |
| **Highlight** | Translucent highlight overlay. |
| **Whiteout** | Solid white box to cover content. |
| **Strikethrough** | Strikethrough line over selected text area. |
| **Underline** | Underline over selected text area. |
| **Image** | Upload and position an image on the page. |
| **Signature** | Open a signature canvas modal (draw with mouse or touch), place result on page. |
| **QR Code** | Generate a QR code from any URL or text string (via `edit-qr-generate` API), set size, and place on page. |

### Additional Edit Features

- **Page numbering** — auto-add page numbers with position (top/bottom/left/right), format (1 / Page 1 / 1 of 10), start number, font size, and colour
- **Headers & footers** — insert header and footer text with alignment, font size, and colour
- **Insert blank page** — add a blank page before or after the current page
- **Page rotation** — per-page rotation in degrees
- **Undo / redo** — per-page history stack
- **Zoom controls** — adjustable canvas zoom
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

Live canvas preview of the first page updates as angle is selected.

---

## API

All operations use a single endpoint:

```
POST /pdf/api.php
Content-Type: multipart/form-data
```

### Common Parameters

| Parameter | Type | Description |
|---|---|---|
| `operation` | string | Operation name (see full list below) |
| `file` | file | Primary PDF input |
| `files[]` | file[] | Multiple files (merge, image-to-pdf) |

### All Operations

```
merge           split           split_chunk     compress        convert
watermark       sign            rotate          protect         unlock
to-images       extract-text    extract-pages   get-info        flatten
grayscale       repair          reorder         delete-pages    pdfa
workflow        word-to-pdf     excel-to-pdf    ppt-to-pdf      image-to-pdf
pdf-to-excel    html-to-pdf     redact          compare
edit-init       edit-page       edit-apply      edit-qr-generate
```

> `edit-page` and `edit-qr-generate` are exempt from rate limiting.

### Operation Parameters

**compress**
- `quality` — `screen` (72 DPI) | `ebook` (150 DPI) | `printer` (300 DPI) | `prepress` (300 DPI + colour) | `custom`
- `custom_dpi` — 50–600 (when `quality=custom`)
- `strip_metadata` — `1`
- `linearize` — `1` (fast web view via qpdf)
- `recompress` — `1` (recompress streams via qpdf)
- Response headers: `X-Original-Size`, `X-Compressed-Size`

**split**
- `split_mode` — `all` | `range` | `interval` | `custom`
- `page_ranges` — e.g. `1-3,5,7-9` (range mode)
- `interval` — pages per chunk (interval mode)
- `split_after` — comma-separated page numbers (custom cut-point mode)

**rotate**
- `angle` — `90` | `180` | `270` | any decimal
- `pages` — `all` | `odd` | `even` | `range`
- `page_range` — e.g. `1-3,5` (when `pages=range`)

**watermark**
- `text`, `font_size` (8–200), `font_style`, `opacity` (0.0–1.0), `angle`, `color` (hex)
- `position` — `diagonal` | `center` | `top-left` | `top-right` | `bottom-left` | `bottom-right`
- `pages` — `all` | `odd` | `even` | `range`
- `page_range`

**protect**
- `user_password`, `owner_password`
- `allow_print` | `allow_copy` | `allow_modify` | `allow_annotate` | `allow_forms` | `allow_accessibility` | `allow_assembly`
- `pqc_algorithm` — PQC algorithm ID
- `pqc-submode` — `keys` | `password`
- `pqc_passphrase`

**sign**
- `sign_method` — `draw` | `type` | `upload`
- `page` — `first` | `last` | `custom`
- `page_number`, `pos_x`, `pos_y`, `sig_size`
- `sign_crypto` — `1` for cryptographic metadata
- `signer_name` (max 100), `signer_email`, `signer_reason` (max 200), `signer_location`
- `cert_source` — `auto` | `own`
- `cert_file`, `cert_password`

**to-images**
- `format` — `png` | `jpeg`
- `dpi` — 72–600
- `jpeg_quality` — JPEG quality (when `format=jpeg`)
- `pages` — `all` | `range`
- `page_range`

**grayscale**
- `mode` — `grayscale` | `bw`

**redact**
- `pattern`, `redact_all`
- `case_sensitive` — `1`
- `whole_word` — `1`
- `fill_color` — hex colour

**reorder**
- `page_order` — comma-separated new order e.g. `3,1,2,4`

**delete-pages** / **extract-pages**
- `pages` or `page_range` — comma-separated numbers or ranges

**compare**
- `dpi` — `72` | `96` | `150` | `300`
- `sensitivity` — diff sensitivity level

**extract-text**
- `preserve_layout` — `1`
- `text_encoding`
- `pages` — `all` | `custom`
- `page_range`

**pdfa**
- `pdfa_level` — `1b` | `2b` | `3b`

**workflow**
- `steps` — JSON array: `[{"op":"compress","quality":"ebook"},{"op":"watermark","text":"DRAFT"}]`
- Chainable ops: `rotate`, `compress`, `watermark`, `protect`, `unlock`, `grayscale`, `flatten`, `repair`, `extract-pages`, `delete-pages`, `reorder`, `pdfa`

**edit-init**
- Returns page count and dimensions for the editor

**edit-apply**
- Applies all canvas annotation layers to the PDF

**edit-qr-generate**
- `qr_text` — text or URL to encode (exempt from rate limiting)

### Response Format

Success: `application/pdf` or `application/zip` as a direct file download.

Error:
```json
{ "error": "Description" }
```

Rate limit: HTTP 429.

---

## Limits & Constraints

| Constraint | Value |
|---|---|
| Max file size | 50 MB (52,428,800 bytes) |
| Max total upload | 200 MB (209,715,200 bytes) |
| Merge: max files | 20 |
| Rate limit | 10 ops per 5 minutes (session-based) |
| Rate-limit exempt | `edit-page`, `edit-qr-generate` |
| Operation timeout | 120 seconds |
| Edit operation timeout | 8 seconds |
| Office formats | `.docx`, `.doc`, `.ppt`, `.pptx`, `.xls`, `.xlsx`, `.odt`, `.odp`, `.ods` |
| Image formats | JPEG, PNG, WebP, BMP, TIFF, GIF |

---

## Backend Engines

| Engine | Used For |
|---|---|
| **Ghostscript** | compress, protect (AES-256), rotate, flatten, grayscale, B&W, repair, pdfa |
| **Poppler** (`pdfunite`, `pdftoppm`, `pdftotext`, `pdfinfo`) | merge, split, to-images, extract-text, get-info |
| **qpdf** | linearization, stream recompression, unlock |
| **LibreOffice** | pdf→word, pdf→excel, word→pdf, excel→pdf, ppt→pdf, html→pdf |
| **PyMuPDF (fitz)** | watermark, redact, sign, edit, compare |
| **ImageMagick** | signature image rendering, image→pdf |
| **Python** | PyMuPDF orchestration, QR code generation |

---

## Security

- **Zero retention** — all uploaded and output files are deleted immediately after the response. Temp files are cleaned up in every error path.
- **No accounts** — no login, no registration, no user data stored.
- **No tracking** — no analytics, no third-party scripts, no persistent cookies beyond session rate-limit state.
- **Magic byte validation** — files are validated against actual binary signatures (`%PDF` header check), not MIME type alone.
- **AES-256-CBC** for standard PDF password protection.
- **31 post-quantum algorithms** for PQC protection mode (client-side).
- **CSP enforcement** — per-request nonces, no inline styles, no inline scripts, no inline event handlers.
- **Input sanitisation** — all strings are sanitised and length-capped before reaching shell commands.
- **No shell injection** — all CLI arguments pass through `escapeshellarg()` / `escapeshellcmd()`.
- **Security headers** — `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy: camera=(), microphone=(), geolocation=()`, `X-XSS-Protection`, `Strict-Transport-Security` (HSTS with preload).
- **Rate limiting** — 10 ops per 5 minutes per session to prevent abuse.

---

## Frontend

### Hub ([/pdf/](https://pqcrypta.com/pdf/))
- 28 tool cards with icon, badge, description, and colour coding
- Instant client-side search across tool names, descriptions, and tags
- Global drag-and-drop overlay — drop a PDF anywhere on the hub to auto-route to the correct tool
- Animated floating document background (canvas-based)
- Ink-trail cursor effect

### All Tool Pages
- Upload zone with drag-and-drop and file-picker fallback
- File list with per-file remove controls
- Per-tool options panel
- Real-time progress bar with percentage
- 3D processing overlay (animated bouncing dots) during server calls
- Result panel with download button and output file size
- Mobile-responsive CSS Grid layout (1–4 columns)

### Preview & Interactive Selection
- **Merge** — thumbnail grid of all uploaded files; drag to reorder before merging
- **Split** — page thumbnail strip with clickable scissors (✂) between pages for custom cut-point mode
- **Reorder** — full drag-and-drop thumbnail grid
- **Delete Pages** — click-to-toggle thumbnail grid for page selection
- **Extract Pages** — click-to-toggle thumbnail grid with automatic range compression
- **Rotate** — live canvas preview of first page that rotates as angle is changed
- **Redact** — canvas page preview for region drawing mode

### Edit Tool Canvas
- 13 annotation tools (text, draw, line, arrow, rectangle, ellipse, highlight, whiteout, image, signature, strikethrough, underline, QR code)
- Font family picker (Helvetica / Times / Courier)
- Font size input
- Line width slider with visual preview
- Fill / outline toggle for shapes
- Colour picker
- Zoom selector
- Undo / redo per page
- Signature canvas modal with mouse and touch support
- QR code generation panel with size selector
- Page numbering panel (position, format, start number, font size, colour)
- Header / footer panel (text, alignment, font size, colour)
- Insert blank page before / after current page
- Per-page rotation
- Preferences auto-saved to cookie (tool, colour, font, size, line width, fill, zoom)

### Sign Tool Canvas
- Three-tab interface: Draw / Type / Upload
- Freehand signature canvas with touch event support
- Clear button to reset and redraw
- Expandable cryptographic metadata section (signer name, email, reason, location, cert)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Server | PHP 8.4 |
| HTTP | Apache with HTTP/2 |
| Styling | Vanilla CSS (CSS variables, Grid, custom animations) |
| Scripts | Vanilla ES6 JavaScript modules (no framework) |
| PDF engines | Ghostscript, Poppler, qpdf, LibreOffice, PyMuPDF, ImageMagick |
| PQC crypto | `@noble/post-quantum` |
| Colour theme | Amber `#ff8c00` + gold `#ffd700` on deep navy `#040810` |

---

## File Structure

```
pdf/
├── index.php                  # Hub — tool cards, search, drag-drop
├── api.php                    # Single POST endpoint — all 34 operations
├── css/
│   ├── pdf.css                # Complete UI styles
│   ├── pdf-background.css     # Canvas container
│   └── pdf-cursor.css         # Ink trail cursor
├── js/
│   ├── pdf.js                 # Hub init, search, drag-drop routing
│   ├── pdf-background.js      # Floating docs + particle animation
│   ├── pdf-cursor.js          # Cursor ink trail
│   ├── pdf-processing.js      # Global 3D processing overlay
│   └── tools/
│       ├── upload.js          # PdfUploadUtil — shared XHR upload handler
│       ├── merge.js           # Thumbnail preview + drag reorder
│       ├── split.js           # Cut-point preview + range/interval modes
│       ├── compress.js        # DPI slider, size comparison headers
│       ├── convert.js         # PDF → Word
│       ├── watermark.js       # 8-position, per-page, font style
│       ├── sign.js            # Draw/type/upload tabs, canvas, crypto metadata
│       ├── rotate.js          # Canvas preview, odd/even/range, decimal angles
│       ├── protect.js         # Dual-mode AES + PQC, key management
│       ├── unlock.js
│       ├── to-images.js       # Format, DPI, JPEG quality, page range
│       ├── extract-text.js    # Layout, encoding, page range
│       ├── extract-pages.js   # Thumbnail selection, range compression
│       ├── pdf-info.js        # Full metadata display
│       ├── flatten.js
│       ├── grayscale.js
│       ├── repair.js
│       ├── reorder.js         # Drag-and-drop thumbnail grid
│       ├── delete-pages.js    # Thumbnail click selection
│       ├── pdfa.js            # Level selector (1b/2b/3b)
│       ├── word-to-pdf.js
│       ├── excel-to-pdf.js
│       ├── ppt-to-pdf.js
│       ├── image-to-pdf.js
│       ├── pdf-to-excel.js
│       ├── html-to-pdf.js
│       ├── redact.js          # Text patterns + region drawing mode
│       ├── compare.js         # DPI + sensitivity controls
│       ├── edit.js            # 13-tool canvas editor, page numbering, headers
│       └── workflow.js        # Visual step builder, drag reorder
├── scripts/                   # Python helpers (PyMuPDF operations)
└── tools/                     # PHP tool pages
    ├── _tool_head.php         # Shared header (CSP nonces, nav)
    ├── _tool_foot.php         # Shared footer
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
