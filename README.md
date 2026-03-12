# Secure PDF Tools

**The Private, Server-Side PDF Toolkit — Free, Fast, and Zero-Retention.**
No uploads to third-party clouds. No accounts. No tracking. Ever.

Every operation runs entirely on the server. Files are processed and immediately deleted — nothing is stored, logged, or shared.

---

## Tools (28 Operations)

### Core Manipulation
| Tool | Description |
|---|---|
| **Merge PDFs** | Combine 2–50 PDF files into one document. Drag to reorder before merging. |
| **Split PDF** | Split by individual pages, fixed chunk size, or custom page ranges. |
| **Reorder Pages** | Drag-and-drop page thumbnails to rearrange pages, then export. |
| **Delete Pages** | Remove specific pages or page ranges from a PDF. |
| **Extract Pages** | Pull out a subset of pages as a new PDF. |
| **Rotate Pages** | Rotate all pages or specific pages by 90°, 180°, or 270°. |
| **Compress PDF** | Reduce file size with five quality presets (Screen, eBook, Printer, Prepress, Custom) plus optional metadata stripping, stream recompression, and linearization. |
| **Repair PDF** | Recover and rebuild corrupted or malformed PDF files via Ghostscript reconstruction. |
| **Flatten PDF** | Flatten form fields and annotations into the page content layer. |
| **Grayscale / B&W** | Convert colour PDFs to grayscale or pure black-and-white. |

### Format Conversion
| Tool | Description |
|---|---|
| **PDF → Word** | Export PDF to editable `.docx` via LibreOffice. |
| **PDF → Excel** | Export PDF tables to `.xlsx` via LibreOffice. |
| **PDF → Images** | Render pages to PNG or JPEG at 72–600 DPI; download as ZIP. |
| **PDF/A Archive** | Convert to PDF/A-2b (long-term archival standard) via Ghostscript. |
| **Word → PDF** | Convert `.doc` / `.docx` / `.odt` to PDF via LibreOffice. |
| **Excel → PDF** | Convert `.xls` / `.xlsx` / `.ods` to PDF via LibreOffice. |
| **PowerPoint → PDF** | Convert `.ppt` / `.pptx` / `.odp` to PDF via LibreOffice. |
| **Images → PDF** | Pack JPEG / PNG / WebP / BMP / TIFF images into a single PDF via ImageMagick. |
| **HTML → PDF** | Upload an `.html` / `.htm` file and convert to PDF via LibreOffice. |

### Protection & Security
| Tool | Description |
|---|---|
| **Protect PDF** | Password-protect with AES-256-CBC **or** encrypt the document with any of 31 post-quantum cryptography algorithms (see below). Set open password, owner password, and granular permission flags (print, copy, modify, annotate, form fill, accessibility, assembly). |
| **Unlock PDF** | Remove password protection from a PDF (owner password required). |
| **Redact PDF** | Permanently black-out text strings, regex patterns, or all occurrences of a phrase across every page using PyMuPDF. |

### Content & Annotation
| Tool | Description |
|---|---|
| **Add Watermark** | Stamp a text watermark on every page. Configurable: text, font size (8–200 pt), opacity (0–100%), rotation angle, colour (hex), and position (centre, header, footer, or diagonal). |
| **Sign PDF** | Place a rendered signature on a PDF. Supports typed-name rendering (ImageMagick) and optional cryptographic metadata (signer name, email, reason, location, date stamp). |
| **Edit PDF** | Page-by-page visual editor: add text boxes, draw freehand, insert images, add QR codes (auto-generated from any URL or text), then apply changes. |
| **Compare PDFs** | Side-by-side visual diff of two PDFs; generates a highlighted difference report. |
| **Extract Text** | Pull all text content from a PDF as a plain `.txt` file using Poppler pdftotext. |
| **PDF Info** | Display full document metadata: title, author, subject, keywords, creator, producer, page count, dimensions, PDF version, encryption status, and permission flags. |

### Automation
| Tool | Description |
|---|---|
| **Workflow Builder** | Chain up to N operations in sequence on one or more PDFs. Supported workflow steps: rotate, compress, watermark, protect, unlock, grayscale, flatten, repair, extract-pages, delete-pages, reorder, pdfa. Each step carries its own parameters. |

---

## Post-Quantum Encryption (31 Algorithms)

The **Protect PDF** tool includes a full PQC encryption mode backed by the same engine used across PQ Crypta. All 31 algorithms pass a full key-generation → encryption → decryption roundtrip.

