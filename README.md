# Secure PDF Tools

**The Private, Server-Side PDF Toolkit — Free, Fast, and Zero-Retention.**
No uploads to third-party clouds. No accounts. No tracking. Ever.

Every operation runs entirely on the server. Files are processed and immediately deleted — nothing is stored, logged, or shared.

**Live:** [pqpdf.com](https://pqpdf.com)

---

## Why This Exists

Most online PDF tools share a structural problem: they are built around cloud storage. A file uploaded to process a watermark travels to a third-party processing service, sits in object storage for minutes to hours, passes through analytics pipelines, and is subject to retention policies that are either vague or opaque. Free tiers are underwritten by data — user files, metadata, behavioural signals — sold or used to train models.

Four specific gaps drove this project:

**1. No zero-retention guarantee anywhere in the stack**
Existing tools retain files for cleanup windows (1 hour, 2 hours, "as soon as possible"). The server-side `send_file()` function in this project calls `cleanup()` immediately after `readfile()` — the temp directory is deleted while the download is still in flight. There is no retention window because there is no buffer.

**2. No multi-engine threat analysis for PDFs**
PDF is the most exploited document format. No free tool runs more than a signature check against uploaded files. This project runs 44 independent analysis engines — 15 static heuristic engines, a dynamic behavioral sandbox that actually executes the PDF in an isolated Linux namespace with full syscall tracing, an ML Intelligence Engine with SHAP explainability (IsolationForest + RandomForest + LightGBM on a 38-feature vector), a differential parsing engine comparing six independent parsers across 8 structural dimensions, a polyglot binary detector, a JavaScript AST deobfuscator, fully offline local threat intelligence (URLhaus · MalwareBazaar · ThreatFox — 6.4M+ indicators, zero external API calls), PDF signature forensics (ByteRange shadow-document detection), phishing detection (credential harvesting, brand impersonation, QR codes), embedded file analysis (PE/ELF/OLE/VBA), and TLSH fuzzy-hash campaign attribution. Every indicator is mapped to a MITRE ATT&CK technique ID.

**3. No post-quantum cryptography in document workflows**
All existing tools use AES-256 at best. This project integrates 31 post-quantum algorithms (NIST-standardised ML-KEM-1024, HQC-128/192/256, FN-DSA variants, and hybrid modes) for PDF encryption, running client-side in the browser before any data is transmitted.

**4. No transparency about what engines are actually running**
Proprietary SaaS tools describe their operations in marketing language. This project runs on openly documented, auditable open-source engines: Ghostscript, Poppler, LibreOffice, Tesseract 5, PyMuPDF, ImageMagick, qpdf, ExifTool, YARA, ClamAV, PeePDF, pikepdf, Acorn, scikit-learn, LightGBM, and imagehash. Every operation in every tool is described in the exact pipeline terms of the actual code.

---

## Tools (45)

### Core Manipulation

| Tool | Link | Description |
|---|---|---|
| **Merge PDFs** | [/tools/merge.php](https://pqpdf.com/tools/merge.php) | Combine up to 20 PDFs into one (200 MB total). Drag thumbnails to reorder pages before merging. Upload progress shows real-time percentage; once upload completes, server phase cycles through descriptive status messages. |
| **Split PDF** | [/tools/split.php](https://pqpdf.com/tools/split.php) | Split by every page, fixed interval, custom page ranges, or interactive cut-point selection. |
| **Reorder Pages** | [/tools/reorder.php](https://pqpdf.com/tools/reorder.php) | Drag-and-drop page thumbnails to rearrange, then export. |
| **Delete Pages** | [/tools/delete-pages.php](https://pqpdf.com/tools/delete-pages.php) | Click thumbnail grid to select pages for removal. |
| **Extract Pages** | [/tools/extract-pages.php](https://pqpdf.com/tools/extract-pages.php) | Click thumbnail grid to select pages to keep; auto-compresses ranges (e.g. 1-3,5,7-9). |
| **Rotate Pages** | [/tools/rotate.php](https://pqpdf.com/tools/rotate.php) | Rotate all, odd, even, or a custom range of pages. Supports 90°/180°/270° and arbitrary decimal angles. Live canvas preview of the first page. |
| **Compress PDF** | [/tools/compress.php](https://pqpdf.com/tools/compress.php) | Five quality presets plus custom DPI slider (50–600). Optional metadata stripping, linearization, and stream recompression. Live before/after split-canvas preview rendered from page 1 when a file is selected — original on the left, simulated compressed on the right. Shows original vs. compressed size after download. |
| **Repair PDF** | [/tools/repair.php](https://pqpdf.com/tools/repair.php) | Reconstruct corrupted or malformed PDFs via Ghostscript. On upload, PDF.js diagnoses the file client-side: checks the PDF header, attempts to parse the cross-reference table and content streams, and renders pages 1–3. A scan card reports detected issues (xref errors, stream corruption, truncation, encryption) as red badges — or confirms the file is readable with a green badge and page count. |
| **Flatten PDF** | [/tools/flatten.php](https://pqpdf.com/tools/flatten.php) | Flatten form fields, annotations, and layers into the page content layer. On upload, PDF.js scans the document client-side and shows a summary card — e.g. "3 form fields · 2 annotations · 1 layer — all will be made permanent" — with amber badges per element type. Shows a green "Already flat" badge if no interactive elements are found. |
| **Grayscale / B&W** | [/tools/grayscale.php](https://pqpdf.com/tools/grayscale.php) | Convert to grayscale or pure black-and-white. Live before/after split-canvas preview rendered from page 1 — color on the left, grayscale simulation on the right. |
| **N-up / Imposition** | [/tools/nup.php](https://pqpdf.com/tools/nup.php) | Arrange multiple PDF pages on each output sheet: 2-up (2×1), 4-up (2×2), 6-up (2×3), 8-up (2×4), 9-up (3×3), or booklet imposition (pages re-ordered and paired for saddle-stitch binding). Uses PyMuPDF `show_pdf_page()` — vector output, no rasterisation. Page size and orientation selectable. |
| **Auto-crop / Deskew** | [/tools/deskew.php](https://pqpdf.com/tools/deskew.php) | Remove excess white margins and correct page rotation. Three modes: auto-crop only, fix rotation only, or both. Auto-crop detects the tight content bounding box across text blocks, vector drawings, and raster images (via PyMuPDF) with a 20pt safety margin. Fix rotation bakes the page's `/Rotate` flag into the content stream so output pages have `rotation=0` in all viewers — aspect ratio and coordinate mapping are preserved even for 90°/180°/270° rotated pages via an offset target-rect approach. **Per-page interactive crop editor**: after upload, each page is rendered with a live draggable crop box — 8 handles (4 corners + 4 edge midpoints) let you resize or move the keep area; drag inside the box to pan it. Auto-detection runs per-page using PDF.js text extraction and updates the handles immediately. Manual adjustments are stored per-page. "Reset page" re-runs auto-detection. "Apply to all pages" normalises the current crop as fractions and applies it proportionally to every page. Page navigator browses the full document before committing. Per-page overrides are sent to the server as a JSON array of `{page, x0, y0, x1, y1}` in PDF display-space points and applied in the Python pipeline. Pages without manual overrides continue to use auto-detection. |

### Format Conversion

| Tool | Link | Description |
|---|---|---|
| **PDF → Word** | [/tools/convert.php](https://pqpdf.com/tools/convert.php) | Export to editable `.docx`, `.odt`, `.rtf`, or `.txt` via LibreOffice. Format fidelity indicator shows star ratings (out of 4) for all four formats, highlighting the active selection with a description of expected quality. |
| **PDF → Excel** | [/tools/pdf-to-excel.php](https://pqpdf.com/tools/pdf-to-excel.php) | Export tables to `.xlsx` via LibreOffice. |
| **PDF → Images** | [/tools/to-images.php](https://pqpdf.com/tools/to-images.php) | Render pages to PNG or JPEG at 72–600 DPI. Select all pages or a custom range. JPEG quality slider when JPEG format is chosen. Live DPI quality preview: page 1 is rendered in a canvas at the selected DPI immediately after upload; re-renders as you change the DPI setting, showing actual output pixel dimensions (e.g. "1240 × 1754 px at 150 DPI") before any server processing. Download as ZIP. |
| **PDF/A Archive** | [/tools/pdfa.php](https://pqpdf.com/tools/pdfa.php) | Convert to PDF/A-1b, PDF/A-2b, or PDF/A-3b for long-term archival. |
| **Word → PDF** | [/tools/word-to-pdf.php](https://pqpdf.com/tools/word-to-pdf.php) | Convert `.doc` / `.docx` / `.odt` via LibreOffice. Format fidelity indicator shows expected output quality for the uploaded file type. |
| **Excel → PDF** | [/tools/excel-to-pdf.php](https://pqpdf.com/tools/excel-to-pdf.php) | Convert `.xls` / `.xlsx` / `.ods` via LibreOffice. Sheet selector — fetches sheet names from the uploaded file and lets you pick which sheet(s) to convert. |
| **PowerPoint → PDF** | [/tools/ppt-to-pdf.php](https://pqpdf.com/tools/ppt-to-pdf.php) | Convert `.ppt` / `.pptx` / `.odp` via LibreOffice. Slide selector — fetches slide titles from the uploaded file and lets you choose which slides to include. |
| **Images → PDF** | [/tools/image-to-pdf.php](https://pqpdf.com/tools/image-to-pdf.php) | Pack JPEG / PNG / WebP / BMP / TIFF images into a single PDF via ImageMagick. |
| **HTML → PDF** | [/tools/html-to-pdf.php](https://pqpdf.com/tools/html-to-pdf.php) | Convert `.html` / `.htm` files or any public URL to PDF via **Playwright/Chromium**. Full Chromium rendering engine captures modern CSS, web fonts, lazy-loaded images, and JavaScript-rendered content. URL mode uses `waitUntil:'load'` + an 8 s networkidle cap (never stalls on analytics/polling), auto-scrolls to trigger IntersectionObserver lazy loading, waits for `document.fonts.ready`, flushes two `requestAnimationFrame` cycles, then emulates `@media print` before generating the PDF. Page size (A4/Letter/Legal/auto-width), orientation, and margins are configurable. |
| **PDF → PowerPoint** | [/tools/pdf-to-ppt.php](https://pqpdf.com/tools/pdf-to-ppt.php) | Convert PDF pages to a PPTX presentation. Each page is rendered at 150 DPI via PyMuPDF and placed as a full-bleed image on its own slide using python-pptx. Slide dimensions match the original page aspect ratio. |
| **PDF → HTML** | [/tools/pdf-to-html.php](https://pqpdf.com/tools/pdf-to-html.php) | Convert PDF pages to a styled HTML document using PyMuPDF `page.get_text("html")`, which preserves font, size, and positioned text spans. Pages are separated by CSS `page-break-after` divs. Produces a single self-contained `.html` file with print-friendly styling. |
| **PDF → Markdown** | [/tools/pdf-to-md.php](https://pqpdf.com/tools/pdf-to-md.php) | Convert PDF to GitHub-flavoured Markdown using **pymupdf4llm** — the latest AI/LLM-optimised layout analysis engine built on PyMuPDF 1.27 + ONNX. Detects headings, paragraphs, tables, code blocks, and list structures. Produces clean `.md` ideal for RAG pipelines, LLM ingestion, and documentation workflows. |
| **PDF/X Output** | [/tools/pdfx.php](https://pqpdf.com/tools/pdfx.php) | Convert a PDF to print-industry PDF/X format (PDF/X-1a, PDF/X-3, or PDF/X-4) via Ghostscript with CMYK colour conversion, `/prepress` quality settings, and configurable render intent. Ensures all fonts are embedded and colour data is print-shop compliant. |

### Security & Privacy

| Tool | Link | Description |
|---|---|---|
| **PDF Forensics Scanner** | [/tools/scan.php](https://pqpdf.com/tools/scan.php) | Forensic analysis across **44 independent engines** — most comprehensive free PDF forensics tool available. **New engines (31–43):** PDF Token Obfuscation detector (hex-escaped name token decoding — /J#61vaScript → /JavaScript — whitespace-split keyword injection, formfeed byte evasion), XFA FormCalc parser (exec/openURL/submit/initialize auto-execute), PDF action dependency graph (cycles · deep chains · fan-in · sleeper nodes), OCG layer cloaking (never-visible · screen/print divergence · hidden clickable links), Unicode & invisible text (RLO U+202E · rendering mode 3/7 · homograph domains), trailer chain forensics (raw /Prev walk · ID mutation · /Root swap Shadow Attack), codec exploit parameter validation (CCITTFax OOB · JBIG2Globals CVE-2009-0658 · DCT mismatch · multi-filter chains), physical entropy topology (PDF-structure-aware sliding window · post-EOF · entropy cliffs), image steganography (LSB chi-square · tracking beacons · JPEG EXIF anomalies), PDF/A compliance fraud (DLP bypass detection), JavaScript behavioral emulation (Node.js vm + Acrobat API stub · LAUNCH_URL · SUBMIT_FORM · MAIL), font CharString emulator (Type 1 decrypt + stack machine · seac OOB · depth overflow), XRef integrity graph (phantom objects · orphan sleepers · free-entry bugs · length fraud). **All existing engines:** structural integrity · 45+ byte signatures · stream entropy · metadata forensics (ExifTool) · font analysis · CVE patterns · qpdf · YARA · PeePDF · dynamic sandbox (strace + Linux namespaces) · ClamAV · ML+SHAP (IsolationForest + RandomForest + LightGBM) · six-parser differential · polyglot detection · JS AST deobfuscation · threat intelligence (6.4M+ local indicators) · signature forensics · phishing · embedded file analysis · campaign attribution · weighted correlation engine (60+ compound patterns + MITRE ATT&CK). Results across **23 analysis tabs**: Summary · Threats · Score · Engines · URLs · Streams · ML · Sandbox · TI · MITRE · Differential · Polyglot · Phishing · Embedded · Signature · History · Annotations · Metadata · XFA · Action Graph · Deep Forensics · Raw JSON · Raw Forensics. **9-mode sanitize.** |
| **Protect PDF** | [/tools/protect.php](https://pqpdf.com/tools/protect.php) | Dual-mode protection: **Standard** (AES-256-CBC server-side) or **PQC** (client-side quantum-safe encryption). See details below. |
| **Unlock PDF** | [/tools/unlock.php](https://pqpdf.com/tools/unlock.php) | Remove password protection (owner password required). Detects the encryption type client-side by reading the PDF header before upload — shows a `🔒 AES-256 encrypted` badge for password-protected files or a `✅ No password protection detected` badge if the file is already unlocked. PQC bundles (`.pqcpdf`) are auto-detected by extension and routed to the quantum-safe decryption panel. |
| **Redact PDF** | [/tools/redact.php](https://pqpdf.com/tools/redact.php) | Two modes: text-pattern redaction (with multi-pattern list, case sensitivity, whole-word matching) or mouse-drawn region redaction on a canvas preview. Custom fill colour. |
| **Add Watermark** | [/tools/watermark.php](https://pqpdf.com/tools/watermark.php) | Stamp text watermarks. 8-position placement, opacity, rotation angle, font size, font style, hex colour. Apply to all, odd, even, or custom page ranges. Live canvas preview — page 1 is rendered and the watermark text is drawn over it in real time as you adjust text, opacity, size, colour, and position. |
| **Sign PDF & PAdES** | [/tools/sign.php](https://pqpdf.com/tools/sign.php) | Four signature modes in one tool. **Draw** — freehand on a canvas with full touch support. **Type** — name rendered as a signature image via ImageMagick (DejaVu-Sans-Oblique). **Upload** — any PNG/JPEG used as the signature image. **PAdES / Crypto Only** — invisible cryptographic signature with no image drawn on the page, verifiable in Adobe Reader's Signatures panel. Visual modes support placement controls: first/last/all/custom page, left/center/right × top/middle/bottom position grid, and a size slider (40–300 pt). Live placement preview composites the signature onto a rendered page 1 canvas in real time as position and size are adjusted. All modes optionally embed a cryptographic digital signature: signer name (required), email, reason, and location metadata. Certificate source is either auto-generated ephemeral RSA-2048 self-signed or user-supplied `.p12`/`.pfx`. PAdES mode uses **pyhanko 0.34** for an incremental CMS/PKCS#7 signature written as an incremental update (original content never modified) — compliant with PAdES-B (ETSI EN 319 102-1). Visual modes use **endesive** for the embedded crypto layer. `/tools/pades.php` 301-redirects to `/tools/sign.php?tab=pades`, preserving existing links and SEO. |
| **Send for E-Signature** | [/tools/esign.php](https://pqpdf.com/tools/esign.php) | Multi-party electronic signature workflow. Upload a PDF, add up to 10 signers (name + optional email), choose sequential (chain) or parallel (all-at-once) signing order. Each signer receives a unique 256-bit secure link — no account required on either side. The initiator sees live signing status polled every 5 seconds and downloads the final PDF once all signers have signed. Individual signing page (`/tools/sign-request.php?t=…&s=…`) shows a page-1 thumbnail (PyMuPDF JPEG render), three signature modes (draw canvas, typed name, uploaded image), placement controls (page/position), and an optional PAdES-B cryptographic layer (auto self-signed RSA-2048 or own `.p12`). Workflow state is stored in an ephemeral temp directory (`/tmp/esign_{32hex}/`) with a 24-hour TTL; the directory is purged at create-time cleanup and on expiry. Sequential signing chains each step's output into the next signer's input PDF. Zero retention — the final PDF is never stored beyond the 24-hour window. |

### Annotate & Inspect

| Tool | Link | Description |
|---|---|---|
| **Edit PDF** | [/tools/edit.php](https://pqpdf.com/tools/edit.php) | Full page-by-page visual editor with 16 annotation tools, an interactive form builder, and a bookmark editor. Tools: text, freehand draw, eraser, line, arrow, rectangle and ellipse (both with independent fill colour), highlight, whiteout, strikethrough, underline, image insert, signature, QR code, stamps, and sticky notes. Bookmark panel writes a native PDF table of contents via `set_toc()`. Draws AcroForm widgets (Text, CheckBox, RadioButton, ListBox, ComboBox, Signature, PushButton) directly onto the PDF canvas, then commits everything server-side via PyMuPDF. See full details below. |
| **Fill PDF Form** | [/tools/fill.php](https://pqpdf.com/tools/fill.php) | Detect and fill all interactive AcroForm fields in any PDF — text inputs, checkboxes, radio buttons, drop-down menus, and list boxes. Values are written server-side via PyMuPDF. Optional flatten-after-filling bakes field values into static page content for archiving or sharing. |
| **Compare PDFs** | [/tools/compare.php](https://pqpdf.com/tools/compare.php) | Visual diff of two PDFs. Configure DPI (72/96/150/300) and sensitivity. Side-by-side page 1 canvas previews render immediately when each file is selected. Outputs a highlighted diff PDF with change regions marked. |
| **Extract Text** | [/tools/extract-text.php](https://pqpdf.com/tools/extract-text.php) | Export all text to `.txt`. Options: layout preservation, text encoding, custom page range. |
| **PDF Info** | [/tools/pdf-info.php](https://pqpdf.com/tools/pdf-info.php) | Display full metadata: title, author, subject, keywords, creator, producer, page count, dimensions, PDF version, encryption status, form type, tagged flag, page rotation, fast web view optimisation, creation and modification dates, permission flags. Shows a quick canvas preview of page 1 alongside the metadata. |
| **OCR PDF** | [/tools/ocr.php](https://pqpdf.com/tools/ocr.php) | Optical Character Recognition for scanned and image-based PDFs. Powered by Tesseract 5 LSTM neural network. Three output formats: plain text (.txt), searchable PDF (original images + invisible text layer so the document becomes copyable and searchable), or a ZIP containing both. DPI control (150/200/300), four page segmentation modes (auto, single column, single block, sparse text), custom page ranges, up to 100 pages per job. Returns OCR confidence score (per-word Tesseract TSV confidence averaged across all pages), word count, and character count. Live text preview tab in the browser — preview extracted text without downloading. |
| **Bookmarks / Outline Editor** | [/tools/outline-editor.php](https://pqpdf.com/tools/outline-editor.php) | Standalone bookmark and outline editor. Upload a PDF to load its existing table of contents. Add, rename, reorder, delete, and adjust the level (1–4) of each entry; each bookmark row has a page-number input validated against the document's actual page count. Uses PyMuPDF `doc.get_toc()` / `doc.set_toc()` to read and write the native PDF outline structure. |
| **Accessibility Checker** | [/tools/a11y.php](https://pqpdf.com/tools/a11y.php) | WCAG 2.1 / PDF/UA compliance audit. Runs 8 checks via PyMuPDF: document title (WCAG 2.4.2), language metadata (WCAG 3.1.1), tagged PDF structure (PDF/UA-1 §7.1), image alt-text presence (WCAG 1.1.1), reading order consistency (WCAG 1.3.2), font embedding (PDF/UA-1 §7.21), bookmark navigation for multi-page documents (WCAG 2.4.5), and page-size consistency. Returns a pass/fail report with WCAG criterion references, impact levels, and an overall grade (A–F). |
| **Font Inspector** | [/tools/font-inspector.php](https://pqpdf.com/tools/font-inspector.php) | Enumerate all fonts used across every page of a PDF. For each font: name, type (Type1, TrueType, CIDFont, etc.), encoding, embedded status, subset status (presence of `+` prefix in the BaseFont name), and the list of pages it appears on. Flags non-embedded fonts in red — critical for print submission and PDF/UA compliance. |
| **Color Profile / CMYK Inspector** | [/tools/color-inspect.php](https://pqpdf.com/tools/color-inspect.php) | Comprehensive colour space audit across all PDF content. Five detection layers: (1) raster images via PyMuPDF `extract_image()` colorspace component count; (2) vector drawings via `get_drawings()` — PyMuPDF preserves colour space tuple length (1=Gray, 3=RGB, 4=CMYK); (3) PDF content-stream operator tokenisation — detects `rg/RG` (RGB), `k/K` (CMYK), `g/G` (Gray), `cs/CS` (named) operators, catching text colours and inline images; (4) resource-dict traversal for Separation (spot), DeviceN, ICCBased, Lab, CalRGB, CalGray; (5) ExtGState inspection for overprint (`/OP true`) and transparency (`/ca`, `/CA`, `/BM`) flags. Ghostscript `inkcov` provides structured per-page CMYK percentages with Total Ink Coverage (TIC) — pages over 300% are flagged as press risk. Reports print-readiness verdict. |
| **Table → JSON** | [/tools/table-json.php](https://pqpdf.com/tools/table-json.php) | Extract structured tables from PDF and export as JSON. Uses **pdfplumber** with `lines_strict` strategy (explicit table borders detected from PDF path operators) and falls back to text-position heuristics. First row is treated as column headers; remaining rows become an array of objects keyed by header. Output is a `.json` file containing `{table_count, page_count, tables:[{id, page, rows, cols, headers, data:[{col:val}]}]}`. |
| **PDF Scanner** | [/tools/camera-scan.php](https://pqpdf.com/tools/camera-scan.php) | Scan physical documents to searchable PDF using a device camera or uploaded photos — no app install required. Live viewfinder renders the camera stream to a `<canvas>` with a real-time Sobel edge-detection overlay that highlights the document boundary as the user frames the shot. Capture freezes the frame and shows four draggable corner handles for precise perspective adjustment. Server-side processing (OpenCV `getPerspectiveTransform` + `warpPerspective`) flattens tilted captures. Four enhancement modes: **Auto** (CLAHE adaptive contrast on LAB L-channel + unsharp mask), **Color** (no processing), **B&W Document** (Gaussian pre-blur + `adaptiveThreshold` — best for printed text), **Grayscale**. Multi-page: captured pages accumulate in a thumbnail gallery with per-page delete; all pages are submitted in one request. OCR mode runs Tesseract 5 per-page to produce a searchable PDF (image + hidden text layer). Image-only mode skips OCR for faster output. All images are deleted immediately after the PDF is streamed. |

### Automation

| Tool | Link | Description |
|---|---|---|
| **Workflow Builder** | [/tools/workflow.php](https://pqpdf.com/tools/workflow.php) | Chain operations visually: add steps from the picker, configure per-step parameters, drag to reorder. Supported steps: rotate, compress, watermark, protect, unlock, grayscale, flatten, repair, extract pages, delete pages, reorder pages, convert to PDF/A, **sign** (typed visual / typed visual + digital / digital-only with auto self-signed cert), **redact** (permanent text-pattern removal, case-sensitive option, black or white fill), and **split every N pages** (terminal step — outputs a ZIP of equal-sized PDF chunks, useful for batch-scanned documents). Save named workflows to localStorage; **Load** replaces current steps, **+ Append** joins a saved workflow onto the end of the current one — enabling complex pipelines composed from saved building blocks. Export / import workflows as JSON. Runs the full sequence on one or more uploaded PDFs. |

---

## Architecture

```
Browser (HTTPS / HTTP3 / QUIC)
        │
        ▼
┌──────────────────────────────────────────────────────────┐
│  pqcrypta-proxy  (Rust, HTTP/3+QUIC, PQC TLS)            │
│                                                          │
│  · TLS termination (X25519MLKEM768 hybrid KEM)           │
│  · Load balancing — least_connections across backends    │
│  · Circuit breaker — opens after 5 failures              │
│  · Health polling — GET /health every 10–30 s per pool   │
│  · Rate limiting — per-IP + JA3/JA4 fingerprint          │
│  · Security headers injected on all responses            │
└──────────────────────────┬───────────────────────────────┘
                           │  HTTP/1.1 (plain, port 8080)
                           ▼
┌──────────────────────────────────────────────────────────┐
│  Apache HTTP/2  ·  PHP 8.4 FPM                           │
│                                                          │
│  ┌─────────────┐     ┌──────────────────────────────┐   │
│  │  Tool page  │────▶│  api.php  (single endpoint)  │   │
│  │  (PHP+JS)   │◀────│  POST multipart/form-data    │   │
│  └─────────────┘     └──────────┬───────────────────┘   │
│                                 │                        │
│                    create_temp_dir()                      │
│                    pdftool_{24-hex-chars}  (mode 0700)   │
│                                 │                        │
│                    ┌────────────▼────────────────────┐   │
│                    │  Processing engines (per op)    │   │
│                    │                                 │   │
│                    │  Ghostscript  — compress, water-│   │
│                    │    mark, rotate, protect, unlock│   │
│                    │    flatten, grayscale, repair   │   │
│                    │  Poppler (pdfunite/pdftoppm/    │   │
│                    │    pdftotext/pdfinfo) — merge,  │   │
│                    │    split, extract-text,         │   │
│                    │    to-images, get-info          │   │
│                    │  qpdf         — protect/unlock  │   │
│                    │  LibreOffice  — Office↔PDF      │   │
│                    │  ImageMagick  — image→PDF       │   │
│                    │  Tesseract 5  — OCR (LSTM)      │   │
│                    │  PyMuPDF 1.27 — edit, nup,      │   │
│                    │    deskew, outline, a11y,       │   │
│                    │    font-inspect, color-inspect  │   │
│                    │  Playwright/Chromium — html→pdf │   │
│                    │  pymupdf4llm — PDF → Markdown   │   │
│                    │  python-pptx — PDF → PPTX       │   │
│                    │  pdfplumber — table → JSON      │   │
│                    │  pyhanko 0.34 — PAdES LTV sign  │   │
│                    │  ExifTool, YARA, ClamAV         │   │
│                    │  PeePDF, strace/unshare, acorn  │   │
│                    │  scikit-learn (IsolationForest  │   │
│                    │    + RandomForest) — ML (⑰)     │   │
│                    │  shap — SHAP TreeExplainer (⑰)  │   │
│                    │  python3-tlsh — TLSH fuzzy hash  │   │
│                    │  zbarimg — QR decode (㉓)        │   │
│                    │  URLhaus/Bazaar/ThreatFox — TI (㉑)│   │
│                    │  pyhanko — sig forensics (㉒)    │   │
│                    └────────────┬────────────────────┘   │
│                                 │                        │
│                    send_file()  ─  readfile() then       │
│                    cleanup()   ─  rm -rf temp dir        │
│                                 │                        │
└─────────────────────────────────┼────────────────────────┘
                                  ▼
                        Download stream → Browser
                        (temp dir already deleted)
```

Every operation creates one isolated temp directory, runs entirely inside it, streams the result to the browser, and deletes the directory. No file ever touches persistent storage.

---

## Security Model

All facts in this section are derived from `api.php`, `_tool_head.php`, and per-tool PHP pages.

### File Validation
- **Two-stage PDF validation**: magic-byte check (`fread($fh, 4) === '%PDF'`) followed by a structural `pdfinfo` parse — a polyglot file that starts with `%PDF` but has no parseable cross-reference table fails the second stage.
- File size limits enforced at upload: **50 MB per file** (`MAX_FILE_SIZE = 52_428_800`), **200 MB total** across all files in a single request (`MAX_TOTAL_SIZE = 209_715_200`).
- MIME type checked against `['application/pdf', 'application/x-pdf']` for PDF operations.
- Page range inputs are validated against `/^\d+$/` before casting — malformed ranges like `1-2-3` or `abc` are rejected before any integer conversion.

### Temporary Workspace Isolation
- Each request creates `sys_get_temp_dir() . '/pdftool_' . bin2hex(random_bytes(12))` — a 24-character cryptographically random hex suffix, directory mode `0700`.
- Shell commands receive paths via `escapeshellarg()` — no user-controlled string ever reaches the shell unescaped.
- `timeout CMD_TIMEOUT` (120 seconds) wraps every external command — no process runs indefinitely.

### Process Isolation Sandbox

Every invocation of a heavy external tool — Ghostscript, Python, LibreOffice, Playwright, ImageMagick, and others — passes through a mandatory four-layer sandbox chain before the tool's process image is loaded. The architecture is **sandbox-by-default**: new tools added to `api.php` inherit the full chain automatically; an explicit opt-out (`NOSANDBOX_TOOLS`) is required to exempt a tool.

**Layer 1 — prlimit (kernel resource caps)**

The outermost layer calls `prlimit` with hard limits that the kernel enforces regardless of what code runs inside:

| Resource | Limit | Constant |
|---|---|---|
| Virtual address space (`--as`) | 1.5 GB | `RLIMIT_AS` |
| Maximum file size (`--fsize`) | 512 MB | `RLIMIT_FSIZE` |
| Process count (`--nproc`) | 256 processes | `RLIMIT_NPROC` |
| Open file descriptors (`--nofile`) | 512 | `RLIMIT_NOFILE` |

CPU time is **not** set via `prlimit --cpu` here. Setting `RLIMIT_CPU` combined with `unshare --pid --fork` (Layer 3) causes a kernel-level `sigprocmask` conflict where the SIGXCPU signal is delivered to the unshare stub rather than the tool process. CPU time is instead enforced by `ulimit -t` inside `pqpdf-sandbox` (Layer 4), applied after the PID namespace fork completes.

**Layer 2 — aa-exec / AppArmor (mandatory access control)**

`aa-exec -p pqpdf-unshare` transitions the calling process into the `pqpdf-unshare` AppArmor profile before `unshare` executes. This is required on Ubuntu 24.04+, where user namespace creation (`clone(CLONE_NEWUSER)`) is gated behind the AppArmor `userns` permission:

- Profile: `/etc/apparmor.d/pqpdf-unshare` — grants `userns` + `mount` permissions needed to create namespaces and bind-mount the job directory; denies all other filesystem writes outside the sandbox scratch area.
- A second profile (`/etc/apparmor.d/usr.local.bin.pqpdf-sandbox`) confines the setup script itself, restricting which binaries it may exec and which paths it may write.

**Layer 3 — unshare (Linux namespace isolation)**

`unshare` creates a set of independent Linux kernel namespaces. The tool and all its children run in a private environment isolated from the host:

| Namespace flag | Effect |
|---|---|
| `--user --map-root-user` | New user namespace — the process believes it is root (UID 0) but holds no real capabilities outside the namespace |
| `--net` | Private network stack with no interfaces — all `connect()` syscalls fail; the tool cannot reach the internet or internal services |
| `--pid --fork` | Isolated PID tree; PIDs start at 1 inside the namespace; child processes cannot escape to the host PID table |
| `--ipc` | Private shared memory and POSIX message queues; tools cannot communicate with other processes on the host via IPC |
| `--mount` | Private mount namespace; filesystem changes (bind mounts, tmpfs) are invisible to the host |

**Layer 4 — pqpdf-sandbox (filesystem isolation script)**

`/usr/local/bin/pqpdf-sandbox` is a shell script that runs inside the new namespaces. It is the innermost layer:

1. Mounts a 512 MB tmpfs at `/sandbox-scratch` — all scratch I/O happens on an in-memory filesystem that vanishes when the namespace exits.
2. Bind-mounts the job directory (passed via `PQPDF_WORKDIR`) into `/sandbox-scratch/work/` — the tool sees its input and writes output there.
3. Applies a per-process CPU time limit via `ulimit -t` — enforced inside the PID namespace where SIGXCPU is delivered correctly.
4. `exec`s the real tool binary. No shell remains after exec; the tool is the direct child process in the namespace.

**NOSANDBOX_TOOLS exemption**

Four tools are exempted from the full chain (`NOSANDBOX_TOOLS = ['pdfinfo', 'qpdf', 'pdfseparate', 'pdftotext']`):
- All four are read-only analysis tools — they never write output files that return to the user.
- All four have minimal attack surface (no interpreter, no JIT, no scripting engine).
- Sandboxing them would add 50–150 ms latency to lightweight info-fetch operations with no meaningful security gain.

**SANDBOX_MIN_LEVEL enforcement**

`SANDBOX_MIN_LEVEL = 'full'` means the server will refuse to run sandboxed tools in degraded mode — if any sandbox layer is unavailable (e.g. `aa-exec` not installed, `/usr/local/bin/pqpdf-sandbox` missing), the operation fails with an error rather than silently running unsandboxed. Degraded execution is always logged to the security event log.

### Zero Retention
- `send_file()` calls `readfile($path)` then immediately calls `cleanup($cleanup_paths)` and `exit`.
- The temp directory is deleted **while the download is still streaming** to the browser.
- No file content is written to any database. No file path is logged.

### Rate Limiting

Two independent rate-limiting layers protect against abuse:

**Session-based (per-browser session)**
- Sliding-window counter: **10 operations per 5-minute window** (`RATE_LIMIT_MAX = 10`, `RATE_LIMIT_WINDOW = 300`).
- Poll/keepalive operations (`edit-page`, `edit-ping`, `pdf-scan-poll`) are explicitly exempt to avoid blocking live progress UIs.
- Limit exceeded returns HTTP 429 with a plain-text message — no silent fail.

**IP-based (per source IP)**
- **30 operations per IP per 5-minute window** (`IP_RATE_LIMIT_MAX = 30`) — generous enough for NAT environments where many users share one IP, but still bounds individual abusers.
- Backed by Redis (`REDIS_HOST=127.0.0.1:6379`, namespace prefix `pqpdf:`) when available; falls back to atomic filesystem token-bucket files under `/tmp/pqpdf_rl/` when Redis is unreachable so the limit is always enforced.
- Returns HTTP 429 when the IP bucket is exhausted.

### Concurrency Limiting

A counting semaphore prevents resource exhaustion under simultaneous heavy load:

- **Maximum 4 concurrent heavy operations** (`MAX_CONCURRENT_JOBS = 4`) — enforced server-wide across all sessions.
- Backed by Redis INCR/DECR atomics with a `BG_JOB_TTL = 600` second expiry (stale slots auto-release); filesystem marker files under `/tmp/pqpdf_jobs/` serve as the fallback counting mechanism.
- Operations that are lightweight or poll-only (`edit-ping`, `edit-page`, `pdf-scan-poll`, `esign-status`, `get-info`, `fill-init`, etc.) are exempt from the semaphore.
- When the limit is reached the server returns HTTP 503 ("Server is busy processing other requests. Please try again shortly.") and records a `concurrency_limit_reached` security event.
- Background scan jobs are separately capped at **3 simultaneous jobs** (`MAX_BG_JOBS = 3`) with the same semaphore pattern.

### Security Event Logging
All security-relevant events are written as **NDJSON** (newline-delimited JSON) to `/var/log/pqpdf/security.ndjson`. One event per line; each line is a complete JSON object ingestible by Elasticsearch, Loki, Datadog, Grafana, or `jq`. If the log file is not writable the entry falls back to `error_log()` so no event is silently dropped.

**Logged events:**

| Event | Trigger | Key context fields |
|---|---|---|
| `invalid_method` | Non-POST request received | `method` |
| `unknown_operation` | `operation` param not in allow-list | `attempted_op` (truncated to 64 chars) |
| `rate_limit_exceeded` | Session hits 10 ops/5 min | `op_count`, `window_s`, `limit` |
| `file_size_exceeded` | Single file > 50 MB | `size`, `limit`, `filename` |
| `total_size_exceeded` | Merge batch > 200 MB | `total_sz`, `limit`, `files` |
| `repeated_pdf_validation_failure` | 3+ consecutive invalid PDF uploads in the same session | `fail_count`, `filename`, `size` |
| `invalid_page_input` | Page range contains non-integer token (e.g. `abc`, `1-2-3`) | `input` (control chars stripped) |
| `concurrency_limit_reached` | Server-wide job slot limit hit (`MAX_CONCURRENT_JOBS = 4`) | `limit` |

**Every event carries a common envelope:**

```json
{
  "ts": 1742256000,
  "event": "rate_limit_exceeded",
  "session": "a3f9c1b04d2e",
  "ip": "203.0.113.42",
  "op": "merge",
  "ua": "Mozilla/5.0 ...",
  "op_count": 10,
  "window_s": 300,
  "limit": 10
}
```

- **`session`** — first 12 hex chars of `sha256(session_id())`. Stable across a session; cannot be used to hijack it.
- **`ip`** — `$_SERVER['REMOTE_ADDR']` by default. Trusts `X-Forwarded-For` only when the `TRUST_PROXY=1` env var is set, preventing IP spoofing via header injection.
- **`ua`** — user-agent string with non-printable characters stripped, truncated at 200 chars.

**Validation failure threshold:** `$_SESSION['pdf_val_fails']` increments on each bad upload. The `repeated_pdf_validation_failure` event fires when the counter reaches 3 (`VAL_FAIL_THRESHOLD`), then resets — one event per burst, not per file.

**Log path override:** set `PQPDF_SECURITY_LOG=/path/to/custom.ndjson` in the server environment.

**Example `jq` queries:**

```bash
# All rate-limit hits in the last hour
jq 'select(.event == "rate_limit_exceeded" and .ts > (now - 3600))' \
  /var/log/pqpdf/security.ndjson

# Top IPs by event volume
jq -r '.ip' /var/log/pqpdf/security.ndjson | sort | uniq -c | sort -rn | head -20

# Full history for a specific session
jq 'select(.session == "a3f9c1b04d2e")' /var/log/pqpdf/security.ndjson

# Repeated validation failures only
jq 'select(.event == "repeated_pdf_validation_failure")' \
  /var/log/pqpdf/security.ndjson
```

**Log rotation** — use standard `logrotate`. Example `/etc/logrotate.d/pqpdf`:

```
/var/log/pqpdf/security.ndjson {
    daily
    rotate 30
    compress
    delaycompress
    missingok
    notifempty
    create 0640 www-data adm
}
```

### Content Security Policy
- Every tool page sets a strict CSP with per-request nonces: `script-src 'nonce-{ext}' 'nonce-{inline}'`.
- `style-src 'self'` — no inline styles anywhere in the HTML.
- No `unsafe-inline`, no `unsafe-eval`, no blob worker URLs.
- All event handlers are registered with `addEventListener()` in external JS modules; no `onclick` or `onload` attributes exist in any HTML.

### Contact Form Spam Protection
The contact form at `/contact/` layers four independent defences:

1. **AI behavioural verification** — client-side analysis of interaction patterns (mouse movement, timing, keystroke dynamics) via the pqcrypta verification API before the submit button is enabled.
2. **Honeypot fields** — two hidden inputs (`name="website"`, `name="url"`) that are invisible to humans. The JS explicitly includes their values in the JSON payload; `performSpamCheck()` in `contact/api/submit.php` throws `SpamException` if either field is non-empty.
3. **Server-side spam patterns** — regex list covering pharmaceutical spam, excessive capitalisation, disposable email domains, and common bot phrases.
4. **IP-based rate limit** — maximum 5 submissions per hour per IP enforced in PostgreSQL before any email is sent.

### Health Check Endpoint
`GET /health` — served by `health.php`, routed via Apache `RewriteRule ^/health(/.*)?$ /health.php [L,QSA]`.

**pqcrypta-proxy** polls this endpoint every 10–30 seconds per backend pool (`health_check_path = "/health"` in `proxy-config.toml`). A non-2xx response increments the circuit-breaker failure counter; 5 consecutive failures open the circuit and stop routing to that backend until 3 consecutive successes close it again.

Two modes:

| Request | Latency | Use case |
|---|---|---|
| `GET /health` | < 5 ms | Proxy liveness polling, uptime monitors |
| `GET /health?full=1` | < 2 s | Ops dashboards, post-deploy readiness gate |

**Checks performed:**

| Check | Mode | Critical → 503 | Degraded → 200 |
|---|---|---|---|
| Temp dir writable | both | ✓ if not writable | — |
| Disk free % | both | < 5% | 5–15% |
| Required tools (`gs`, `pdfunite`, `pdfinfo`, `qpdf`, `python3`) | `?full=1` | ✓ if any missing | — |
| Optional tools (11 tools incl. `soffice`, `tesseract`, `clamscan`) | `?full=1` | — | ✓ if any missing |
| Database connectivity | `?full=1` | — | ✓ if unreachable |
| Security log dir writable | `?full=1` | — | ✓ if not writable |

**Example liveness response:**
```json
{
  "status": "healthy",
  "ts": 1742256000,
  "server_id": "server-01",
  "checks": {
    "temp_dir": { "status": "ok", "path": "/tmp" },
    "disk":     { "status": "ok", "free_pct": 33.1, "free_gb": 76.69 }
  }
}
```

**`server_id`** is set via the `SERVER_ID` environment variable on each node (`SERVER_ID=server-01`, `SERVER_ID=server-02`, etc.). Falls back to `gethostname()`. Included in every response so load balancer access logs identify which backend answered each probe.

**`Cache-Control: no-store`** is set on all health responses to prevent proxies from serving a stale healthy status after a node goes down.

---

## Enterprise / On-Premise

**[pqpdf.com/enterprise.php](https://pqpdf.com/enterprise.php)**

The full PQ PDF engine — all 45 tools, all 44 forensic engines, the four-layer sandbox, ML models with SHAP explainability, TLSH campaign attribution, fully offline threat intelligence (6.4M+ local indicators — URLhaus, MalwareBazaar, ThreatFox), and post-quantum cryptography — packaged for deployment inside your own infrastructure.

### Why On-Premise

| Concern | SaaS risk | On-premise resolution |
|---|---|---|
| **Data sovereignty** | Files traverse third-party networks and may be retained | Files never leave your network — zero-egress architecture |
| **Compliance** | GDPR, HIPAA, FedRAMP, ISO 27001 require data-handling controls you cannot audit in SaaS | Deploy behind your own firewall; your DPO controls every processing step |
| **Cost at scale** | Per-page or per-seat SaaS pricing compounds with volume | One server instance; unlimited internal use at flat infrastructure cost |
| **Security posture** | Shared multi-tenant infrastructure; you cannot verify isolation | Dedicated host; your own AppArmor profiles, SELinux policy, network rules |
| **Air-gap environments** | SaaS requires internet egress | Fully offline-capable — no phone-home, no licence server, no CDN dependency |

### What the Enterprise Page Covers

- **Security architecture** — four-layer sandbox (prlimit → AppArmor → Linux namespaces → pqpdf-sandbox), zero-retention file lifecycle, NIST PQC standards (FIPS 203/204/205)
- **Cost & ROI** — documented breach incidents with verified financial penalties (MOVEit, Change Healthcare, Anthem, Equifax, Capital One, British Airways, Morgan Stanley, and others), vs. on-premise deployment cost
- **Threat intelligence** — current PDF attack statistics (Check Point Research 2025, HP Wolf Security 2024–2025, Kaspersky 2024, Verizon DBIR 2025), recent CVEs in CISA KEV (CVE-2023-26369, CVE-2023-21608), and the PDFSider campaign (January 2026)
- **Compliance** — GDPR Article 83, HIPAA §164.312, CCPA, SOX, FedRAMP Ready, UK GDPR, PCI DSS
- **Feature comparison** — public (free) tier vs. enterprise on-premise, and PQ PDF vs. SaaS competitors
- **Deployment overview** — server requirements, four-layer sandbox, PostgreSQL persistence, Redis concurrency, ClamAV daemon, ML model retraining

Contact: **contact@pqcrypta.com** with subject `Enterprise / On-Premise Deployment`.

---

## Comparison

Facts are derived from code (`api.php` constants, engine list, scan.php source). Competitor claims are based on their published documentation and terms of service as of early 2026.

| Feature | PQ PDF PDF Tools | Adobe Acrobat Online | SmallPDF | iLovePDF | PDF24 | Sejda |
|---|---|---|---|---|---|---|
| File retention after processing | **Deleted during download** (cleanup in send_file()) | Up to 1 hour (published policy) | Up to 1 hour | Up to 2 hours | Up to 1 hour | Up to 1 hour |
| Account required | **No** | Yes (for most features) | No (limited) | No (limited) | No | No |
| Threat scanning engines | **44 independent engines** incl. ML + SHAP + sandbox + TI + MITRE ATT&CK + phishing + campaign attribution | None | None | None | None | None |
| ML-based anomaly detection | **Yes** (IsolationForest + RandomForest + LightGBM ensemble, 38-feature vector, continuously retrained, model drift detection) | No | No | No | No | No |
| Dynamic behavioral sandbox | **Yes** (strace + Linux namespaces, full syscall trace) | No | No | No | No | No |
| Post-quantum encryption | **Yes** (31 algorithms via @noble/post-quantum, client-side) | No | No | No | No | No |
| OCR engine | **Tesseract 5 LSTM** (confidence scoring, searchable PDF output, TSV word-level stats) | Adobe Sensei | Acrobat engine | Tesseract | Tesseract | Tesseract |
| PDF edit tools | **16 annotation types** incl. sticky notes, QR code, stamps + bookmark editor | Many | Limited | Limited | Limited | Limited |
| Threat intelligence lookups | **Yes** (URLhaus · MalwareBazaar · ThreatFox — 6.4M+ indicators, fully local, zero external API calls per scan) | No | No | No | No | No |
| MITRE ATT&CK mapping | **Yes** (55-entry lookup, every indicator tagged) | No | No | No | No | No |
| Phishing detection | **Yes** (Engine ㉓ — urgency phrases, brand impersonation, AcroForm credential harvesting, QR codes) | No | No | No | No | No |
| Campaign attribution (fuzzy hash) | **Yes** (Engine ㉕ — TLSH similarity hash vs confirmed-malicious history) | No | No | No | No | No |
| Transparency (engine names) | **Ghostscript, Poppler, LibreOffice, Tesseract, PyMuPDF, ExifTool, YARA, ClamAV, PeePDF, pikepdf, strace, acorn, scikit-learn, lightgbm, shap, tlsh, imagehash, zbarimg** | Undisclosed | Undisclosed | Undisclosed | Undisclosed | Undisclosed |
| Open-source engines only | **Yes** — every engine is named open-source software | No | No | No | No | No |
| Max upload (free) | **50 MB / file, 200 MB total** | 100 MB (Adobe account) | 5 GB (with account) | 200 MB | 200 MB | 200 MB |
| JavaScript AST deobfuscation | **Yes** (Engine ⑳, acorn parser) | No | No | No | No | No |
| Differential parsing (6 parsers, 8 dimensions) | **Yes** (Engine ⑰ — MuPDF · Poppler · Ghostscript · qpdf · pdfminer · pdf.js) | No | No | No | No | No |

---

## PDF Forensics Scanner — 25 Forensic Engines

[/tools/scan.php](https://pqpdf.com/tools/scan.php)

**44 forensic engines** — the most comprehensive free PDF threat analysis available, including techniques absent from every commercial static scanner:

**New engines (32–43):** XFA FormCalc Script Extractor (the only static tool that parses FormCalc — exec/openURL/submit exfiltration/initialize auto-execute) · PDF Action Dependency Graph (directed graph of all actions — cycles, deep chains, fan-in maximization, dead sleeper nodes) · OCG Layer Cloaking (hidden optional content groups with never-visible and screen/print divergence detection) · Unicode & Invisible Text (rendering mode 3/7 invisible text, RLO override attacks U+202E, homograph domains) · Trailer Chain Forensics (raw /Prev chain walk — ID mutation, /Root swap Shadow Attack, encryption changes) · Codec Exploit Parameter Validation (CCITTFax OOB, JBIG2Globals CVE-2009-0658, DCT mismatch, LZW EarlyChange, multi-filter chains) · Physical Entropy Topology (PDF-structure-aware sliding-window entropy — post-EOF data, entropy cliffs) · Image Steganography (LSB chi-square test, tracking beacons, JPEG EXIF anomalies) · PDF/A Compliance Fraud Detection (false archival claims bypass DLP) · JavaScript Behavioral Emulation (Node.js vm + full Acrobat API stub — captures LAUNCH_URL, SUBMIT_FORM, MAIL at runtime) · Font CharString Stack Machine Emulator (Type 1 bytecode decryption + emulation) · Cross-Object XRef Integrity Graph (phantom objects, orphan sleeper payloads, free-entry bugs, length fraud).

**Existing engines:** 15 static heuristic engines · dynamic behavioral sandbox (Linux namespace isolation, syscall tracing) · ML Intelligence Engine (SHAP explanations) · differential parsing across six independent parsers · polyglot binary detector · JavaScript AST deobfuscation · AcroForm field forensics · document revision history · annotation forensics · named tree analysis · content stream forensics · object stream analysis · threat intelligence (6.4M+ local indicators) · PDF signature forensics · phishing detection · embedded file analysis · TLSH fuzzy-hash campaign attribution · weighted correlation engine (60+ compound patterns). All 44 engines run server-side in a single request. Every indicator is tagged with a MITRE ATT&CK technique ID.

### How It Works

1. The PDF is uploaded and saved to an isolated temporary directory. A session token is returned for optional sanitize follow-up.
2. A Python script runs all 16 static heuristic engines (including ExifTool, qpdf, YARA, PeePDF, AcroForm Field Forensics, Document Revision History, Annotation Forensics, Named Tree Analysis, Content Stream Forensics, and Object Stream Analysis) against the raw bytes and parsed object graph via PyMuPDF.
3. Engine ⑭ renders the PDF through Ghostscript, MuPDF, and Poppler inside isolated Linux namespaces (`unshare --net --pid --mount`) with all syscalls captured by `strace`. Detects runtime behavior invisible to static analysis.
4. Engine ⑮ calls `clamdscan --no-summary` against the PDF. The `clamav` user is a member of the `www-data` group so the clamd daemon reads upload files directly — no `--fdpass` needed, no slow single-process fallback (~20 ms per scan).
5. Engine ⑯ extracts a 38-feature vector from all preceding engine outputs, applies Bayesian contextual scoring, runs IsolationForest anomaly detection (unsupervised — works from scan 1), RandomForest classification (activates at ≥50 labeled samples), and LightGBM ensemble scoring (gradient boosting, class-imbalance weighted). Reports per-scan feature importance via SHAP TreeExplainer (RandomForest/LightGBM) and KernelExplainer (IsolationForest). Model drift detection warns when models are >30 days old. Scan features, scores, and auto-inferred labels are persisted to PostgreSQL. Training runs every 30 minutes via cron.
6. Engine ⑰ runs MuPDF (`mutool`), Poppler (`pdfinfo`/`pdfdetach`), Ghostscript, qpdf, pdfminer, and Node.js pdf.js independently and cross-compares 8 dimensions: page count, object count, JavaScript presence, PDF version, encryption status, AcroForm presence, embedded file count, and OpenAction. Seven distinct discrepancy checks (Critical/High/Medium) flag hidden objects, shadow object trees, or deliberate parser-confusion exploits. Page delta scoring is weighted by magnitude (up to +70/critical for >50 page delta). A hard 30-second SIGALRM wraps the engine; pdfminer runs in a subprocess with `timeout 6` for guaranteed hard-kill.
7. Engine ⑱ scans every stream (raw and decompressed) for file magic byte signatures — ZIP, Windows PE, Linux ELF, Mach-O, Java class, OLE/CFBF, RAR, 7-Zip, embedded PostScript, HTML/XHTML, WebAssembly, Python bytecode — and also performs mid-stream scanning at non-zero offsets to catch embedded payloads prefixed by junk bytes. JAR files are detected by ZIP+META-INF/MANIFEST.MF signature. Detects polyglot files that embed executable droppers inside a valid PDF container.
8. Engine ⑲ extracts JavaScript from `/JS` literals and keyword-bearing compressed streams (with Unicode `\uXXXX` pre-processing), parses each through the Acorn AST parser (ECMAScript 2022), and walks the AST detecting obfuscation constructs invisible to text-pattern matching: `eval()` chains, `String.fromCharCode()` arrays (shellcode staging), `unescape()` decode pipelines, large numeric arrays (heap spray), `new Function()` dynamic construction, `atob()`/`btoa()` base64 decode chains, and property accessor obfuscation patterns.
9. Engine ⑳ queries four local PostgreSQL databases — no external API calls, no rate limits, sub-millisecond per scan. **URLhaus hashes** (5M+ SHA-256 malware payload hashes), **URLhaus URLs** (70K+ malicious URLs, refreshed every 30 min by cron), **MalwareBazaar** (1M+ confirmed malware samples with family labels), **ThreatFox IOCs** (176K+ hashes, URLs, and domains). A confirmed match raises a Critical indicator and auto-labels the scan as `malicious` in the training database.
10. Engine ㉑ analyses PDF digital signatures via pyhanko: computes ByteRange coverage (gaps = unsigned content = shadow document attack), and diffs object inventories across incremental revisions after signing to detect execution vectors added post-signing.
11. Engine ㉒ runs phishing analysis: 30+ urgency/deception phrases, brand impersonation keywords (Microsoft, Apple, PayPal, DocuSign, etc.), AcroForm `SubmitForm` + password-field credential harvesting detection, and QR code decoding via `zbarimg` with suspicious domain scoring.
12. Engine ㉓ uses `pdfdetach` to extract every embedded file attachment and inspects each for PE/ELF/OLE/OOXML/script magic bytes, VBA macro detection in OOXML containers, strings extraction from executables, nested PDF detection, and full ZIP content listing for non-OOXML archives (flags dangerous executables/scripts inside ZIP payloads).
13. Engine ㉔ computes a TLSH (Trend Locality Sensitive Hash) of the full PDF and a pHash perceptual hash of each page thumbnail — both similarity-preserving hashes. TLSH score <30 = near-identical; <100 = same campaign family. pHash hamming distance ≤8 = visual match. Also fingerprints extracted JavaScript by MD5 of sorted fragments for code-similarity matching. Campaign name is surfaced from MalwareBazaar family labels when a cluster match is found.
14. Engine ㉕ analyses all AcroForm widget fields: JS triggers on field events (/AA keystroke/validate/calculate), SubmitForm exfiltration targets, hidden/NoExport fields, password-type fields, and calculation-order (/CO) chain exploitation.
15. Engines ㉖–㉚ run the five structural-depth passes: Document Revision History (per-%%EOF metadata + object deltas), Annotation Forensics (all /Annot URI/JS/Launch/GoToR/SubmitForm actions), Named Tree Analysis (/Names /JavaScript registry, /AA count, /DocMDP, /Perms, UR3), Content Stream Forensics (PostScript exec/run/token/setpagedevice operators, ICC profile abuse, content bombs), and Object Stream Analysis (decompress every /ObjStm, re-scan for JS/Launch/EmbeddedFile/high-entropy payloads).
16. Engine ㉛ (Correlation Engine) examines combinations of findings from all 30 preceding engines and adds weighted bonus points (35–100) for dangerous combinations (e.g. JavaScript + `/OpenAction` + high entropy = +100). 50+ compound patterns. Final score capped at 999.
17. All 55 MITRE ATT&CK technique mappings are applied across all indicators. `mitre_techniques` list added to scan result for SIEM/SOAR integration.
18. All indicators are deduplicated, sorted by risk level, and returned as JSON with a composite risk score, ML malicious-probability score, and MITRE ATT&CK technique list.
16. The client renders a 23-tab report: 📊 Summary, ⚠️ Threats, 📈 Score, ⚙️ Engines (per-engine two-panel browser — click any of 44 engines to see its full findings, structure fields, and special data), 🌐 URLs, 📦 Streams, 🧠 ML (LightGBM probability, SHAP bar chart, feature importances), 🔬 Sandbox, 🌍 Threat Intel, 🎯 MITRE ATT&CK, 🔬 Parsing, 🧬 Polyglot, 🎣 Phishing, 📎 Embedded, ✍️ Signature, 📜 History, 📌 Annotations, 🏷️ Metadata, 📋 Raw JSON, and 🔍 Raw Forensics (decoded stream content, JavaScript sources, all indicator contexts, complete structure dump). Clickable stat cards on the Summary tab navigate directly to the corresponding tab. An animated engine-chip strip with 31 chips shows each engine completing in sequence during the scan.

### Scoring

Each indicator contributes base points multiplied by `min(count, 3)`:

| Risk Level | Base Points |
|---|---|
| Critical | 50 |
| High | 25 |
| Medium | 10 |
| Low | 3 |

The Correlation Engine (㉕) adds weighted bonus points (35–100) on top for dangerous indicator combinations, using log-scaled weighted voting and ML anomaly feedback amplification. Final score is capped at 999.

| Score | Risk Level |
|---|---|
| 0 | Clean |
| 1–14 | Low |
| 15–54 | Suspicious |
| 55–999 | Dangerous |

### Engine ① — Structure Validator

Validates the fundamental file structure before any content analysis:

- **Header position** — flags `%PDF-` found beyond byte offset 1024 (exploit obfuscation technique)
- **`%%EOF` markers** — counts end-of-file markers; >2 indicates incremental update stacking or exploit layering
- **XRef table count** — flags >3 cross-reference tables; complex update chains can hide malicious objects
- **Obfuscation codecs** — counts `ASCIIHexDecode`, `ASCII85Decode`, `LZWDecode` occurrences; >3 flags multi-layer encoding used to evade static scanners
- **Excessive filter chains** — flags >120 `/Filter` entries in a single file (abnormal density indicating deeply nested stream obfuscation)
- **Incremental injection detection** — flags documents where the final incremental revision introduces >10 new objects (proportional injection heuristic; legitimate revisions add at most a handful of objects for annotations or form fills; >10 new objects in the last revision is a strong indicator of a malicious payload injected post-signing)
- **Structural data collected** — `pdf_version`, `linearized` flag, binary comment presence, `eof_markers`, `xref_tables`, `filter_count`

### Engine ② — Raw Pattern Scanner

Scans raw file bytes for 45+ known-malicious byte sequences across six categories. Each match records the occurrence count and a 80-byte context snippet (20 bytes before, 60 bytes after) for the Threats tab.

**JavaScript execution**
`/JavaScript`, `/JS` (space / CR / LF / paren / hex variants), `/Launch`, `/OpenAction`, `/AA`

**Remote & form actions**
`/GoToR`, `/GoToE`, `/SubmitForm`, `/ImportData`, `/Named`, `/Rendition`, `/Sound`, `/Movie`, `/Hide`

**Embedded & rich content**
`/EmbeddedFile`, `/RichMedia`, `/XFA`, `/AcroForm`

**Obfuscation structures**
`/ObjStm`, `/Encrypt`, `/JBIG2Decode`, `/ASCIIHexDecode`, `/ASCII85Decode`

**Dangerous JavaScript APIs**
`unescape()`, `eval()`, `String.fromCharCode`, `this.exportDataObject`, `this.submitForm`, `app.openDoc`, `collab.getIcon` (CVE-2009-0927), `util.printf` (CVE-2008-2992), `util.printd` (CVE-2007-5020), `media.newPlayer` (CVE-2009-4324), `Collab.collectEmailInfo` (CVE-2007-5659)

**Page transition evasion**
`/Trans` combined with JavaScript — page-transition dictionaries used to fire JavaScript during slide transitions, a known evasion technique that delays execution until a page is displayed. `/OpenAction` hidden via an AcroForm `/DR` indirect reference variant — malicious `/OpenAction` entries obscured inside the default-resource dictionary of an AcroForm to evade parsers that only check the document catalog directly.

**Shellcode & heapspray signatures**
`%u9090` (Unicode NOP sled), `%u4141` (classic heapspray fill), `%u0c0c%u0c0c` (0x0C heap-fill pattern), `%u0d0d%u0d0d` (0x0D heap-fill pattern)

### Engine ③ — Stream Decompressor & Content Inspector

Opens every object in the xref graph (up to 6,000 objects) via PyMuPDF and inspects each stream:

- **Decompression** — calls `doc.xref_stream(xref)` to decompress FlateDecode and other encoded streams, then re-scans the decompressed content — catching JavaScript and shellcode hidden inside compressed objects that raw-byte scanners miss entirely
- **Sliding-window entropy** — calculates per-stream entropy using 512-byte sliding windows; any window exceeding 7.6 bits/byte on non-image streams flags encrypted, packed, or obfuscated payloads (detects shellcode splices that average out in whole-stream analysis). Decompression bomb detection flags streams with >500:1 compression ratio. Image streams (`/DCTDecode`, `/JPXDecode`, `/CCITTFax`, `/JBIG2`) are excluded from entropy flagging.
- **14 JS/shellcode signatures** scanned in decompressed content: `function `, `var `, `eval(`, `unescape(`, `String.fromCharCode`, `this.exportDataObject`, `app.openDoc`, `collab.`, `util.printf`, `.submitForm`, `%u9090`, `%u4141`, `\x0c×8`
- **Stream type classification** — `data`, `font`, `xobject`, `javascript`, `embedded`
- **Report data** — up to 40 streams returned with xref number, decompressed size, entropy value, type, suspicious flag, and matched pattern list

### Engine ④ — Object Graph Analyzer

Walks the full xref graph (up to 6,000 objects), reads every object dictionary, and checks for 10 dangerous action-type combinations:

| Dictionary Pattern | Label | Risk |
|---|---|---|
| `/S /Launch` | Launch action object | Critical |
| `/S /JavaScript` | JavaScript action object | Critical |
| `/S /SubmitForm` | Form submit action object | Medium |
| `/S /ImportData` | Import data action object | Medium |
| `/S /GoToR` | Remote go-to action object | Medium |
| `/S /GoToE` | Embedded navigation action | Medium |
| `/S /Named` | Named action object | Medium |
| `/S /Rendition` | Rendition action object | Medium |
| `/RichMedia` | Rich media annotation | High |
| `/XFA` | XFA form object | High |

Reports the exact xref object numbers of all matched Launch and JavaScript objects for forensic attribution.

### Engine ⑤ — URL Extractor

Extracts all HTTP/HTTPS URLs from two passes:

1. **Raw bytes** — regex scan of the entire file (`https?://` followed by 4–250 non-whitespace chars)
2. **Decompressed streams** — same regex applied inside every decompressed stream, catching URLs embedded inside compressed objects

**data: URI scheme detection** — scans streams for `data:` URI schemes (e.g. `data:text/html`, `data:application/octet-stream`) used to encode and deliver payloads inline without a resolvable HTTP URL, bypassing URL-reputation filters.

**Hex-encoded URL detection** — scans JavaScript content for hex-escape sequences spelling `http`: `\x68\x74\x74\x70` and variants. A common obfuscation technique to hide URL strings from text-pattern and regex-based scanners while remaining fully functional at runtime.

URLs are de-duplicated and capped at 150. The Correlation Engine cross-references extracted URLs and flags three patterns as High: IP-literal addresses (`http://1.2.3.4/...`), raw port numbers (`http://host:4444/...`), and 12+ character random-looking subdomains.

### Engine ⑥ — Metadata Analyzer

Reads all standard PDF metadata fields via PyMuPDF (`title`, `author`, `subject`, `keywords`, `creator`, `producer`, `creationDate`, `modDate`, `format`) and performs four checks:

- **Exploit-tool strings** — scans `Creator` and `Producer` fields for known strings: `exploit`, `metasploit`, `canvas`, `core impact`, `meterpreter`, `shellcode`, `payload`, `pdfcrack`, `hashcat`, `dompdf exploit`. A match reports the exact field value as Critical.
- **Empty metadata** — flags PDFs with no title, author, creator, or producer. Exploit-crafted PDFs routinely strip all metadata to reduce forensic attribution and avoid reputation-based sandbox triggers. Reported as Low; escalated by the Correlation Engine when combined with JavaScript or embedded files.
- **XMP stream** — reads the XML metadata stream and flags any `<script` or `javascript` reference as High.
- **Creation/modification timestamp delta** — computes the elapsed seconds between `creationDate` and `modDate`. A delta of 0–5 seconds indicates the document was machine-generated (creation and modification recorded in the same scripted run), a pattern consistent with automated exploit-kit tooling rather than a human-authored document. Reported as Low; escalated by the Correlation Engine when combined with other exploit indicators.

### Engine ⑦ — Font Anomaly Detector

Inspects every object dictionary containing `/Font` for two historically exploited patterns:

- **`/JBIG2Decode` in font streams** — the JBIG2 image compression codec linked directly to CVE-2009-0658 (critical Adobe Reader 0-day, all versions ≤9.0, CVSS 9.3) and CVE-2010-0188 (LibTIFF heap overflow via embedded TIFF). Reported as Critical.
- **Oversized `/Widths` arrays** — font objects with `/Widths` arrays longer than 600 characters, matching the abnormally large glyph-width arrays used in historic heap overflow attacks against Acrobat's font rendering engine. Reported as High.

### Engine ⑧ — CVE Pattern Matcher

Nine specific CVE signatures matched by boolean lambda tests against raw bytes:

| Match Condition | CVE / Label | Severity |
|---|---|---|
| `/JBIG2Decode` + `/JavaScript` both present | CVE-2009-0658 | Critical |
| `/TIFF` + (`/Launch` or `/JavaScript`) | CVE-2010-0188 | Critical |
| `util.printf` present | CVE-2008-2992 | Critical |
| `collab.getIcon` present | CVE-2009-0927 | Critical |
| `util.printd` present | CVE-2007-5020 | Critical |
| `Collab.collectEmailInfo` present | CVE-2007-5659 | Critical |
| `media.newPlayer` present | CVE-2009-4324 | Critical |
| `%u0c0c%u0c0c` or binary `\x0c×8` | Heapspray (0x0C fill) | Critical |
| `%u0d0d%u0d0d` | Heapspray (0x0D fill) | Critical |

### Engine ⑨ — Structural Statistics

Collects document-level statistics via PyMuPDF for the 15-cell summary dashboard:

| Stat | Source |
|---|---|
| Page count | `doc.page_count` |
| Object count | `doc.xref_length() - 1` |
| Encrypted | `doc.needs_pass` |
| Embedded file count | `doc.embfile_count()` |
| Form field count | `doc.get_fields()` |
| Annotation count | Sum of `page.annots()` across all pages |
| Link count | Sum of `page.get_links()` across all pages |
| File size | `os.path.getsize()` |
| PDF version | From Engine ① |
| %%EOF markers | From Engine ① |
| XRef tables | From Engine ① |
| Total streams | From Engine ③ |
| High-entropy streams | From Engine ③ |

**Object-to-page ratio heuristic** — computes `object_count / page_count`. A ratio >50 objects per page is anomalous for normal documents (a typical 10-page report has 200–400 objects total) and is a strong indicator of exploit payload inflation — large numbers of synthetic objects used for heap-spray staging or to bury malicious streams inside a legitimate-looking document. Reported as Medium.

**Zero-page detection** — a PDF reporting 0 pages is not a document at all; it is a pure exploit container with no renderable content. Zero-page PDFs are used exclusively to deliver shellcode, embedded executables, or JavaScript payloads with no visible content to distract victims or analysts. Reported as Critical.

### Engine ⑩ — ExifTool Metadata Forensics

Runs `exiftool -PDF:all -XMP:all` to extract metadata layers that are invisible to PyMuPDF's document model:

- **Exploit-kit fingerprinting** — scans Creator, Producer, Author, Subject, Keywords, and XMPToolkit fields for known exploit-tool strings: Metasploit, msfvenom, Canvas, Core Impact, shellcode, payload, pdfcrack, dompdf exploit
- **XFA confirmation** — independently verifies `HasXFA` from EXIF metadata (cross-check against Engine ④)
- **Embedded attachment detection** — surfaces `EmbeddedFileSize` / `EmbeddedFile` fields visible only via EXIF layer
- **Summary export** — Creator, Producer, CreateDate, ModifyDate, PDFVersion, Linearized, PageCount, HasXFA, and Encryption exported to the structure dictionary for the Metadata tab
- **Feeds Correlation Engine** — sets `exiftool_exploit_found` flag used by Engine ㉕ for compound scoring

### Engine ⑪ — qpdf Structural Integrity

Runs `qpdf --check` to validate cross-reference tables, trailer dictionaries, and overall document structure:

- **XRef reconstruction** — if qpdf must reconstruct the xref table, reports as High risk (deliberately broken xref is a common technique to hide exploit objects from basic parsers while still rendering in vulnerable viewers)
- **Damaged structure** — explicit "damaged" report from qpdf is flagged as High risk (intentional corruption to conceal exploit content from scanners)
- **Structural errors** — other qpdf errors flagged as Medium risk (up to 3× count multiplier)
- **Structural warnings** — minor anomalies flagged as Low risk
- **Status export** — `qpdf_status` (ok / warnings / errors / damaged) exported to structure dictionary
- **Feeds Correlation Engine** — sets `qpdf_damaged` flag used by Engine ㉕ for compound scoring

### Engine ⑫ — YARA Rule Engine

Compiles and matches 21+ custom YARA rules targeting PDF-specific attack byte patterns — independent of PDF structure parsing. Also loads any external `.yar` rule files from a configured rules directory:

| Rule | Detects |
|---|---|
| `PDF_Heapspray_Classic` | `%u9090%u9090`, `\u9090\u9090`, `%u0c0c%u0c0c`, `\u0c0c\u0c0c`, `%u0d0d%u0d0d`, binary 16-byte NOP sled, `0x0c0c0c0c` |
| `PDF_JS_Shellcode_Loader` | `eval()+unescape()`, `eval()+String.fromCharCode`, `unescape()+String.fromCharCode` combinations |
| `PDF_CVE_2009_0658` | `/JBIG2Decode` combined with `/JavaScript` or `/OpenAction` at byte level |
| `PDF_CVE_2008_2992` | `util.printf` and `%8999999999` format-string overflow patterns |
| `PDF_Suspicious_Launch` | `/S /Launch` and `/S/Launch` direct byte patterns |
| `PDF_AutoOpen_Executable` | `/OpenAction` combined with `/JavaScript`, `/Launch`, or `/EmbeddedFile` |
| `PDF_Obfuscated_Hex_Keywords` | `#6A#61#76#61` ("java" hex), `#65#76#61#6C` ("eval" hex) — PDF name obfuscation |
| `PDF_XFA_Script_Exploit` | `/XFA` combined with `/JavaScript` or `<script` |
| `PDF_RichMedia_Vector` | `/RichMedia`, or `.swf`+`Flash` combo |
| `PDF_AA_Malicious_Trigger` | `/AA` combined with `/JavaScript` or `/Launch` |
| `PDF_Encoder_Chain` | Any 3 of `/ASCII85Decode`, `/ASCIIHexDecode`, `/RunLengthDecode`, `/LZWDecode` |
| `CVE_2010_1240` | `/Launch` + `/Win` byte pattern (CVE-2010-1240 launch action exploit) |
| `CVE_2018_4990` | Double-free pattern bytes associated with CVE-2018-4990 |
| `CVE_2021_XFA` | XFA + JavaScript combined exploit pattern |
| `PowerShell_Encoded` | Base64-encoded PowerShell (`cABvAHcAZQByAHMAaABlAGwAbA`) or `-EncodedCommand` |
| `Base64_Chain_Decode` | Multi-stage base64 decode chain patterns |
| `Unicode_Obfuscation` | `\u0065\u0076\u0061\u006C` and similar Unicode-escaped JavaScript keyword obfuscation |
| `CobaltStrike_Beacon` | Known Cobalt Strike stager byte patterns |
| `XMP_Injection` | XMP metadata containing script or executable injection markers |
| `Phishing_Template` | Brand impersonation + credential harvesting form structure byte patterns |
| `Dropper_Launcher` | Multi-stage dropper launch sequence patterns |

- **Byte-level independence** — YARA scans raw file bytes, bypassing PDF parser layers entirely; catches patterns hidden in object streams
- **Feeds Correlation Engine** — populates `yara_hits` set used by Engine ㉕ for compound scoring

### Engine ⑬ — PeePDF Deep Analysis

Parses the full PDF object tree using the PeePDF framework — an entirely independent parser from PyMuPDF:

- **Vulnerability patterns** — PeePDF's built-in vulnerability detector flags known CVE pattern combinations; each confirmed pattern reported as Critical
- **Suspicious element location** — reports exact object IDs for dangerous elements from both `Suspicious elements` and `Dangerous elements` dictionaries: `/Launch`, `getIcon()`, `printf()`, `unescape()`, `exportDataObject()`, `submitForm()`, `/EmbeddedFile`, `/JS`, `/JavaScript`, `eval()`, `/OpenAction`, `/AA`, `/XFA`, `/URI`
- **JavaScript object inventory** — lists all PDF object IDs containing JavaScript
- **Independent verification** — cross-checks PyMuPDF-based findings; if both parsers flag the same element, the compound risk in Engine ㉕ is elevated
- **Summary export** — PDF version, object count, stream count, and vulnerability count exported to structure dictionary
- **Feeds Correlation Engine** — sets `peepdf_vuln_count` used by Engine ㉕ for compound scoring

### Engine ⑭ — Dynamic Behavioral Sandbox

The only engine that actually *executes* the PDF. Renders the file through three independent interpreters inside fully isolated Linux process namespaces, capturing all syscalls via `strace`:

**Isolation architecture**

| Layer | Mechanism | Effect |
|---|---|---|
| Network namespace | `unshare --net` | New network stack with no interfaces — all `connect()` calls fail and are logged |
| PID namespace | `unshare --pid --fork` | Isolated process tree; child PIDs cannot escape |
| Mount namespace | `unshare --mount --mount-proc` | Isolated `/proc` — sandbox cannot inspect host processes |
| Syscall tracing | `strace -f -q` | Every syscall of every child process captured to log |
| Time limit | `timeout 20` per renderer | Hang/loop exploits terminated and flagged |

**Renderers run (in sequence)**

| Renderer | Binary | What it exercises |
|---|---|---|
| Ghostscript | `gs -dNOSAFER -sDEVICE=nullpage` | Full PostScript interpreter + PDF JavaScript engine |
| MuPDF | `mutool draw -o /dev/null` | Compact independent C renderer; different parser attack surface |
| Poppler | `pdftotext` | Stream-based text extractor; distinct xref/stream handling |

**Behavioral threat indicators**

| Indicator | Detection | Risk |
|---|---|---|
| **Network beacon** | `connect()`/`socket()` with `AF_INET`/`AF_INET6` in isolated namespace | Critical (+80) |
| **Shellcode execution** | `mmap()` with `PROT_EXEC|MAP_ANONYMOUS` — anonymous executable memory | Critical (+70) |
| **Process spawn** | `execve()` of any binary not in the renderer launch chain | Critical (+85) |
| **Filesystem escape** | `openat()` with `O_WRONLY`/`O_RDWR` to paths outside sandbox dir | High (+60) |
| **Process bomb** | >50 `fork`/`clone`/`vfork` syscalls | High (+40) |
| **Render timeout** | Renderer exceeds 20-second execution limit | High (+35) |

- **Network isolation guarantee** — in a network namespace with no interfaces, any `connect()` is definitively malicious. There is no legitimate reason for a PDF renderer to initiate a network connection.
- **Feeds Correlation Engine** — sets `sandbox_network_attempts`, `sandbox_mmap_exec_anon`, `sandbox_exec_attempts`, `sandbox_behavioral_score` flags used by Engine ㉕ for compound scoring

### Engine ㉕ — Correlation Engine

Cross-references all findings from Engines ①–㉔ and scores 45+ dangerous indicator combinations that are orders of magnitude more serious than their individual parts. Each matched combination adds a weighted bonus on top of the base indicator scores.

**Weighted voting with log-scaling** — the engine uses a `weighted_vote()` function that applies logarithmic scaling to multi-engine confirmation signals, so each additional independent engine confirming a threat adds a progressively diminishing but always positive score increment. This prevents runaway score inflation from repeated low-quality signals while still rewarding genuine cross-engine convergence.

**ML-Correlation feedback boost** — when the ML Intelligence Engine (⑯) reports an anomaly score >0.7, the Correlation Engine amplifies its own compound scoring by a fixed boost factor. High ML anomaly confidence is a strong independent signal that the document's feature vector is structurally unusual, and the boost ensures that ML-flagged documents receive appropriately elevated correlation scores even when individual indicator combinations fall just below bonus thresholds.

**Multi-engine JavaScript confirmation bonus** — when 3 or more independent engines each independently confirm JavaScript presence (e.g. Engine ②, Engine ④, Engine ⑬, Engine ⑱ all flag JS), an additional confirmation bonus is applied on top of existing JS-combination bonuses. Independent corroboration from multiple parsers and analysis methods substantially reduces false-positive risk and justifies elevated scoring.

**Auto-execution combinations (highest danger)**

| Combination | Bonus | Why |
|---|---|---|
| `/OpenAction` + JavaScript | +75 | Document auto-executes JS on open — zero user interaction required |
| `/OpenAction` + `/Launch` | +80 | Auto-launches external program on open |
| JavaScript + `/Launch` | +80 | Script-controlled arbitrary program execution |

**Payload delivery combinations**

| Combination | Bonus | Why |
|---|---|---|
| JavaScript + `/EmbeddedFile` | +65 | `exportDataObject()` drops attachment to disk and executes it |
| JavaScript + `/XFA` | +45 | XFA+JS full document scripting — multiple historic critical CVEs |
| JavaScript + `/RichMedia` | +40 | JS controls Flash/multimedia objects — historic heap-spray surface |

**Obfuscation & shellcode chains**

| Combination | Bonus | Why |
|---|---|---|
| `unescape()` + JavaScript | +75 | Classic Unicode-escaped shellcode decode-and-execute |
| `eval()` + JavaScript | +60 | Dynamic execution of obfuscated payload strings |
| `eval()` + `unescape()` | +85 | Textbook two-stage shellcode loader |
| `String.fromCharCode` + JavaScript | +40 | Character-level string assembly to evade pattern matching |
| `/JBIG2Decode` + JavaScript | +100 | CVE-2009-0658 exact combination confirmed (CVSS 9.3) |
| JavaScript + heapspray | +90 | JS sprays the heap before triggering a vulnerability |
| Multiple heapspray patterns (≥2) | +80 | Two or more distinct NOP/heap-fill sigs = active exploit attempt |
| Deep encoding + JavaScript | +50 | Multi-pass codec layers hiding JS from static scanners |
| Multiple `%%EOF` + JavaScript | +35 | Polyglot structure confuses parsers away from malicious JS objects |

**Metadata & structure combinations**

| Combination | Bonus | Why |
|---|---|---|
| Empty metadata + JavaScript | +35 | Stripped attribution + active scripting = crafted exploit profile |
| Empty metadata + `/EmbeddedFile` | +20 | Dropper PDFs strip metadata to avoid reputation-based triggers |
| `/OpenAction` + `/EmbeddedFile` | +45 | Auto-triggered file drop on open — no JavaScript required |
| `/AA` + JavaScript | +40 | Event-driven JS triggers on field/page interaction |
| Suspicious URL patterns | +30–60 | IP-literal, raw-port, or randomised-subdomain C2 indicators |

**Metadata & structure combinations**

| Combination | Bonus | Why |
|---|---|---|
| Empty metadata + JavaScript | +35 | Stripped attribution + active scripting = crafted exploit profile |
| Empty metadata + `/EmbeddedFile` | +20 | Dropper PDFs strip metadata to avoid reputation-based triggers |
| `/OpenAction` + `/EmbeddedFile` | +45 | Auto-triggered file drop on open — no JavaScript required |
| `/AA` + JavaScript | +40 | Event-driven JS triggers on field/page interaction |
| Suspicious URL patterns | +30–60 | IP-literal, raw-port, or randomised-subdomain C2 indicators |

**Cross-engine compound patterns (Engines ⑩–⑬ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| qpdf structural damage + JavaScript | +70 | Broken xref/trailer hides JS exploit objects from most parsers |
| qpdf structural damage + `/Launch` or `/EmbeddedFile` | +65 | Structurally concealed executable delivery payload |
| ExifTool exploit-kit fingerprint + JavaScript | +80 | Known exploit tool generated this document; active JS confirms intent |
| ExifTool exploit-kit fingerprint + `/Launch` or `/EmbeddedFile` | +75 | Exploit-kit-generated dropper PDF confirmed |
| Multiple YARA critical rules (≥2) | +60–90 | Stacked YARA critical hits confirm active malicious document (scales with count) |
| YARA heap-spray + JavaScript | +50 | Byte-level corroboration of heap-spray+JS delivery chain |
| YARA shellcode loader + auto-exec trigger | +70 | Complete exploit chain confirmed: load → execute |
| PeePDF vulnerability + JavaScript | +55–85 | Cross-engine vulnerability confirmation (scales with vuln count) |
| PeePDF vulnerability + heap-spray | +65 | Full memory-corruption exploit chain confirmed by independent parser |

**Dynamic sandbox compound patterns (Engine ⑭ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| Dynamic network beacon + JavaScript | +95 | Live connection attempt confirmed + JS delivery vector — active C2-connected exploit |
| Dynamic shellcode + heap-spray | +95 | Runtime anonymous exec memory + static heap spray — full memory-corruption chain confirmed by both static and dynamic analysis |
| Dynamic shell spawn + PDF trigger (`/AA`, `/OpenAction`, `/Launch`) | +95 | Runtime process execution + auto-trigger mechanism — confirmed exploitation with persistence hook |
| Dynamic exploitation + ExifTool exploit-kit fingerprint | +90 | Runtime behavior + known exploit-kit origin — high-confidence exploit kit delivery |
| Dynamic exploitation + PeePDF vulnerability | +90 | Two independent analysis paths (dynamic + structural) both confirm exploitation |
| Dynamic beacon + suspicious URL match | +85 | Live network calls + static C2-pattern URLs — confirmed exfiltration capability |
| Dynamic shellcode + JavaScript | +90 | Runtime exec memory + JS delivery vector — JS staging shellcode payload confirmed |
| Render timeout + JavaScript | +45 | Renderer hung >20 s + embedded JS — JS infinite-loop DoS exploit |

**Threat Intelligence compound patterns (Engine ㉑ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| TI hash confirmed malicious | +120 | Definitive hash match — the exact file is known malware |
| TI hash confirmed + JavaScript or auto-exec | +40 | Known malware + active content — confirmed exploit delivery |
| TI hash confirmed + live sandbox beaconing | +50 | Three independent engines agree: known malware + runtime C2 |

**Phishing compound patterns (Engine ㉓ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| Credential harvesting + brand impersonation | +70 | AcroForm SubmitForm + password field + impersonation keywords = classic phishing PDF |
| Credential form + suspicious submission URL | +80 | Form submits credentials to a C2 endpoint — confirmed exfiltration |
| QR code + suspicious embedded URL | +45 | QR routes victims to phishing pages while bypassing URL scanners |
| High phishing score (≥3) + JavaScript | +40 | Urgency phrases + JS may auto-submit forms or redirect the victim |

**Embedded file compound patterns (Engine ㉔ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| Embedded executable + auto-exec trigger | +100 | Complete dropper chain: trigger drops and runs the payload |
| Embedded executable + JavaScript | +85 | JS can call `exportDataObject()` to drop and execute the attachment |
| OLE attachment with VBA + JavaScript | +80 | Dual payload: JS drops the OLE file, macros execute on open |
| Embedded executable + ExifTool exploit-kit fingerprint | +90 | Professionally crafted dropper from a known attack toolkit |

**Signature forensics compound patterns (Engine ㉒ → ⑮)**

| Combination | Bonus | Why |
|---|---|---|
| ByteRange gap + JavaScript | +95 | Unsigned JS injected after signing — shadow document attack |
| Post-signature revisions with execution content | +90 | Execution vectors added after the document was signed |
| ByteRange gap + qpdf structural damage | +85 | Combined forgery + structural damage hides the payload from both validators and structure scanners |

**Dampening:** isolated `/OpenAction` with no JavaScript, no `/Launch`, no embedded files, no heapspray, no XFA, and no RichMedia has its score reduced by 7 points — `/OpenAction` alone is common in legitimate PDFs for navigation and zoom.

### Engine ⑮ — ClamAV Signature Scanner

Runs the local ClamAV daemon against the PDF and reports any signature matches:

- **Database** — 700,000+ signatures updated daily via `freshclam`, including the full `Pdf.Exploit.*` family (`Pdf.Exploit.CVE_2009_0927`, `Pdf.Exploit.CVE_2009_4324`, `Exploit.PDF-JS.*`, and many more)
- **Method** — calls `clamdscan --no-summary <path>` directly; the `clamav` user is a member of the `www-data` group so the clamd daemon reads upload files without needing `--fdpass`, reusing the in-memory signature database (~20 ms per scan)
- **Results** — exit code 0 = clean (engine recorded in `engines_run`, no indicator added); exit code 1 = match found, signature name extracted from output and reported as Critical; exit code 2 = scanner error, recorded in `structure.clamav_error`
- **Distinction** — Engines ①–⑭ use heuristics, structural analysis, and dynamic execution to catch zero-days and novel exploits; Engine ⑮ provides authoritative signature intelligence for confirmed known threats. A ClamAV match auto-labels the scan record as `malicious` in the ML training database.

### Engine ⑯ — ML Intelligence Engine

Extracts a 38-feature vector from all 15 preceding engine outputs and applies a three-layer ML scoring pipeline:

**Features extracted (38 total)**

Structural flags (`has_js`, `has_launch`, `has_openaction`, `has_embedded`, `has_xfa`, `has_richmedia`, `has_aa`, `has_uri`, `multiple_eof`, `qpdf_damaged`, `exiftool_exploit`), dynamic sandbox signals (`sandbox_network`, `sandbox_mmap_exec`, `sandbox_exec`, `sandbox_timeout`, `sandbox_fork_bomb`), YARA results (`yara_hits`, `yara_heapspray`, `yara_shellcode`, `yara_cve_pattern`), PeePDF results (`peepdf_vuln_count`), entropy metrics (`high_entropy_streams`, `high_entropy_ratio`), document stats (`stream_count`, `page_count`, `object_count`, `object_density`, `url_count`), score counters (`raw_indicator_count`, `raw_critical_count`, `raw_high_count`, `raw_score`), document attributes (`encrypted`, `linearized`, `has_metadata`, `metadata_complete`, `pdf_version`), creator classification (`creator_benign`, `creator_suspicious`).

**Bayesian contextual scoring**

Known-benign creator tools (JasperReports, LibreOffice, OpenOffice, Acrobat, Preview, PrimoPDF, Adobe Distiller, Nitro, Word) dampen the score by up to 10 points. Known-suspicious tools (Metasploit, msfvenom, Canvas, Core Impact, meterpreter) amplify by 30 points and add a Critical indicator.

**IsolationForest (unsupervised)**

Trained on all scan history with `contamination` set to the observed malicious fraction (min 1%, max 10%). Anomaly score converted to malicious probability 0–1. Model version: `if-v1`. Active from scan 1.

**RandomForest (supervised)**

Activates once ≥50 user/auto-labeled samples accumulate. 300 trees, max depth 12, `class_weight='balanced'`. Malicious probability from `predict_proba()`. Model version: `rf-v1-n{samples}`. Cross-validated AUC logged at training time.

**Auto-labeling**

High-confidence signals automatically label scan records for training: ClamAV signature match → `malicious`; dynamic sandbox behavioral score ≥80 → `malicious`; ≥2 YARA critical rule hits → `malicious`. User feedback (false-positive / confirm-threat) overrides auto-labels.

**Continuous improvement**

Training cron (`*/30 * * * * python3 /var/www/html/public/pdf/ml/train.py`) retrains both models every 30 minutes. Models saved to `/pdf/ml/models/` as `.pkl` files via `joblib`. Metadata (sample counts, contamination, CV AUC) persisted to `meta.json`.

**Explainable ML — SHAP TreeExplainer**

When the RandomForest model is active, per-sample SHAP values are computed via `shap.TreeExplainer` (Tree SHAP, polynomial time). For each scan, the SHAP value for each of the 38 features represents its individual contribution to the malicious probability prediction for that specific document — positive values push toward malicious, negative toward benign. Top 8 contributing features are returned as `ml_shap_explanation` with direction arrows and rendered as a signed bar chart in the ML panel. Falls back to model-level `feature_importances_` if SHAP is not installed. A human-readable explanation sentence is generated from the top features (e.g. "High risk: dynamic sandbox network beacon (+0.38), embedded JavaScript (+0.22), missing metadata (+0.11)").

**Result display**

The Summary tab shows a lime ML panel: malicious probability bar (0–100%), model version, contextual adjustment note, SHAP feature importance chart (top contributing features with signed bars and human-readable explanation text), and false-positive / confirm-threat feedback buttons. Feedback POSTs `scan_id` + `feedback` to `api.php?operation=pdf-scan-feedback`.

### Engine ⑱ — Differential Parsing Detection

Runs **six** independent PDF parsers against the same file and compares their structural interpretation across **8 dimensions**. A hard 30-second SIGALRM wraps the entire engine; pdfminer runs as a subprocess for guaranteed hard-kill on timeout; qpdf uses targeted fast commands only (not `--json`).

| Parser | Tools used | Data extracted |
|---|---|---|
| MuPDF | `mutool show xref` · `mutool info` · `mutool show trailer` | Object count, page count, PDF version, JavaScript, AcroForm, OpenAction, Encrypt flag |
| Poppler | `pdfinfo` · `pdfdetach -list` | Page count, PDF version, JavaScript, encryption status, form type (AcroForm/XFA/none), linearized, embedded file count, suspects flag |
| Ghostscript | `gs -sDEVICE=nullpage` | Page count (from `Page N` stdout markers), JavaScript, OpenAction, LaunchAction in stderr/stdout |
| qpdf | `--show-npages` · `--show-encryption` · `--check` | Page count, encryption status, linearized, structural integrity |
| pdfminer | `python3 -c "..."` subprocess (6 s `timeout` hard-kill) | Page count, encryption (is_extractable), OpenAction, AcroForm, JavaScript in Names tree |
| pdf.js (Node.js) | `node` inline script via subprocess | Page count, JavaScript presence, OpenAction presence, encryption status |

**Discrepancy checks and scores:**

| Check | Severity | Score | What it indicates |
|---|---|---|---|
| Page count disagreement | High | +35 | Hidden incremental update, shadow object tree, parser-confusion exploit |
| JavaScript visibility discrepancy | Critical | +50 | JS hidden behind parser-specific quirks: duplicate object IDs, broken references, malformed stream boundaries |
| Object count discrepancy >10% | Medium | +20 | Duplicate object numbers or hidden objects in compressed object streams |
| Encryption status mismatch | Critical | +40 | Parser-specific `/Encrypt` dictionary handling — encryption oracle attack indicator |
| PDF version mismatch | Medium | +15 | Conflicting version headers activate parser-specific code paths |
| AcroForm visibility discrepancy | Medium | +15 | Hidden form action trees carrying JavaScript, XFA, or submit exfiltration actions |
| Embedded file count discrepancy | High | +25 | Attachments hidden in non-standard EmbeddedFiles locations invisible to some parsers |

**Why it matters:** Attackers craft PDFs where one parser recovers hidden exploit objects, scripts, or attachments that another ignores entirely. Standard single-parser scanners miss this by design. Parser disagreement on any of the 8 dimensions is a strong indicator of deliberate evasion.

### Engine ⑲ — Polyglot / Embedded Binary Detection

Scans every PDF stream — both raw bytes and decompressed (zlib inflate, raw deflate) — for file magic byte signatures:

| Signature | Format | Risk | Score |
|---|---|---|---|
| `PK\x03\x04` | ZIP archive | Medium | +15 |
| `MZ` | Windows PE executable | Critical | +70 |
| `\x7fELF` | Linux ELF binary | Critical | +70 |
| `\xcf\xfa\xed\xfe` | Mach-O binary (x64 LE) | Critical | +70 |
| `\xfe\xed\xfa\xce` | Mach-O binary (x86) | Critical | +70 |
| `\xca\xfe\xba\xbe` | Java class file | High | +40 |
| `\xd0\xcf\x11\xe0` | OLE/CFBF (Office binary) | High | +35 |
| `%!PS-Adobe` | Embedded PostScript | Medium | +20 |
| `Rar!\x1a\x07` | RAR archive | Medium | +12 |
| `7z\xbc\xaf\x27\x1c` | 7-Zip archive | Medium | +12 |
| `<script` | Embedded HTML script | Medium | +15 |
| `<html` / `<!DOCTYPE html` | HTML/XHTML polyglot | Medium | +15 |
| `\x00asm` | WebAssembly module | High | +35 |
| Python bytecode magic | Python .pyc bytecode | Medium | +20 |
| ZIP + META-INF/MANIFEST.MF | JAR archive (Java) | High | +40 |

Polyglot files simultaneously satisfy the format rules of two or more file types. The PDF appears valid to all viewers while also containing a self-extracting archive or executable dropper that activates when saved to disk and opened by a compatible application. Used to smuggle payloads past content-type-based security controls.

### Engine ⑳ — JavaScript AST Deobfuscation

Extracts all JavaScript from the PDF (inline `/JS` literal strings and compressed streams containing `eval`, `unescape`, `String.fromCharCode`, `app.`, or `this.getField` keywords). Each fragment is passed through **Acorn** (a production JavaScript parser used by Babel and webpack) to build a full AST. The walker detects:

| Pattern | AST node | Risk | Score |
|---|---|---|---|
| `eval()` / `execScript()` / `Function()` call | `CallExpression` with matching callee | High | +30 |
| `String.fromCharCode(x, x, x, ...)` with >30 args | `CallExpression` on `String.fromCharCode` | High | +25 |
| `unescape()` call | `CallExpression` with name `unescape` | Medium | +15 |
| Array of 150+ numeric literals | `ArrayExpression` with all-numeric `Literal` elements | High | +30 |
| `new Function(string)` | `NewExpression` with callee `Function` | High | +35 |
| Property accessor obfuscation (`window["ev"+"al"]`) | `MemberExpression` with computed string-concat key | High | +30 |

**Anti-sandbox pattern detection:** The AST walker also detects environment-probing patterns used by exploit payloads to detect sandbox execution: `app.platform`, `screen.width`, `app.viewerVersion`, `navigator.*`, `Date.now()` timing calls. These are scored as medium-risk anti-analysis indicators.

**Node.js VM sandbox execution:** Multi-stage `eval(unescape(...))` chains that are statically opaque are executed in a sandboxed `vm.runInNewContext` environment with a 2-second timeout and no access to the Node.js `require` system. The decoded payload is captured and re-scanned for shellcode signatures. This resolves obfuscation that static AST analysis alone cannot decode.

**Why AST over patterns:** Text-pattern scanners look for the string `eval`. Obfuscated payloads spell it as `e\u0076al`, `window['ev'+'al']`, or build it via `String.fromCharCode`. The AST sees the *meaning* — a `CallExpression` with a callee named `eval` — regardless of how the source was written. Up to **6 deobfuscation passes** are applied per fragment to unwrap nested multi-stage eval chains (increased from 3 for deeper coverage of recursive obfuscation). Up to 6 fragments are analyzed per scan; each run is capped at 60 KB.

**Dependency:** `acorn` (npm, `/var/www/html/node_modules/acorn/`). Invoked as a Node.js subprocess.

---

### Engine ㉑ — Threat Intelligence

Queries four local PostgreSQL tables — **zero external API calls per scan**, no rate limits, sub-millisecond lookups. All databases are downloaded in bulk and kept current by `tools/update_ti_feeds.py` running on cron.

**Local databases**

| Database | Records | Cron schedule | What it detects |
|---|---|---|---|
| **URLhaus hashes** | 5,086,836 SHA-256 hashes | Weekly (Sun 3am) | Known malware payload hashes |
| **URLhaus URLs** | 70,536 malicious URLs | Every 30 min | Active malware download / C2 URLs |
| **MalwareBazaar** | 1,062,513 samples | Daily (2:30am) | Confirmed malware samples with family labels (Mirai, LummaStealer, Vidar, etc.) |
| **ThreatFox IOCs** | 176,744 IOCs | Daily (2am) | Hashes, URLs, domains, IPs with malware family attribution |

**Auto-labeling:** A confirmed hash match auto-labels the PostgreSQL scan record as `malicious` (label_source = `threat_intel`), feeding directly into ML retraining.

**Cron download script:** `tools/update_ti_feeds.py` — uses the AbuseCH API key only for bulk downloads, not per-scan calls. Each feed is a full ZIP download; records are upserted into PostgreSQL with `ON CONFLICT DO UPDATE`.

**MITRE ATT&CK tagging:** Every TI indicator is mapped to `T1588.001` (Obtain Capabilities: Malware) and `T1588.002` (Obtain Capabilities: Tool).

---

### Engine ㉒ — PDF Signature Forensics

Forensic analysis of PDF digital signatures using the **pyhanko** library.

**ByteRange coverage analysis**

A digitally signed PDF declares the byte ranges it covers in the `/ByteRange` array of the signature dictionary. If the signed byte ranges do not cover the entire file, the uncovered bytes are outside the signature's protection — any content there was appended after signing:

- Computes file length vs. total ByteRange coverage
- Gap ≥ 1 byte → `Unsigned Content Gap` indicator (High)
- Gap containing active content (`/JavaScript`, `/Launch`, `/OpenAction`, `/EmbeddedFile`) → critical shadow document attack indicator (+95 Correlation bonus)
- **Gap size ratio analysis** — computes gap size as a percentage of the total file size. A gap >20% of file size is a strong indicator that a substantial block of content exists outside the signed region, far exceeding what any legitimate annotation or form-fill update would produce. Reported as High and escalated by the Correlation Engine.
- Gap size reported in bytes to aid forensic triage

**Incremental revision diffing**

Legitimate incremental updates (annotations, form fill-ins) are expected after signing. Malicious ones add execution vectors:

- Splits the PDF at each `%%EOF` marker to isolate each incremental revision
- Diffs object inventories before and after the signature using PyMuPDF's `xref_get_key()`
- Objects of type `/Action`, `/JavaScript`, `/Launch`, `/OpenAction`, `/EmbeddedFile`, `/RichMedia` present in post-signature revisions but not before → `Post-Signature Execution Content Added` indicator (Critical)
- Revision count and per-revision object delta exported to `structure.signature_forensics`

---

### Engine ㉓ — Phishing Detection

Multi-vector phishing analysis across four detection layers.

**Urgency & deception phrase detection (30+ patterns)**

Scans extracted text for phrases statistically associated with social engineering: `login required`, `verify your account`, `suspended`, `unusual activity`, `confirm your identity`, `prize`, `winner`, `claim your reward`, `limited time`, `act now`, `expire`, `update your information`, `security alert`, `account locked`, `verify now`, and others. Each match increments a phishing score; score ≥3 reported as High.

**Brand impersonation keywords**

Matches brand names associated with phishing campaigns against extracted text and metadata: Microsoft, Office 365, OneDrive, Apple, iCloud, Amazon, PayPal, DocuSign, Adobe, DHL, FedEx, UPS, IRS, HMRC, Netflix, LinkedIn, Dropbox, Google, Chase, Bank of America, and others.

**AcroForm credential harvesting**

Structural analysis of PDF forms for the credential-exfiltration pattern:
- `/SubmitForm` action present in AcroForm → flags the submission endpoint URL
- Password-type field (`/FT /Tx` + `/TU` containing "password" / "pwd" / "pass" / "pin") → `Credential Harvesting Form` indicator (High)
- Both present together + suspicious submission URL → Correlation Engine adds +80 bonus

**QR code extraction and decoding**

- Renders each PDF page to a PNG image via PyMuPDF at 150 DPI
- Runs `zbarimg --raw` to decode all embedded QR codes
- Decoded URLs checked for suspicious patterns: raw IP addresses, non-HTTPS schemes, URL shortener domains (`bit.ly`, `tinyurl.com`, `t.co`, etc.), domains registered <90 days (if whois available)
- Phishing QR code reported as High; combined with suspicious embedded URLs → Correlation Engine adds +45 bonus

**Dependency:** `zbarimg` (`zbar-tools` apt package, `/usr/bin/zbarimg`).

---

### Engine ㉔ — Embedded File Analysis

Forensic analysis of every embedded file attachment extracted from the PDF.

**Extraction**

Uses `pdfdetach -saveall` (Poppler) to extract all `/EmbeddedFile` stream attachments to a temporary directory. Falls back to PyMuPDF `doc.embfile_get()` for each `doc.embfile_count()` attachment if pdfdetach fails.

**Magic byte classification**

| Magic bytes | Format | Risk | Score |
|---|---|---|---|
| `MZ` | Windows PE executable | Critical | +80 |
| `\x7fELF` | Linux ELF binary | Critical | +80 |
| `\xd0\xcf\x11\xe0` | OLE/CFBF (Office binary) | High | +45 |
| `PK\x03\x04` + OOXML signature | Office Open XML (docx/xlsx/pptx) | Medium | +25 |
| `.bat`, `.ps1`, `.vbs`, `.cmd`, `.sh`, `.py` extension | Script file | High | +50 |
| `Rar!\x1a\x07` / `7z\xbc\xaf` | Compressed archive | Medium | +15 |

**VBA macro detection**

For OOXML attachments (ZIP containers): inspects entries `xl/vbaProject.bin`, `word/vbaProject.bin`, and `ppt/vbaProject.bin`. Presence of any `vbaProject.bin` entry confirms VBA macros — reported as High regardless of attachment type.

**Non-OOXML ZIP content listing**

For ZIP archives that are not Office OOXML containers, the full ZIP entry list is extracted (up to 50 entries) and included in the scan results for the Embedded tab. The engine scans every entry for dangerous extensions (`.exe`, `.dll`, `.sys`, `.bat`, `.cmd`, `.ps1`, `.vbs`, `.js`, `.jse`, `.wsf`, `.hta`, `.scr`, `.pif`) — any match is reported as Critical with the full list of dangerous filenames. Even benign-looking ZIPs are listed so analysts can review unexpected contents without manual extraction.

**Nested PDF detection**

If an embedded file attachment begins with the `%PDF` magic bytes, it is flagged as a nested PDF document. Nested PDFs are processed independently by the outer PDF reader and can contain self-contained malicious payloads (JavaScript, launch actions, heap-spray shellcode) that bypass defences applied only to the outer document. Full recursive analysis of nested PDFs is logged for manual triage.

**Strings extraction**

For PE/ELF executables: extracts printable ASCII strings ≥8 chars (`strings`-style scan of raw bytes). Surfaces suspicious API names (`CreateRemoteThread`, `VirtualAlloc`, `WinExec`, `ShellExecute`, `DownloadFile`), IP address literals, and command-line arguments to aid manual triage.

**PowerShell .ps1 content analysis**

For embedded `.ps1` script attachments, the raw script content is regex-scanned for high-risk PowerShell execution patterns: `Invoke-Expression` / `IEX`, `DownloadString` / `WebClient`, `bypass ExecutionPolicy`, and `-EncodedCommand` / `-enc` with a base64 payload. Any match is reported as Critical — embedded PowerShell scripts with download-and-execute patterns are a primary living-off-the-land dropper technique, and their presence in a PDF attachment is unambiguously malicious.

**Correlation:** A PDF carrying a PE executable is treated as a confirmed dropper regardless of other indicators — the Correlation Engine adds +100 when combined with any auto-execute trigger.

---

### Engine ㉕ — Campaign Attribution

Fuzzy-hash similarity matching against the confirmed-malicious scan history.

**TLSH (Trend Locality Sensitive Hash)**

TLSH is a locality-sensitive hash: similar files produce similar hashes, unlike SHA-256 where a 1-bit change produces a completely different hash. It is designed specifically for malware similarity clustering.

- Computed on the full PDF byte content using the `python3-tlsh` library
- TLSH score 0 = identical; <30 = near-identical (same exploit kit generation / same campaign tooling); <100 = same malware campaign family; ≥100 = distinct samples
- Stored in `pdf_scan_history.tlsh_hash` (indexed via `idx_psh_tlsh_hash`)

**Database comparison**

After computing the TLSH hash, Engine ㉕ queries the 500 most recently confirmed-malicious PDFs from PostgreSQL:

```sql
SELECT id, sha256, tlsh_hash, label
FROM pdf_scan_history
WHERE label = 'malicious'
  AND tlsh_hash IS NOT NULL
  AND tlsh_hash NOT IN ('', 'TNULL', 'T1')
ORDER BY scan_at DESC
LIMIT 500
```

Any match with score <100 is reported as a `Campaign Attribution Match` indicator. The matched scan's ID is included for cross-reference.

**Structural fingerprint fallback**

For PDFs too small for reliable TLSH (< 512 bytes), Engine ㉕ falls back to a structural fingerprint: object count, stream count, action type set, and encrypted flag. The fingerprint is stored as JSON in `structure.campaign_attribution.structural_fingerprint`.

**MITRE ATT&CK:** Campaign matches are tagged `T1583` (Acquire Infrastructure) and `T1587.001` (Develop Capabilities: Malware).

---

### MITRE ATT&CK Mapping

Every indicator produced by all 44 engines is tagged with one or more MITRE ATT&CK technique IDs via a 55-entry lookup table keyed on indicator name substrings:

| Technique ID | Name | Triggered by |
|---|---|---|
| T1059.007 | Command and Scripting: JavaScript | JavaScript indicators, eval chains, AST findings |
| T1203 | Exploitation for Client Execution | Heapspray, shellcode, CVE patterns, large numeric arrays |
| T1027 | Obfuscated Files or Information | unescape(), fromCharCode, dynamic eval, new Function() |
| T1055 | Process Injection | Anonymous exec memory (sandbox), shellcode execution |
| T1071 | Application Layer Protocol | Network beacon, suspicious URLs |
| T1041 | Exfiltration Over C2 Channel | Network beacon + URL match |
| T1566.001 | Phishing: Spearphishing Attachment | Phishing indicators |
| T1204.002 | User Execution: Malicious File | Launch actions, dropper patterns |
| T1588.001 | Obtain Capabilities: Malware | TI hash confirmed |
| T1497 | Virtualization/Sandbox Evasion | Anti-sandbox patterns, timing evasion |
| T1547 | Boot or Logon Autostart | OpenAction + Launch combinations |
| T1553.003 | Subvert Trust Controls: SIP Hijacking | Signature forgery / ByteRange gap |
| T1036 | Masquerading | Metadata stripping, polyglot files |
| … | (45 more entries across all engines) | — |

The full `mitre_techniques` array is included in every scan result JSON for SIEM/SOAR integration.

### Forensic Console

Before and during scanning, a live event log panel ("Forensic Console") streams timestamped events to the browser. It sits above the results area and auto-scrolls. Appearance is terminal-style: macOS traffic-light dots, monospace font, dark background.

Each log line has three parts:

| Part | Format | Example |
|---|---|---|
| Timestamp | `HH:MM:SS.mmm` | `21:47:52.089` |
| Badge | Colour-coded label | `UPLOAD` · `INFO` · `START` · `DONE` · `WARN` · `ERROR` · `Clean` |
| Message | Human-readable description | `Upload complete (0.11s) — token: pdftool_…` |

Section dividers (`── Upload ──`, `── Engines ──`, `── Results ──`) separate phases. The console can be collapsed/expanded and cleared with header buttons. When all 44 engines complete, a `Clean` or risk-level badge appears alongside the final elapsed time.

### Result Banner and Risk Levels

The top of the Summary tab shows a full-width risk banner. Five levels, colour-coded:

| Level | Icon | Score range | Banner class |
|---|---|---|---|
| Clean | ✅ | 0 | Green |
| Low Risk | 🟡 | 1–99 | Yellow |
| Suspicious | 🟠 | 100–299 | Orange |
| High Risk | ⚠️ | 300–599 | Red |
| Dangerous | 🔴 | 600–999 | Dark red |

Below the banner title a score meter bar fills 0–999 left-to-right in a matching colour. The label shows `Risk Score: X / 999`.

### Statistics Grid

The 15-cell stats grid below the banner summarises key structural fields at a glance. Three cells are interactive — clicking them jumps directly to the corresponding tab:

| Cell | Clickable | Alert (red) when |
|---|---|---|
| Pages | — | — |
| Objects | — | — |
| File Size | — | — |
| PDF Version | — | — |
| Encrypted | — | Always (if true) |
| Embedded Files | ✓ → Embedded tab | > 0 |
| Form Fields | — | — |
| Annotations | — | — |
| Links | — | — |
| %%EOF Markers | — | > 2 |
| XRef Tables | — | > 3 |
| Total Streams | ✓ → Streams tab | — |
| High-Entropy Streams | — | > 0 |
| URLs Found | ✓ → URLs tab | > 0 |
| Threats Found | ✓ → Threats tab | > 0 |

All cells have `data-tip` tooltips on hover explaining what the field measures.

### Report Tabs

The tab bar uses a pill-style design with background highlighting on hover and an amber-tinted active state. Dynamic badges on several tabs update live during the scan (threat count, ML %, MITRE technique count, etc.).

| Tab | Badge | Contents |
|---|---|---|
| **📊 Summary** | — | Risk banner + score meter, 15-cell stats grid (3 clickable), engines-completed pill strip (✓ {engine name} for all 30 that ran), ML Intelligence panel (probability bar + SHAP feature bars + false-positive / confirm-threat feedback buttons if scan_id available) |
| **⚠️ Threats** | indicator count | All indicators grouped Critical → High → Medium → Low. Each card shows: risk badge, engine label, count pill, indicator key, description, byte-context snippet |
| **📈 Score** | — | Score gauge (large number + bar, 0–999), per-engine contribution bars (points breakdown), full "every indicator scored" table: engine / indicator / risk / base pts / count multiplier / total pts |
| **⚙️ Engines** | — | Two-panel browser: left sidebar lists all 44 engines (status dot: green=clean · orange=findings · grey=skipped, findings count pill). Click any engine → right panel shows full indicator cards, engine-specific data (stream table for ③, URL list for ⑤, SHAP bars for ⑯, differential table for ⑰, certificate chain for ㉑, correlation bonuses + **Per-Engine Indicator Counts** table + **Final Risk Assessment** KV for ㉛), and all structure fields |
| **🌐 URLs** | URL count | All unique HTTP/HTTPS URLs from raw bytes + decompressed streams, per-URL copy button |
| **📦 Streams** | suspicious count | Top 40 streams table: XRef # · type (with tooltip) · decompressed size · Shannon entropy bar (red if > 7.2) · status (OK / High Entropy / Patterns Found) · matched patterns. Suspicious rows highlighted amber, high-entropy rows orange. |
| **🧠 ML** | malicious % | LightGBM + IsolationForest + RandomForest ensemble: malicious probability bar (colour-coded by risk), model version, context adjustment note, SHAP bar chart (per-feature directional contribution, red=malicious / green=benign), feature importance bars, false-positive / confirm-threat feedback buttons (trains next model update) |
| **🔬 Sandbox** | behavioural score | 7-cell metrics grid (Behavioral Score · Network Attempts · Exec Attempts · Process Forks · FS Escape Attempts · Anon Exec Memory · Timeout/Hang — cells turn red at critical thresholds), renderer list, sandbox threat indicators, matched YARA rules |
| **🌍 Threat Intel** | `!` if malicious | Confirmed-malware banner (if SHA-256 matches), SHA-256 display, per-database results (URLhaus · MalwareBazaar · ThreatFox · FeodoTracker · OpenPhish), domain-level TI matches (source badge + URL + type tags), campaign attribution details (TLSH · pHash · JS fingerprint), similar malicious sample list with similarity % |
| **🎯 MITRE** | technique count | All ATT&CK technique IDs mapped from indicators — ID + name + tactic, grouped by tactic. Indicator rows per technique. |
| **🔬 Parsing** | — | Differential Parsing Detection: 6 parser cards (MuPDF · Poppler · Ghostscript · qpdf · pdfminer · pdf.js), each showing pages, objects, PDF version, JS, encryption, AcroForm, embedded files, linearised, OpenAction, annotations, structural integrity. Mismatch badges (Critical / High / Medium) flag parser-evasion discrepancies. |
| **🧬 Polyglot** | — | Magic-byte hits (type + risk badge) from Engine ⑱, plus JS AST deobfuscation findings (dynamic eval · fromCharCode arrays · unescape calls · large numeric arrays · `new Function`) from Engine ⑲ |
| **🎣 Phishing** | signal score | Phishing signal score meter, credential harvesting detection, urgency phrase tags, brand keyword tags (Microsoft · PayPal · Apple · etc.), QR code decodes, OCR-extracted text from images, phishing indicator cards |
| **📎 Embedded** | file count | Per-file cards: name · magic-byte type (PE/ELF/OLE/ZIP/script) · size · VBA macro detection · ZIP content listing (50 entries, dangerous-extension flags) · dangerous PE import list · suspicious string list · nested PDF detection |
| **✍️ Signature** | — | Signature status card (count or "None"), ByteRange coverage gap (shadow-document indicator), post-signing revision diff (objects added after signing), unsigned JS/launch actions flag, per-certificate cards (subject · issuer · valid dates · algorithm · self-signed · expired) |
| **🏷️ Metadata** | — | Document metadata KV table (title · author · subject · keywords · creator · producer · dates · XMP flag) + structure info KV table (version · EOF markers · xref tables · linearised · binary comment · stream counts) + full 44-engine structure dump |
| **📋 Raw JSON** | — | Complete scan result JSON, syntax-highlighted (strings · keys · booleans · nulls · numbers), one-click copy button |
| **🔍 Raw Forensics** | — | JavaScript source code from streams · JS AST deobfuscation contexts · decoded stream content (3 KB preview per stream) · every indicator context snippet · complete sorted KV dump of all structure fields from all 44 engines |

### Sanitize Options

After every scan (including clean results), a 9-mode sanitize panel appears below the result. The session token links the sanitize request to the uploaded file; the original is never modified — all operations produce a new file for download. After sanitization completes a **Download Sanitized PDF** button and a **Scan the Sanitized File** button appear (re-runs the full 44-engine scan on the cleaned output).

**Basic**

| Method | How | Safety |
|---|---|---|
| **Flatten to Images** | Renders every page to 144 DPI raster images via PyMuPDF, rebuilds as a new PDF | Maximum — destroys all JavaScript, launch actions, embedded files, XFA forms, rich media, and object streams with absolute certainty. Text becomes non-searchable. |
| **Strip Active Content** | Re-processes through Ghostscript with `-dSAFER` | Moderate — removes JavaScript, launch actions, embedded files, and rich media. Text is usually retained but not guaranteed. Cannot guarantee removal of zero-day or heavily obfuscated exploit structures. |

**Advanced — Surgical Cleaning**

| Method | How | Preserves |
|---|---|---|
| **Remove JavaScript** | Nullifies all `/JS`, `/JavaScript`, `/AA` entries and OpenAction JS refs via PyMuPDF object walk | Layout, searchable text, static AcroForms, embedded files |
| **Remove Embedded Files** | Strips all `/EmbeddedFile` attachments via `embfile_del` + object walk | All visible page content |
| **Remove XFA Forms** | Removes `/XFA` XML form definitions via object walk | Static AcroForms, page content |
| **Remove Rich Media** | Nullifies `/RichMedia`, `/Movie`, `/Sound` objects | Text, images, static content |
| **Normalize Structure** | Rebuilds via `qpdf --object-streams=disable --decode-level=all` — collapses incremental updates, decodes all filter chains | All page content |
| **Flatten Forms** | Renders AcroForm widgets into static page content via PyMuPDF `bake()` | Rendered field values as static text |
| **Strip Metadata** | Clears `/Info` dictionary and XMP stream via `set_metadata({})` + `del_xml_metadata()` | All page content |

---

## Edit PDF — 16 Annotation Tools + Form Builder + Bookmark Editor

[/tools/edit.php](https://pqpdf.com/tools/edit.php)

The editor renders each page to a canvas and applies all changes server-side via PyMuPDF.

| Tool | Description |
|---|---|
| **Text** | Click to place a text box. Font family (Helvetica / Times / Courier), font size, bold, italic, left/centre/right alignment, colour. |
| **Freehand Draw** | Freehand pen with configurable line width (slider with live preview) and colour. |
| **Eraser** | Freehand white-stroke eraser — paints white over any content. Line width scales with the width slider (2× multiplier, minimum 10 pt). Applied as a white draw path via PyMuPDF on export. |
| **Line** | Straight line with stroke width and colour. |
| **Arrow** | Directional arrow annotation. |
| **Rectangle** | Rectangle with fill/outline toggle, **independent fill colour** (separate from stroke colour), fill opacity, stroke width, and stroke colour. |
| **Ellipse** | Ellipse with fill/outline toggle, **independent fill colour** (separate from stroke colour), fill opacity, stroke width, and stroke colour. |
| **Highlight** | Translucent highlight overlay with configurable opacity. |
| **Whiteout** | Solid white box to cover content. |
| **Strikethrough** | Strikethrough line over selected text area. |
| **Underline** | Underline over selected text area. |
| **Image** | Upload and position an image on the page. |
| **Signature** | Open a signature canvas modal (draw with mouse or touch), place result on page. |
| **QR Code** | Generate a QR code from any URL or text string (via `edit-qr-generate` API), set size, and place on page. |
| **Stamp** | Insert one of 12 built-in stamps (DRAFT, APPROVED, REJECTED, CONFIDENTIAL, TOP SECRET, VOID, COPY, FINAL, REVISED, REVIEW, NOT APPROVED, PAID) or type a custom stamp text. |
| **Sticky Note** | Click to place a sticky note annotation at any point on the page. Enter text in the modal, pick a note colour (yellow default). Rendered as a folded-corner icon on the canvas; written as a native `page.add_text_annot()` PDF text annotation with a popup comment on export — visible in Acrobat and all standards-compliant viewers. **Right-click** any sticky note icon to Edit (reopens modal pre-filled) or Delete (records an undo history entry). |
| **Form Field** | Draw any interactive AcroForm widget onto the page — see Form Builder section below. |

### Form Builder

The Form Field tool creates native, interactive AcroForm widgets embedded in the PDF using PyMuPDF's `page.add_widget()`. Click the **Form Field** toolbar button, pick a widget type from the popover, drag a rectangle on the canvas to define the field area, then set properties in the modal. The field appears as a dashed blue rectangle (with type:name label) while editing, and is written as a real interactive field on Apply & Download.

All 7 PyMuPDF / PDF specification widget types are supported:

| Widget Type | Properties |
|---|---|
| **Text** | Field name, default value, multiline, max length, password mode, tooltip, font size, text colour, required, read-only |
| **CheckBox** | Field name, checked-by-default state, tooltip, font size, text colour, required, read-only |
| **RadioButton** | Field name (used as group name), option value (string stored when selected), tooltip, font size, text colour, required, read-only |
| **ListBox** | Field name, choices (one per line), default selection, multi-select, tooltip, font size, text colour, required, read-only |
| **ComboBox** | Field name, choices (one per line), default selection, editable (allow free text entry), tooltip, font size, text colour, required, read-only |
| **Signature** | Field name, tooltip, required, read-only |
| **PushButton** | Field name, button caption text, tooltip, required, read-only |

**Implementation notes:**
- Widget type constants resolved via `getattr(fitz, 'PDF_WIDGET_TYPE_*', fallback_int)` for cross-version compatibility
- Field flags (`PDF_FIELD_IS_*`) applied bitwise: `READ_ONLY`, `REQUIRED`, `MULTILINE`, `PASSWORD`, `MULTISELECT`, `EDIT`
- Text colour set as RGB tuple `(r, g, b)` normalised to 0–1 range; font size via `widget.text_fontsize`
- Choices for ListBox/ComboBox set via `widget.choice_values = [...]`
- Fields are stored in the annotations array as `{type: 'form_field', field_type: '...', name: '...', x, y, w, h, ...props}` per page
- All `form_field` annotation objects survive serialisation through the existing `edit-apply` JSON payload (no special handling needed in the JS serialiser)

### Additional Edit Features

- **Bookmark editor** — open the Bookmarks panel to build a navigable table of contents: add bookmark entries by title and page number (defaulting to the current page), delete with the ✕ button, or **right-click any row** to Edit (pre-fills the form for in-place update) or Delete. On Apply & Download, bookmarks are written to the PDF via `out_doc.set_toc()`, creating a native PDF outline visible in all viewers that support bookmarks (Acrobat, Chrome, Firefox, Preview, etc.)
- **Page numbering** — auto-add page numbers with position (top/bottom/left/right), format (1 / Page 1 / 1 of 10), start number, font size, and colour
- **Headers & footers** — insert header and footer text with alignment, font size, and colour
- **Insert blank page** — add a blank page before or after the current page; also supports creating a new blank PDF from scratch
- **Duplicate page** — copy the current page (image, annotations, and rotation) and insert it immediately after
- **Page rotation** — per-page rotation (toolbar buttons for current page, or right-click any thumbnail for any page)
- **Undo / redo** — per-page history stack
- **Zoom controls** — adjustable canvas zoom
- **Stroke style** — solid, dashed, dotted, or dash-dot per annotation
- **Annotation opacity** — global opacity slider applied to highlights and filled shapes
- **Preferences persistence** — last-used tool, colour, font family, font size, line width, fill mode, and zoom are saved in a cookie
- **Session timer** — a floating badge (bottom-right) counts down from 30 minutes of inactivity. Any canvas interaction, tool change, or keypress resets the timer. The badge turns amber at 5 minutes remaining and red at 2 minutes. A keepalive ping is sent to the server every 5 minutes of activity so the server-side session TTL stays in sync. After 30 minutes of inactivity the session expires and a fresh upload is required.

### Page Thumbnail Sidebar

Pages are displayed as draggable thumbnails in the left sidebar:

- **Drag to reorder** — drag any thumbnail to a new position; all annotation and rotation state moves with it
- **Click to navigate** — click any thumbnail to jump to that page
- **Right-click context menu** — right-click any thumbnail to access per-page operations without navigating away:

| Action | Description |
|---|---|
| Go to Page N | Navigate to that page |
| Rotate 90° Clockwise | Rotate that specific page CW |
| Rotate 90° Counter-clockwise | Rotate that specific page CCW |
| Rotate 180° | Flip that page upside down |
| Duplicate Page | Copy page + annotations, insert after |
| Insert Blank Before | Insert empty page before this page |
| Insert Blank After | Insert empty page after this page |
| Move to First | Move this page to position 1 |
| Move to Last | Move this page to the end |
| Delete Page | Remove this page (disabled on single-page docs) |

### Page Navigation Toolbar

| Button | Action |
|---|---|
| ⏮ First | Jump to page 1 |
| ◀ Prev | Previous page (keyboard: ←) |
| Page counter | Current / total display |
| ▶ Next | Next page (keyboard: →) |
| ⏭ Last | Jump to last page |

---

## Fill PDF Form

[/tools/fill.php](https://pqpdf.com/tools/fill.php)

Detects all interactive AcroForm fields in an uploaded PDF and presents them as a fill-in form in the browser. Supports every standard field type — text inputs, checkboxes, radio buttons, drop-down menus (ComboBox), and list boxes. Values are written back server-side via PyMuPDF and the filled PDF is returned immediately. An optional **Flatten after filling** mode bakes field values into static page content so the document can no longer be edited — useful for archiving, printing, or sharing final versions.

### How it works

1. Upload a PDF containing AcroForm fields — the server runs a PyMuPDF extraction script that reads every widget's `field_type_string`, current value, choices, `on_state`, page index, and bounding rect.
2. Fields are grouped by page and rendered in the browser as a native HTML form: `<input type="text">` for Text fields, `<input type="checkbox">` for CheckBox, `<input type="radio">` for RadioButton (grouped by field name), `<select>` for ComboBox and ListBox.
3. After filling in values, clicking **Fill & Download** sends the token, field values JSON, and flatten flag to the server. PyMuPDF opens the original file, iterates all widgets, writes back each value, optionally calls `doc.bake()` to flatten, and streams the result as `application/pdf`.
4. The download blob is created via `URL.createObjectURL()` — no server storage of the filled file.

### Field type support

| Field Type | Rendered as | Notes |
|---|---|---|
| Text | `<input type="text">` | Pre-filled with existing value |
| CheckBox | `<input type="checkbox">` | Checked when value matches `on_state` |
| RadioButton | `<input type="radio">` | All options with same name grouped; uses `on_state` for option values |
| ComboBox | `<select>` | Populated from `choice_values`; current value pre-selected |
| ListBox | `<select>` | Same as ComboBox |
| Signature | (skipped) | Signature fields are not fillable via text |
| PushButton | (skipped) | Action buttons have no fillable value |

### Flatten mode

When **Flatten after filling** is checked, the server calls `doc.bake()` after writing field values. This merges all widget appearances into the page content stream, removing interactivity. The resulting PDF renders identically in all viewers but cannot be re-filled or re-edited through a form interface.

---

## Protect PDF — Dual Encryption Modes

[/tools/protect.php](https://pqpdf.com/tools/protect.php)

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

[/tools/sign.php](https://pqpdf.com/tools/sign.php)

| Method | Description |
|---|---|
| **Draw** | Freehand signature on a canvas. Full touch support for mobile. Clear and redraw. |
| **Type** | Typed name rendered as a signature image via ImageMagick. |
| **Upload** | Upload a pre-drawn signature image. |

**Placement controls:** page (first / last / custom page number), horizontal position, vertical position, signature size (points).

**Live placement preview:** after drawing, typing, or uploading a signature, a preview canvas renders page 1 of the uploaded PDF with the signature composited at the chosen position and size. A dashed amber border highlights the signature bounding box. The preview updates in real time as you adjust position, size, switch tabs, or modify the signature — no server round-trip required.

**Cryptographic metadata (optional, expandable):** signer name, email, reason, location, date stamp. Supports auto-generated or custom certificate (`cert_source: auto | own`, `cert_file`, `cert_password`).

---

## Flatten PDF — Content Scan

[/tools/flatten.php](https://pqpdf.com/tools/flatten.php)

On upload, PDF.js scans the document client-side before submission:

- **Form fields** — counted via `getFieldObjects()`
- **Annotations** — non-link, non-widget annotations counted per page via `getAnnotations()`
- **Layers** — optional content groups counted via `getOptionalContentConfig()`

A scan card appears immediately with a human-readable summary and amber badges per element type:

> *3 form fields · 2 annotations · 1 layer — all will be made permanent*

If no interactive elements are found, a green **Already flat** badge is shown. If the file is encrypted or unreadable, a red **Scan failed** badge appears — flattening can still proceed.

---

## Repair PDF — Corruption Diagnostics

[/tools/repair.php](https://pqpdf.com/tools/repair.php)

On upload, PDF.js performs a client-side diagnostic pass before the file is sent to the server:

- Verifies the `%PDF-` header is present
- Attempts `getDocument()` — catches and classifies xref table errors, stream corruption, truncation, and encryption
- Renders pages 1–3 to catch per-page content stream errors

A diagnostic card shows the results:

- **No issues found** — green badge with page count (e.g. "12 pages readable")
- **Issues detected** — one red badge per problem (e.g. "⚠️ Cross-reference table error detected", "⚠️ Page 2: render error")

The Repair tool will attempt server-side recovery regardless of the scan result.

---

## Unlock PDF — Encryption Detection

[/tools/unlock.php](https://pqpdf.com/tools/unlock.php)

When a `.pdf` file is selected, the first 4 KB is read client-side and checked for the `/Encrypt` dictionary marker:

| Result | Badge |
|---|---|
| Password-protected | 🔒 `AES-256 encrypted — password required` (amber) |
| Not encrypted | ✅ `No password protection detected` (green) |

PQC bundles (`.pqcpdf`) are auto-detected by file extension and routed directly to the quantum-safe decryption panel — the encryption badge is not shown for these files as the bundle already contains full algorithm and key metadata.

---

## PDF → Images — Live DPI Preview

[/tools/to-images.php](https://pqpdf.com/tools/to-images.php)

After upload, page 1 is rendered via PDF.js at the currently selected DPI. A preview canvas appears between the DPI selector and the pages options, showing the actual output resolution before any server processing:

- Renders at the selected DPI scale (`dpi / 72` × page viewport), capped to 360 px wide for display
- A hint line shows exact pixel dimensions **and estimated file size per page**: e.g. `1240 × 1754 px per page — ~1.5 MB per page`
- File size is estimated from pixel count × bytes/pixel: PNG uses ~0.7 bytes/pixel (lossless document content estimate); JPEG scales with the quality slider — `0.04 + (quality/100) × 0.11` bytes/pixel so a quality-85 JPEG at 1240 × 1754 px estimates ~0.16 MB/page
- Size estimate updates immediately on file load (triggered by `updateEstimate()` after the DPI preview renders) and re-runs whenever DPI, format, or quality changes
- Re-renders automatically when you switch DPI — no submit required
- Shows `⚠️ Very large files — processing may take longer` inline at 300 DPI

---

## Split PDF — Four Split Modes

[/tools/split.php](https://pqpdf.com/tools/split.php)

| Mode | Parameter | Description |
|---|---|---|
| All pages | `split_mode=all` | One file per page |
| Custom ranges | `split_mode=range` + `page_ranges` | Comma-separated ranges e.g. `1-3,5,7-9` |
| Interval | `split_mode=interval` + `interval` | Every N pages |
| Cut points | `split_mode=custom` + `split_after` | Click scissors (✂) between page thumbnails in the preview to set split points |

---

## Watermark — Position Options

[/tools/watermark.php](https://pqpdf.com/tools/watermark.php)

`position`: `diagonal` | `center` | `top-left` | `top-right` | `bottom-left` | `bottom-right`

`pages`: `all` | `odd` | `even` | `range` (+ `page_range`)

Additional parameter: `font_style`

---

## Redact PDF — Two Modes

[/tools/redact.php](https://pqpdf.com/tools/redact.php)

**Text pattern mode:**
- Add multiple patterns to a list; each can be removed individually
- `case_sensitive` toggle
- `whole_word` toggle
- `fill_color` — hex colour for redaction boxes

**Region drawing mode:**
- Draw rectangles directly on a canvas page preview to mark redaction areas

---

## OCR PDF — Tesseract 5 LSTM

[/tools/ocr.php](https://pqpdf.com/tools/ocr.php)

Converts scanned and image-based PDFs to machine-readable text using Tesseract 5's LSTM neural network engine.

### How It Works

1. **Page rasterisation** — `pdftoppm` converts each PDF page to a PPM image at the selected DPI. Pages are processed one at a time to prevent disk exhaustion on large files; the PPM for a given page is deleted before the next page starts.
2. **LSTM OCR** — `tesseract` processes the PPM with the `eng` LSTM model. The PSM (page segmentation mode) is passed as `--psm {psm}`. Output format flags (`txt`, `pdf`, `tsv`, or all three) are passed in a single invocation per page — no page is OCR'd twice.
3. **Confidence scoring** — Tesseract's TSV output (column 10) contains per-word confidence values (0–100) and structural-element markers (−1). The handler filters out −1 values and averages the remaining scores across all pages to produce an overall confidence percentage.
4. **Searchable PDF assembly** — When format includes `pdf`, tesseract produces one PDF per page (original image + invisible text overlay). If the job covers more than one page, `pdfunite` concatenates the per-page PDFs into a single searchable document. Single-page jobs skip `pdfunite` and use a direct `rename()`.
5. **Output packaging** — `txt` output is sent as a `.txt` file; `pdf` output as a `.pdf`; `both` output as a `.zip` containing both files.
6. **Stats header** — The API sets `X-OCR-Stats` (JSON: `pages_processed`, `total_pages`, `word_count`, `char_count`, `avg_confidence`, `dpi`, `format`) and `Access-Control-Expose-Headers: X-OCR-Stats` so the browser JS can read it from the XHR response without inspecting the body.
7. **Result UI** — After completion the result panel renders in column order: (a) a header row with ✅ icon, title, message, and action buttons; (b) a stats bar with 5 stat tiles (pages, words, characters, confidence %, DPI) plus a colour-coded confidence bar (green ≥80%, amber ≥55%, red below); (c) an inline preview panel toggled by a **👁️ Preview Text** button in the actions row — no separate tab bar. Progress during the server phase uses JS `setInterval` to advance the bar and cycle through 8 phase messages every 2.4 s; the `pdf-progress-bar-busy` CSS class adds marching diagonal stripes via `::after`. The progress element carries `data-no-overlay` to suppress the global `pdf-processing.js` MutationObserver overlay (which would otherwise cover the in-page progress bar).
8. **Timeout** — XHR timeout is calculated dynamically as `max(3 min, pages × secPerPage × 3)`: 150 DPI uses 3 s/page, 200 DPI uses 5 s/page, 300 DPI uses 8 s/page. For a 75-page job at 200 DPI this yields an 18.8-minute client timeout. PHP's execution cap is lifted with `set_time_limit(0)` at the start of `op_ocr()`. Per-command shell timeouts also scale with DPI: `pdftoppm` gets 60/90/120 s and `tesseract` gets 90/120/180 s for 150/200/300 DPI respectively.

### Options

| Option | Values | Default |
|---|---|---|
| DPI | 150 / 200 / 300 | 200 |
| PSM | 3 (auto), 4 (single column), 6 (single block), 11 (sparse text) | 3 |
| Output format | txt / pdf / both (ZIP) | txt |
| Pages | all / custom range | all |
| Max pages | 100 (hard cap) | — |

### Performance

Derived from the time estimate logic in `ocr.js` (`timeEstimate` function):

| DPI | Approximate time per page |
|---|---|
| 150 | ~3 seconds |
| 200 | ~5 seconds |
| 300 | ~8 seconds |

A 20-page document at 200 DPI takes approximately 100 seconds. The UI shows a live time estimate badge for documents over 15 pages and cycles through 8 OCR-phase status messages while the server processes.

### Accuracy Notes

- Best results on clean, high-contrast scans (printed text, typed forms).
- **300 DPI recommended** for small fonts or handwriting — higher resolution gives the LSTM model more detail.
- **PSM 11 (sparse text)** works best for documents with isolated text blocks: forms, receipts, labels.
- **PSM 4 (single column)** suits newspaper-style layouts.
- The client-side JS analyses the PDF with PDF.js before upload and warns if a text layer already exists (OCR is unnecessary for native-digital PDFs).
- Only the `eng` language model is installed (`/usr/share/tesseract-ocr/5/tessdata/eng.traineddata`). Multi-language documents produce lower confidence scores.

### Privacy

- The PPM image for each page is deleted immediately after that page is OCR'd — only one page image exists on disk at any time.
- No text content, file content, or OCR output is written to any database or log.
- The temp directory (`pdftool_{24-hex}`) is deleted by `send_file()` → `cleanup()` while the download is streaming.

---

## Rotate PDF — Page Selection

[/tools/rotate.php](https://pqpdf.com/tools/rotate.php)

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
| PDF engines | Ghostscript, Poppler, qpdf, LibreOffice, PyMuPDF, ImageMagick, Playwright/Chromium |
| Threat scanning | PyMuPDF (heuristic engines ①–⑨), ExifTool 12 (⑩), qpdf 11.9 (⑪), YARA 4.5 (⑫, 21+ rules), PeePDF 0.4 + pikepdf (⑬), strace 6.8 + unshare (⑭ sandbox), ClamAV 1.4+ (⑮), scikit-learn + LightGBM + SHAP (⑯), Node.js pdf.js (⑰), imagehash/pHash (⑱ polyglot), acorn (⑲ AST), Correlation (㉕), local TI databases: URLhaus 5M+ hashes, MalwareBazaar 1M+ samples, ThreatFox 176K+ IOCs, FeodoTracker C2s, OpenPhish domains (⑳) |
| Process sandbox | prlimit (resource caps) + AppArmor aa-exec (MAC) + unshare (Linux namespaces) + pqpdf-sandbox (tmpfs isolation) — four-layer chain on all heavy tools |
| Cache / state | Redis 7 (concurrency semaphore, IP rate-limit buckets, background job slots) with filesystem fallback |
| ML / AI | scikit-learn 1.8.0 (IsolationForest + RandomForest), LightGBM (gradient-boosted ensemble), numpy 1.26.4, shap, joblib, psycopg2 — continuous learning with model drift detection (⑯) |
| PQC crypto | `@noble/post-quantum` |
| PDF.js | Mozilla PDF.js (page preview rendering, DPI preview, content scanning, corruption diagnostics) |
| Colour theme | Amber `#ff8c00` + gold `#ffd700` on deep navy `#040810` |

---

## File Structure

```
pdf/
├── index.php                  # Hub — tool cards, search, drag-drop, IndexedDB file transfer; featured-tools carousel (5 slides: Forensics Scanner, PQC Encrypt, E-Sign, Merge, Convert)
├── api.php                    # Single POST endpoint — all 45 operations (sandbox chain, concurrency guard, IP rate limit, Redis integration); includes op_pdf_sanitize with 9 modes: flatten · strip · remove-js · remove-embedded · remove-xfa · remove-richmedia · normalize · flatten-forms · remove-metadata
├── api_config.php             # API config bootstrap — loads central server config
├── enterprise.php             # Enterprise / on-premise landing page — security architecture, cost & ROI with breach data, compliance, competitor comparison, CTA; 22 cited sources
├── about.php                  # About page — all 45 tools explained, privacy model, engine architecture, security controls
├── security-dashboard.php     # Security event telemetry dashboard — NDJSON log reader, stat aggregation, IP/op/UA top-N, heatmap, event log table with filters + CSV/JSON export; token-gated (PQPDF_DASHBOARD_TOKEN env var)
├── about.css                  # Styles for the about page
├── sitemap.php                # Dynamic XML sitemap with lastmod timestamps from filemtime()
├── sitemap.xml                # Static sitemap fallback
├── robots.txt                 # Crawl directives
├── favicon.ico                # Site favicon (ICO)
├── favicon.svg                # Site favicon (SVG)
├── README.md                  # This file
├── USAGE.md                   # API reference and integration examples
├── deploy/                    # Deployment and configuration scripts (self-contained — copy this dir to any server)
│   ├── start.sh               # Full installation wizard — collects all config upfront, runs every step unattended
│   ├── deploy.sh              # Step 1 — install all system prerequisites (apt, pip, npm, AppArmor, sandbox)
│   ├── extract.sh             # Step 2 — unpack ui.tar.gz into web root, create runtime dirs, set permissions
│   ├── install.sh             # Step 3 — configure application (Apache, PHP-FPM, SSL, env file, DB schema, cron)
│   ├── setup-proxy.sh         # Step 4 — install and configure pqcrypta-proxy (PQC TLS, HTTP/3, ACME, rate limiting)
│   ├── schema.sql             # PostgreSQL schema — pdf_scan_history table, indexes, grants
│   ├── package.sh             # Build a versioned distributable archive of the web UI
│   ├── ui.tar.gz              # Pre-built web UI archive — all PHP, CSS, JS, Python scripts (1.3 MB)
│   └── PREREQUISITES.md       # Full prerequisite reference (packages, services, hardware)
├── css/
│   ├── pdf.css                # Complete UI styles (incl. homepage carousel, Forensics Scanner card crimson accent)
│   ├── enterprise.css         # Enterprise landing page styles — 4-column pillar grid, full-width arch cards, 2-column feature grid, breach grid, CVE cards, compliance badges, responsive breakpoints
│   ├── pdf-background.css     # Canvas container
│   ├── pdf-cursor.css         # Ink trail cursor
│   ├── pdf-enhanced.css       # Cutting-edge UI enhancements (@property, scroll-driven animations, container queries)
│   ├── fill.css               # Fill PDF form tool styles
│   ├── scan.css               # Threat scanner styles (risk colours, engine chips, stream table, score meter, indicator cards, two-panel engine browser, SHAP/ML bars, raw forensics code blocks, 9-mode sanitize panel — ssn-* basic/advanced cards, scan-san-* about section chip grid)
│   └── security-dashboard.css # Security dashboard styles (dark panel theme, heatmap, timeline SVG, top-N bars, event table)
├── js/
│   ├── security-dashboard.js  # Security dashboard — stats rendering, SVG timeline chart, activity heatmap (7d×24h), top-N panels, event log table (sort/filter/paginate), CSV+JSON export, auto-refresh
│   ├── pdf.js                 # Hub init, search, drag-drop routing
│   ├── pdf-carousel.js        # Featured-tools carousel — auto-advance (6 s), touch swipe, keyboard, reduced-motion pause, ARIA live region
│   ├── pdf-background.js      # Floating docs + particle animation
│   ├── pdf-cursor.js          # Cursor ink trail
│   ├── pdf-processing.js      # Global 3D processing overlay + cross-page hub-drop file delivery via IndexedDB
│   ├── pdf-page-preview.js    # Shared ES module: PdfPagePreview, PdfSplitPreview, PdfReorderPreview, PdfMergePreview, renderSinglePagePreview
│   ├── pdf-enhanced.js        # Cutting-edge UI enhancements
│   ├── toolbar-drag.js        # PDF editor toolbar drag-to-reorder (CSP-compliant)
│   └── tools/                 # All tool scripts are ES modules (type="module")
│       ├── upload.js          # PdfUploadUtil — shared XHR upload handler
│       ├── scan.js            # Threat scanner — engine strip animation, 23-tab report renderer with per-engine two-panel browser (sidebar + detail pane with SHAP/feature-importance bars, stream table, URL list, per-parser diff table, phishing tags, embedded file cards, sig forensics, campaign fingerprints, correlation breakdown), Raw Forensics tab (JS sources, decoded streams, indicator contexts, structure dump), 9-mode sanitize flow (flatten · strip · remove-js · remove-embedded · remove-xfa · remove-richmedia · normalize · flatten-forms · remove-metadata)
│       ├── camera-scan.js     # Camera document scanner — live viewfinder, Sobel edge overlay, perspective handles, multi-page gallery, OCR mode
│       ├── merge.js           # Thumbnail preview + drag reorder; upload progress bar; server-phase cycling status messages (6 rotating messages while Ghostscript merges)
│       ├── split.js           # Cut-point preview + range/interval modes
│       ├── compress.js        # DPI slider, before/after split-canvas preview, size comparison
│       ├── convert.js         # PDF → Word/ODT/RTF/TXT + format fidelity star indicator
│       ├── watermark.js       # 8-position, per-page, font style, live canvas watermark preview
│       ├── sign.js            # Draw/type/upload tabs, canvas, live placement preview (PDF.js composite), crypto metadata
│       ├── sign-request.js    # Individual signer page — signature mode tabs, page/position placement, optional PAdES layer
│       ├── esign.js           # Multi-party e-signature — signer list, sequential/parallel mode, live status polling
│       ├── rotate.js          # Canvas preview, odd/even/range, decimal angles
│       ├── protect.js         # Dual-mode AES + PQC, key management
│       ├── unlock.js          # Encryption type detection (client-side header scan), AES badge, PQC bundle routing
│       ├── to-images.js       # Format, DPI, JPEG quality, page range, live DPI quality preview (PDF.js) — size estimate from pixel dimensions × bytes/pixel
│       ├── extract-text.js    # Layout, encoding, page range
│       ├── extract-pages.js   # Thumbnail selection, range compression
│       ├── pdf-info.js        # Full metadata display + quick page 1 canvas preview
│       ├── flatten.js         # PDF.js content scan (form fields, annotations, layers) with summary card
│       ├── grayscale.js       # Before/after split-canvas preview (color vs. grayscale)
│       ├── repair.js          # PDF.js corruption diagnostics (header, xref, streams, page rendering)
│       ├── reorder.js         # Drag-and-drop thumbnail grid
│       ├── delete-pages.js    # Thumbnail click selection
│       ├── pdfa.js            # Level selector (1b/2b/3b)
│       ├── pdfx.js            # PDF/X standard selector (PDF/X-1a, X-3, X-4)
│       ├── nup.js             # N-up/imposition — grid preview, booklet mode
│       ├── deskew.js          # Per-page interactive crop editor — 8-handle drag box, auto-detection, page navigator
│       ├── outline-editor.js  # Outline/bookmark editor — add, edit, delete, reorder entries
│       ├── pades.js           # PAdES-B signing — visual signature placement, LTV timestamp
│       ├── a11y.js            # Accessibility checker — tagged PDF, alt text, reading order, colour contrast
│       ├── font-inspector.js  # Font inspector — enumerate fonts, embedding status, subset flags
│       ├── color-inspect.js   # Colour inspector — spot colours, ICC profiles, overprint settings
│       ├── table-json.js      # Table → JSON extraction with preview
│       ├── word-to-pdf.js     # Format fidelity indicator
│       ├── excel-to-pdf.js    # Sheet selector (fetches sheet names from uploaded file)
│       ├── ppt-to-pdf.js      # Slide selector (fetches slide titles from uploaded file)
│       ├── pdf-to-ppt.js      # PDF → PPTX — page count display, download
│       ├── pdf-to-html.js     # PDF → HTML — styled output preview
│       ├── pdf-to-md.js       # PDF → Markdown — heading detection, code block extraction
│       ├── image-to-pdf.js
│       ├── pdf-to-excel.js
│       ├── html-to-pdf.js
│       ├── redact.js          # Text patterns + region drawing mode
│       ├── compare.js         # Side-by-side page 1 canvas previews, DPI + sensitivity controls
│       ├── edit.js            # 15-tool canvas editor, eraser, duplicate page, first/last nav, right-click thumbnail context menu, session timer
│       ├── fill.js            # AcroForm field detection + value entry (text, checkbox, radio, select, listbox), flatten-after-fill toggle
│       ├── ocr.js             # Tesseract 5 OCR — PDF.js text-layer detection, DPI time-estimate badge, server progress cycling (8 phase messages), confidence bar + 5-stat tiles, text preview tabs
│       └── workflow.js        # Visual step builder, drag reorder; 15 step types incl. sign (3 modes), redact, split-every-N (ZIP output)
├── scripts/                   # Python helpers (server-side operations)
│   ├── pdf_to_excel.py        # Table extraction helper (PyMuPDF + openpyxl)
│   ├── pdf_to_ppt.py          # PDF → PPTX (PyMuPDF page rasterisation + python-pptx)
│   ├── pdf_to_html.py         # PDF → HTML (PyMuPDF get_text("html"), positioned spans)
│   ├── pdf_to_md.py           # PDF → Markdown (heading detection, code block extraction)
│   ├── nup.py                 # N-up/imposition (PyMuPDF show_pdf_page, booklet reorder)
│   ├── deskew.py              # Auto-crop + rotation correction (PyMuPDF content bbox, per-page overrides)
│   ├── pades_sign.py          # PAdES-B signing with optional LTV timestamp (pyhanko)
│   ├── outline_write.py       # Outline/bookmark write-back (PyMuPDF set_toc)
│   ├── a11y_check.py          # Accessibility analysis (tagged PDF, alt text, reading order, contrast)
│   ├── font_inspect.py        # Font enumeration (PyMuPDF get_fonts, embedding + subset flags)
│   ├── color_inspect.py       # Colour inspection (spot colours, ICC profiles, overprint)
│   ├── table_json.py          # Table → JSON extraction (PyMuPDF find_tables)
│   └── url_to_pdf.cjs         # URL-to-PDF conversion helper (Node.js)
├── ml/                        # ML Intelligence Engine (Engine ⑯)
│   ├── train.py               # Training script — IsolationForest + RandomForest + LightGBM, runs every 30 min via cron
│   └── models/                # Trained model artefacts (git-ignored)
│       ├── isolation_forest.pkl   # Unsupervised anomaly model
│       ├── random_forest.pkl      # Supervised classifier (written once ≥50 labeled samples)
│       ├── lightgbm_model.pkl     # LightGBM gradient-boosted ensemble (written once ≥50 labeled samples)
│       ├── scaler.pkl             # StandardScaler fitted on training data
│       └── meta.json              # Sample counts, contamination rate, CV AUC, lgbm_active, lgbm_cv_auc, trained_at, feature key list
├── node_modules/acorn/        # JS AST parser for Engine ⑳ (npm install acorn)
└── tools/                     # PHP tool pages
    ├── _tool_head.php         # Shared header (CSP nonces, nav with PDF Home link)
    ├── _tool_foot.php         # Shared footer (cache-busted pdf-processing.js)
    ├── scan.php               # PDF Forensics Scanner — 44-engine structural + behavioural + ML + differential + polyglot + AST forensic analysis + 9-mode sanitize (flatten · strip active content · remove JS · remove embedded files · remove XFA · remove rich media · normalize structure · flatten forms · strip metadata)
    ├── camera-scan.php        # Camera/photo document scanner — live viewfinder, perspective correction, OCR mode
    ├── merge.php
    ├── split.php
    ├── compress.php
    ├── convert.php
    ├── watermark.php
    ├── sign.php
    ├── sign-request.php       # Individual signer page for esign workflow (?t=token&s=signer)
    ├── esign.php              # Multi-party e-signature — initiator uploads, adds signers, tracks status
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
    ├── pdfx.php               # PDF/X conversion (PDF/X-1a, X-3, X-4 via Ghostscript)
    ├── nup.php                # N-up/imposition (2-up, 4-up, 6-up, 8-up, 9-up, booklet)
    ├── deskew.php             # Auto-crop + deskew with per-page interactive crop editor
    ├── outline-editor.php     # Outline/bookmark editor — add, rename, delete, reorder
    ├── pades.php              # PAdES-B LTV signing with visual signature placement
    ├── a11y.php               # Accessibility checker (tagged PDF, alt text, reading order, colour contrast)
    ├── font-inspector.php     # Font inspector — name, type, encoding, embedded, subset, pages
    ├── color-inspect.php      # Colour inspector — spot colours, ICC profiles, overprint settings
    ├── table-json.php         # Table → JSON extraction
    ├── word-to-pdf.php
    ├── excel-to-pdf.php
    ├── ppt-to-pdf.php
    ├── pdf-to-ppt.php         # PDF → PowerPoint (PPTX)
    ├── pdf-to-html.php        # PDF → HTML
    ├── pdf-to-md.php          # PDF → Markdown
    ├── image-to-pdf.php
    ├── pdf-to-excel.php
    ├── html-to-pdf.php
    ├── redact.php
    ├── compare.php
    ├── edit.php
    ├── fill.php
    ├── ocr.php
    └── workflow.php
```

---

## Site Pages

| Page | URL | Description |
|---|---|---|
| **Home / PDF Tools** | [pqpdf.com](https://pqpdf.com) | Tool hub — all 45 PDF tools |
| **About** | [pqpdf.com/about.php](https://pqpdf.com/about.php) | Complete guide to PQ PDF — all 45 tools explained, privacy model, engine architecture, and security controls |
| **Contact** | [pqpdf.com/contact](https://pqpdf.com/contact/) | Contact form with AI behavioural human verification |
| **Legal Notice** | [pqpdf.com/legal/legal.php](https://pqpdf.com/legal/legal.php) | Terms of use, file handling guarantee, IP rights |
| **Privacy Policy** | [pqpdf.com/legal/privacy.php](https://pqpdf.com/legal/privacy.php) | Data handling, GDPR rights, zero-retention confirmation |
| **Privacy & Security** | [pqpdf.com/legal/security.php](https://pqpdf.com/legal/security.php) | Technical security page: temp-dir lifecycle, TLS config, ML data policy |
| **Sitemap** | [pqpdf.com/sitemap.php](https://pqpdf.com/sitemap.php) | Dynamic XML sitemap with `lastmod` timestamps derived from `filemtime()` |
| **Security Dashboard** | `/security-dashboard.php` | Live security event telemetry — event timeline, heatmap, top IPs/ops/UAs, filterable event log table with CSV/JSON export. `noindex, nofollow`. Token-gated via `PQPDF_DASHBOARD_TOKEN` env var or `X-Dashboard-Token` header. |

---

## API Quickstart

All operations share a single endpoint: `POST /api.php` with `multipart/form-data`.

### Compress a PDF

```bash
curl -X POST https://pqpdf.com/api.php \
  -F "operation=compress" \
  -F "quality=ebook" \
  -F "file=@input.pdf" \
  --dump-header - \
  --output compressed.pdf
```

**Quality presets:** `screen` (72 dpi) · `ebook` (150 dpi) · `printer` (300 dpi) · `prepress` (300 dpi, colour-managed)

**Response headers:**

| Header | Example | Description |
|---|---|---|
| `X-Original-Size` | `1048576` | Input file size in bytes |
| `X-Compressed-Size` | `786432` | Output file size in bytes |
| `X-Compression-Fallback` | `false` | `true` if compression grew the file and the original was returned unchanged |

### Merge PDFs

```bash
curl -X POST https://pqpdf.com/api.php \
  -F "operation=merge" \
  -F "files[]=@doc1.pdf" \
  -F "files[]=@doc2.pdf" \
  -F "files[]=@doc3.pdf" \
  --output merged.pdf
```

Up to 20 files, 200 MB total. Page order follows the array order of `files[]`.

### OCR a scanned PDF

```bash
curl -X POST https://pqpdf.com/api.php \
  -F "operation=ocr" \
  -F "dpi=200" \
  -F "format=pdf" \
  -F "psm=3" \
  -F "file=@scanned.pdf" \
  --output searchable.pdf
```

**`format`:** `txt` · `pdf` (searchable PDF) · `both` (ZIP with both)
**`psm`:** `3` auto · `4` single-column · `6` block · `11` sparse

After the request completes the response header `X-OCR-Stats` carries a JSON object:

```json
{
  "pages_processed": 12,
  "total_pages": 12,
  "word_count": 4821,
  "char_count": 28934,
  "avg_confidence": 94.3,
  "dpi": 200,
  "format": "pdf"
}
```

### Run a PDF threat scan

Scanning is two-step: start a scan, then poll for results.

**Step 1 — start scan:**

```bash
curl -X POST https://pqpdf.com/api.php \
  -F "operation=pdf-scan-start" \
  -F "file=@suspect.pdf" \
  -c cookies.txt
```

**Response:**
```json
{ "status": "started", "session_token": "abc123..." }
```

**Step 2 — poll for report:**

```bash
curl -X POST https://pqpdf.com/api.php \
  -F "operation=pdf-scan-poll" \
  -b cookies.txt
```

Returns `{ "status": "running" }` while the 20 engines execute, then the full threat report JSON once complete. Poll every 2 seconds until `status` is `done` or `error`.

**Threat report envelope:**
```json
{
  "status": "done",
  "risk_score": 42,
  "risk_level": "Suspicious",
  "ml_score": 0.17,
  "indicators": [
    {
      "engine": "Engine ②",
      "level": "High",
      "message": "/JavaScript action found in raw bytes",
      "count": 1
    }
  ],
  "engines_run": 20,
  "session_token": "abc123..."
}
```

### Common parameters

| Parameter | Description | Default |
|---|---|---|
| `operation` | Operation name (see allowed list in `api.php`) | required |
| `file` | Single PDF file (most operations) | required |
| `files[]` | Multiple files (merge, compare) | required |
| `quality` | Compress preset: `screen` / `ebook` / `printer` / `prepress` | `ebook` |
| `dpi` | Target DPI for image output / OCR | varies |
| `page_range` | Custom page range e.g. `1-3,5,7-9` | all pages |

### Limits

| Limit | Value |
|---|---|
| Max file size | 50 MB per file |
| Max total upload | 200 MB per request |
| Rate limit | 10 operations / 5 minutes (per session) |
| Shell command timeout | 120 seconds |
| OCR page cap | 100 pages per job |

HTTP `429` is returned when the rate limit is exceeded.

### Health Check

```bash
# Liveness probe — fast, used by pqcrypta-proxy every 10–30 s
curl https://pqpdf.com/health

# Full readiness check — all tools + DB + log dir
curl "https://pqpdf.com/health?full=1"
```

HTTP `200` = healthy or degraded (proxy keeps routing). HTTP `503` = unhealthy (proxy stops routing, circuit-breaker opens).

The `X-Server-ID` response header identifies which backend node answered — useful when tailing logs across a multi-server pool:

```bash
curl -I https://pqpdf.com/health
# X-Server-ID: server-01
```

---

## Enterprise FAQ

### File size and concurrency

**Q: What are the upload limits?**
50 MB per file, 200 MB total across all files in a single request. These are hard limits enforced in `api.php` (`MAX_FILE_SIZE = 52_428_800`, `MAX_TOTAL_SIZE = 209_715_200`) before any processing begins.

**Q: How many operations can I run in parallel?**
The session-based rate limit is 10 operations per 5-minute sliding window. Polling endpoints (`pdf-scan-poll`, `edit-ping`, `edit-page`) are exempt. Concurrent sessions from different browsers / IP addresses each have their own independent counter.

**Q: Is there a page count limit?**
OCR is capped at 100 pages per job. All other operations have no hard page cap, though processing time is bounded by the 120-second per-command shell timeout.

**Q: What happens to my file after I click Download?**
The temp directory is deleted by `cleanup()` *inside* `send_file()`, which is called immediately after `readfile()` streams the file to the browser. Deletion begins while the download is still in flight — there is no retention buffer, no post-download cleanup window, and no async garbage-collection job.

### Security and compliance

**Q: Does PQ PDF use any third-party cloud services for processing?**
No. Every operation — including OCR, threat scanning, format conversion, and ML inference — runs entirely on the server at pqpdf.com. No file data is forwarded to any external API, cloud storage bucket, or third-party service at any point.

**Q: How is data in transit protected?**
All connections are served over TLS 1.2/1.3 via Apache with HSTS (`max-age=31536000; includeSubDomains; preload`). HTTP connections are upgraded by the `upgrade-insecure-requests` CSP directive. Certificate pinning is not enforced at the application layer; browsers rely on standard CA chain validation.

**Q: Does the PDF forensics scanner send files or hashes to VirusTotal or any external service?**
No. The scanner is fully offline — no VirusTotal, no AlienVault OTX, no external API calls of any kind during a scan. All 44 forensic engines run entirely locally, including the Threat Intelligence engine (Engine ⑳), which queries local PostgreSQL databases. URLhaus, MalwareBazaar, and ThreatFox data is downloaded in bulk by cron and stored on-server; no data leaves the server during a scan. PyMuPDF, ExifTool, qpdf, YARA, PeePDF, pikepdf, ClamAV, the scikit-learn/LightGBM ML models, and the dynamic sandbox (`strace` + `unshare` Linux namespaces) all execute on the same server.

**Q: What data does the ML engine store?**
Engine ⑯ writes a 38-feature vector (structural heuristics, entropy scores, indicator counts) derived from each scan to PostgreSQL. **No file content, file name, IP address, or user identifier is stored.** The feature vector contains only numeric measurements extracted from the PDF structure. Stored records are used exclusively to retrain the IsolationForest, RandomForest, and LightGBM models every 30 minutes. Users can submit a feedback label (malicious / benign) via the scan report UI; this label is appended to the existing feature row, not stored separately.

**Q: Is the service GDPR-compliant?**
Files are never retained (zero-retention guarantee above). The only personal data processed is the IP address in Apache access logs, which are subject to standard server log rotation. No file content, metadata, or user-identifying information is stored in any application database. See the [Privacy Policy](https://pqpdf.com/legal/privacy.php) for full GDPR rights.

**Q: What compliance frameworks are relevant?**
The zero-retention architecture (no file persistence, no cloud forwarding, ephemeral temp dirs) is designed to be compatible with workflows governed by GDPR, HIPAA (no PHI storage), and internal data-handling policies that prohibit uploading documents to third-party SaaS. PQ PDF does not hold any formal certifications (SOC 2, ISO 27001) at this time.

### SLAs and support

**Q: What uptime SLA is offered?**
PQ PDF is a free public service with no formal SLA. There is no guaranteed uptime, no support tier, and no compensated downtime.

**Q: How do I report a security vulnerability?**
Email **contact@pqcrypta.com** with a description of the issue. We aim to acknowledge reports within 48 hours and publish fixes without unnecessary delay. Please allow reasonable time for remediation before public disclosure.

**Q: How do I request a security review or integration discussion for enterprise use?**
Use the [contact form](https://pqpdf.com/contact/) or email **contact@pqcrypta.com** with the subject line `Enterprise / Security Review`. Include your organisation name, use-case description, and any specific compliance requirements.

---

## License

Copyright © PQ PDF. All rights reserved.