| Category | Algorithms |
|---|---|
| NIST Standards | `ml-kem-1024`, `post-quantum`, `hybrid`, `classical` |
| NIST 2025 Code-Based | `hqc-128`, `hqc-192`, `hqc-256` |
| FN-DSA Stack | `fn-dsa-512-compact`, `fn-dsa-1024-security`, `fn-dsa-dual-signature`, `fn-dsa-fp-hardened`, `fn-dsa-transition-stack`, `fn-dsa-zk-stack` |
| Max-Secure Variants | `max-secure-crypto-agile`, `max-secure-hybrid-transition`, `max-secure-lightweight`, `max-secure-pqc-zk`, `max-secure-pure-pq`, `max-secure-stateless` |
| Multi-Algorithm | `multi-algorithm`, `multi-kem`, `multi-kem-triple`, `lattice-code-hybrid` |
| Advanced | `ai-synthesized-crypto-agile`, `entropy-orchestrated`, `quantum-lattice-fusion`, `quantum-resistant-consensus`, `pq3-stack`, `quad-layer`, `post-zk-homomorphic` |

---

## API

All operations are handled by a single PHP endpoint:

```
POST /pdf/api.php
Content-Type: multipart/form-data
```

### Common Parameters

| Parameter | Type | Description |
|---|---|---|
| `operation` | string | One of the 34 operation names listed below |
| `file` | file | Primary PDF input (most operations) |
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

### Operation-Specific Parameters

**compress**
- `quality` — `screen` (72 DPI) | `ebook` (150 DPI) | `printer` (300 DPI) | `prepress` (300 DPI + colour) | `custom`
- `custom_dpi` — integer 50–600 (when `quality=custom`)
- `strip_metadata` — `1` to remove document metadata
- `linearize` — `1` to enable fast web view (qpdf)
- `recompress_streams` — `1` to recompress content streams (qpdf)

**split**
- `split_mode` — `all` (one file per page) | `range` (custom ranges) | `chunk` (every N pages)
- `page_ranges` — comma-separated ranges e.g. `1-3,5,7-9` (when `split_mode=range`)
- `chunk_size` — integer pages per chunk (when `split_mode=chunk`)

**rotate**
- `angle` — `90` | `180` | `270`
- `pages` — `all` or comma-separated page numbers

**watermark**
- `text` — watermark string
- `font_size` — integer 8–200
- `opacity` — float 0.0–1.0
- `angle` — rotation in degrees
- `color` — hex colour e.g. `#ff8c00`
- `position` — `center` | `header` | `footer` | `diagonal`

**protect**
- `user_password` — open password
- `owner_password` — owner/permissions password
- `allow_print` / `allow_copy` / `allow_modify` / `allow_annotate` / `allow_forms` / `allow_accessibility` / `allow_assembly` — `1` to allow each permission
- `pqc_algorithm` — any of the 31 PQC algorithm IDs for post-quantum mode

**sign**
- `sign_crypto` — `1` to add cryptographic metadata
- `signer_name` — name string (max 100 chars)
- `signer_email` — email address
- `signer_reason` — reason string (max 200 chars)
- `signer_location` — location string

**to-images**
- `format` — `png` | `jpeg`
- `dpi` — integer 72–600

**grayscale**
- `mode` — `grayscale` | `bw`

**redact**
- `pattern` — text string or regex to redact
- `redact_all` — `1` to redact every occurrence

**reorder**
- `page_order` — comma-separated new page order e.g. `3,1,2,4`

**delete-pages** / **extract-pages**
- `pages` — comma-separated page numbers or ranges

**workflow**
- `steps` — JSON array of step objects `[{"op":"compress","quality":"ebook"},{"op":"watermark","text":"DRAFT"}]`

### Response Format

Success returns `application/pdf` (or `application/zip` for multi-file output) as a direct file download.

Error returns JSON:
```json
{ "error": "Description of what went wrong" }
```

Rate-limit response uses HTTP 429.

---

## Limits & Constraints

| Constraint | Value |
|---|---|
| Max file size | 50 MB |
| Max total upload | 200 MB |
| Merge: max files | 50 |
| Rate limit | 10 operations per 5 minutes (session-based) |
| Operation timeout | 120 seconds (8 seconds for lightweight edit operations) |
| Allowed office formats | `.docx`, `.doc`, `.ppt`, `.pptx`, `.xls`, `.xlsx`, `.odt`, `.odp`, `.ods` |
| Allowed image formats | JPEG, PNG, WebP, BMP, TIFF, GIF |

---

## Backend Engines

| Engine | Used For |
|---|---|
| **Ghostscript** | compress, protect (AES-256), rotate, flatten, grayscale (+ B&W), repair, pdfa |
| **Poppler** (`pdfunite`, `pdftoppm`, `pdftotext`, `pdfinfo`) | merge, split, to-images, extract-text, get-info |
| **qpdf** | stream recompression, linearization, unlock |
| **LibreOffice** | pdf→word, pdf→excel, word→pdf, excel→pdf, ppt→pdf, html→pdf |
| **PyMuPDF (fitz)** | watermark, redact, sign, edit, compare |
| **ImageMagick** | signature rendering, image→pdf |
| **Python** | orchestration layer for PyMuPDF operations, QR code generation |

---

## Security

- **Zero retention** — every uploaded and output file is deleted immediately after the response is sent. Temp files are cleaned up in all error paths.
- **No accounts** — no login, no registration, no session data beyond rate limiting.
- **No tracking** — no analytics, no third-party scripts, no cookies beyond session rate-limit state.
- **AES-256-CBC** for classical PDF password protection.
- **31 post-quantum algorithms** for PQC protection mode.
- **Magic byte validation** — uploaded files are checked against their actual binary signatures, not just extension or MIME type.
- **CSP enforcement** — strict Content Security Policy with per-request nonces. No inline styles, no inline scripts, no inline event handlers.
- **Input sanitisation** — all user-supplied strings are sanitised and length-capped before reaching CLI commands.
- **No shell injection** — all CLI arguments are passed through `escapeshellarg()` / `escapeshellcmd()`.
- **Security headers** — `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy: strict-origin-when-cross-origin`, `Permissions-Policy`, `X-XSS-Protection`, `Strict-Transport-Security` (HSTS with preload).
- **Rate limiting** — 10 ops per 5 minutes per session to prevent abuse.

---

## Frontend

### Hub (`/pdf/`)
- 28 tool cards with icon, badge, and description
- Instant client-side search / filter across tool names, descriptions, and tags
- Global drag-and-drop overlay — drop a PDF anywhere on the hub to auto-route to the correct tool
- Animated floating document background
- Ink-trail cursor effect

### Tool Pages (`/pdf/tools/*`)
- Upload zone with drag-and-drop and file-picker fallback
- File list with individual remove controls
- Per-tool options panel (quality sliders, text inputs, toggles)
- Real-time progress bar with percentage
- 3D processing overlay (bouncing dots animation) during server calls
- Result panel with download button and file size display
- Mobile-responsive layout (1–4 column CSS Grid)

### Edit Tool
- Page-by-page canvas viewer
- Text box tool — click to place, type, adjust font size and colour
- Freehand draw tool — pen with configurable stroke width and colour
- Image insert tool — upload and position an image on a page
- QR code generator — generate a QR code from any URL or text string and insert it
- Undo/redo stack per page
- Apply all edits and download the modified PDF

### Compare Tool
- Upload two PDFs
- Side-by-side page diff rendered as images
- Highlighted change overlay showing added, removed, and modified regions

### Workflow Builder
- Visual step builder — add steps from a dropdown, configure each step's parameters
- Drag to reorder steps
- Live step summary list
- Supports 12 chainable operations
- Single file input, all steps applied in sequence, one output file

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
├── index.php              # Hub page — tool cards, search, drag-drop
├── api.php                # Single POST endpoint — all 34 operations
├── css/
│   ├── pdf.css            # Complete UI styles (~4400 lines)
│   ├── pdf-background.css # Floating document canvas container
│   └── pdf-cursor.css     # Ink trail cursor
├── js/
│   ├── pdf.js             # Hub initialisation, search, drag-drop
│   ├── pdf-background.js  # Floating docs + particle animation
│   ├── pdf-cursor.js      # Cursor ink trail
│   ├── pdf-processing.js  # Global 3D processing overlay
│   └── tools/
│       ├── upload.js      # Shared upload utility (PdfUploadUtil)
│       ├── merge.js
│       ├── split.js
│       ├── compress.js
│       ├── convert.js     # PDF → Word
│       ├── watermark.js
│       ├── sign.js
│       ├── rotate.js
│       ├── protect.js
│       ├── unlock.js
│       ├── to-images.js
│       ├── extract-text.js
│       ├── extract-pages.js
│       ├── pdf-info.js
│       ├── flatten.js
│       ├── grayscale.js
│       ├── repair.js
│       ├── reorder.js
│       ├── delete-pages.js
│       ├── pdfa.js
│       ├── word-to-pdf.js
│       ├── excel-to-pdf.js
│       ├── ppt-to-pdf.js
│       ├── image-to-pdf.js
│       ├── pdf-to-excel.js
│       ├── html-to-pdf.js
│       ├── redact.js
│       ├── compare.js
│       ├── edit.js
│       └── workflow.js
├── scripts/               # Server-side Python helpers (PyMuPDF ops)
└── tools/                 # PHP tool pages
    ├── _tool_head.php     # Shared page header (CSP, nonces, nav)
    ├── _tool_foot.php     # Shared page footer
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
