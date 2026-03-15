# Secure PDF Tools

**The Private, Server-Side PDF Toolkit — Free, Fast, and Zero-Retention.**
No uploads to third-party clouds. No accounts. No tracking. Ever.

Every operation runs entirely on the server. Files are processed and immediately deleted — nothing is stored, logged, or shared.

**Live:** [pqcrypta.com/pdf](https://pqcrypta.com/pdf)

---

## Tools (31)

### Core Manipulation

| Tool | Link | Description |
|---|---|---|
| **Merge PDFs** | [/pdf/tools/merge.php](https://pqcrypta.com/pdf/tools/merge.php) | Combine up to 20 PDFs into one (200 MB total). Drag thumbnails to reorder pages before merging. Upload progress shows real-time percentage; once upload completes, server phase cycles through descriptive status messages. |
| **Split PDF** | [/pdf/tools/split.php](https://pqcrypta.com/pdf/tools/split.php) | Split by every page, fixed interval, custom page ranges, or interactive cut-point selection. |
| **Reorder Pages** | [/pdf/tools/reorder.php](https://pqcrypta.com/pdf/tools/reorder.php) | Drag-and-drop page thumbnails to rearrange, then export. |
| **Delete Pages** | [/pdf/tools/delete-pages.php](https://pqcrypta.com/pdf/tools/delete-pages.php) | Click thumbnail grid to select pages for removal. |
| **Extract Pages** | [/pdf/tools/extract-pages.php](https://pqcrypta.com/pdf/tools/extract-pages.php) | Click thumbnail grid to select pages to keep; auto-compresses ranges (e.g. 1-3,5,7-9). |
| **Rotate Pages** | [/pdf/tools/rotate.php](https://pqcrypta.com/pdf/tools/rotate.php) | Rotate all, odd, even, or a custom range of pages. Supports 90°/180°/270° and arbitrary decimal angles. Live canvas preview of the first page. |
| **Compress PDF** | [/pdf/tools/compress.php](https://pqcrypta.com/pdf/tools/compress.php) | Five quality presets plus custom DPI slider (50–600). Optional metadata stripping, linearization, and stream recompression. Live before/after split-canvas preview rendered from page 1 when a file is selected — original on the left, simulated compressed on the right. Shows original vs. compressed size after download. |
| **Repair PDF** | [/pdf/tools/repair.php](https://pqcrypta.com/pdf/tools/repair.php) | Reconstruct corrupted or malformed PDFs via Ghostscript. On upload, PDF.js diagnoses the file client-side: checks the PDF header, attempts to parse the cross-reference table and content streams, and renders pages 1–3. A scan card reports detected issues (xref errors, stream corruption, truncation, encryption) as red badges — or confirms the file is readable with a green badge and page count. |
| **Flatten PDF** | [/pdf/tools/flatten.php](https://pqcrypta.com/pdf/tools/flatten.php) | Flatten form fields, annotations, and layers into the page content layer. On upload, PDF.js scans the document client-side and shows a summary card — e.g. "3 form fields · 2 annotations · 1 layer — all will be made permanent" — with amber badges per element type. Shows a green "Already flat" badge if no interactive elements are found. |
| **Grayscale / B&W** | [/pdf/tools/grayscale.php](https://pqcrypta.com/pdf/tools/grayscale.php) | Convert to grayscale or pure black-and-white. Live before/after split-canvas preview rendered from page 1 — color on the left, grayscale simulation on the right. |

### Format Conversion

| Tool | Link | Description |
|---|---|---|
| **PDF → Word** | [/pdf/tools/convert.php](https://pqcrypta.com/pdf/tools/convert.php) | Export to editable `.docx`, `.odt`, `.rtf`, or `.txt` via LibreOffice. Format fidelity indicator shows star ratings (out of 4) for all four formats, highlighting the active selection with a description of expected quality. |
| **PDF → Excel** | [/pdf/tools/pdf-to-excel.php](https://pqcrypta.com/pdf/tools/pdf-to-excel.php) | Export tables to `.xlsx` via LibreOffice. |
| **PDF → Images** | [/pdf/tools/to-images.php](https://pqcrypta.com/pdf/tools/to-images.php) | Render pages to PNG or JPEG at 72–600 DPI. Select all pages or a custom range. JPEG quality slider when JPEG format is chosen. Live DPI quality preview: page 1 is rendered in a canvas at the selected DPI immediately after upload; re-renders as you change the DPI setting, showing actual output pixel dimensions (e.g. "1240 × 1754 px at 150 DPI") before any server processing. Download as ZIP. |
| **PDF/A Archive** | [/pdf/tools/pdfa.php](https://pqcrypta.com/pdf/tools/pdfa.php) | Convert to PDF/A-1b, PDF/A-2b, or PDF/A-3b for long-term archival. |
| **Word → PDF** | [/pdf/tools/word-to-pdf.php](https://pqcrypta.com/pdf/tools/word-to-pdf.php) | Convert `.doc` / `.docx` / `.odt` via LibreOffice. Format fidelity indicator shows expected output quality for the uploaded file type. |
| **Excel → PDF** | [/pdf/tools/excel-to-pdf.php](https://pqcrypta.com/pdf/tools/excel-to-pdf.php) | Convert `.xls` / `.xlsx` / `.ods` via LibreOffice. Sheet selector — fetches sheet names from the uploaded file and lets you pick which sheet(s) to convert. |
| **PowerPoint → PDF** | [/pdf/tools/ppt-to-pdf.php](https://pqcrypta.com/pdf/tools/ppt-to-pdf.php) | Convert `.ppt` / `.pptx` / `.odp` via LibreOffice. Slide selector — fetches slide titles from the uploaded file and lets you choose which slides to include. |
| **Images → PDF** | [/pdf/tools/image-to-pdf.php](https://pqcrypta.com/pdf/tools/image-to-pdf.php) | Pack JPEG / PNG / WebP / BMP / TIFF images into a single PDF via ImageMagick. |
| **HTML → PDF** | [/pdf/tools/html-to-pdf.php](https://pqcrypta.com/pdf/tools/html-to-pdf.php) | Upload `.html` / `.htm` and convert via LibreOffice. |

### Protection & Security

| Tool | Link | Description |
|---|---|---|
| **PDF Threat Scanner** | [/pdf/tools/scan.php](https://pqcrypta.com/pdf/tools/scan.php) | Static, dynamic, and ML analysis across 20 detection engines: structure validation, 45+ byte-level patterns, stream entropy analysis, object graph analysis, URL extraction, metadata forensics, font anomaly detection, CVE pattern matching, ExifTool EXIF/XMP analysis, qpdf structural integrity, YARA rule matching (11 custom rules), PeePDF deep object analysis, dynamic behavioral sandbox (strace + isolated Linux namespaces), correlation analysis, ClamAV signature scanning (700k+ signatures), ML Intelligence Engine (IsolationForest + RandomForest + Bayesian contextual scoring), differential parser comparison (PyMuPDF vs pdfminer vs qpdf), polyglot/embedded binary detection (PE, ELF, ZIP, Mach-O, OLE and more), and JavaScript AST deobfuscation (acorn). Returns a scored threat report with per-indicator context and sanitize options. |
| **Protect PDF** | [/pdf/tools/protect.php](https://pqcrypta.com/pdf/tools/protect.php) | Dual-mode protection: **Standard** (AES-256-CBC server-side) or **PQC** (client-side quantum-safe encryption). See details below. |
| **Unlock PDF** | [/pdf/tools/unlock.php](https://pqcrypta.com/pdf/tools/unlock.php) | Remove password protection (owner password required). Detects the encryption type client-side by reading the PDF header before upload — shows a `🔒 AES-256 encrypted` badge for password-protected files or a `✅ No password protection detected` badge if the file is already unlocked. PQC bundles (`.pqcpdf`) are auto-detected by extension and routed to the quantum-safe decryption panel. |
| **Redact PDF** | [/pdf/tools/redact.php](https://pqcrypta.com/pdf/tools/redact.php) | Two modes: text-pattern redaction (with multi-pattern list, case sensitivity, whole-word matching) or mouse-drawn region redaction on a canvas preview. Custom fill colour. |

### Content & Annotation

| Tool | Link | Description |
|---|---|---|
| **Add Watermark** | [/pdf/tools/watermark.php](https://pqcrypta.com/pdf/tools/watermark.php) | Stamp text watermarks. 8-position placement, opacity, rotation angle, font size, font style, hex colour. Apply to all, odd, even, or custom page ranges. Live canvas preview — page 1 is rendered and the watermark text is drawn over it in real time as you adjust text, opacity, size, colour, and position. |
| **Sign PDF** | [/pdf/tools/sign.php](https://pqcrypta.com/pdf/tools/sign.php) | Three signature input methods: draw (canvas with touch support), type (text-rendered), or upload an image. Place on first/last/custom page with x/y/size controls. Live placement preview: after drawing, typing, or uploading a signature, it is composited directly onto a rendered page 1 canvas at the chosen position and size — updates in real time as you move the position selectors or drag the size slider. Optional cryptographic metadata. |
| **Edit PDF** | [/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php) | Full page-by-page visual editor with 15 annotation tools (see below). |
| **Compare PDFs** | [/pdf/tools/compare.php](https://pqcrypta.com/pdf/tools/compare.php) | Visual diff of two PDFs. Configure DPI (72/96/150/300) and sensitivity. Side-by-side page 1 canvas previews render immediately when each file is selected. Outputs a highlighted diff PDF with change regions marked. |
| **Extract Text** | [/pdf/tools/extract-text.php](https://pqcrypta.com/pdf/tools/extract-text.php) | Export all text to `.txt`. Options: layout preservation, text encoding, custom page range. |
| **PDF Info** | [/pdf/tools/pdf-info.php](https://pqcrypta.com/pdf/tools/pdf-info.php) | Display full metadata: title, author, subject, keywords, creator, producer, page count, dimensions, PDF version, encryption status, form type, tagged flag, page rotation, fast web view optimisation, creation and modification dates, permission flags. Shows a quick canvas preview of page 1 alongside the metadata. |
| **OCR PDF** | [/pdf/tools/ocr.php](https://pqcrypta.com/pdf/tools/ocr.php) | Optical Character Recognition for scanned and image-based PDFs. Powered by Tesseract 5 LSTM neural network. Three output formats: plain text (.txt), searchable PDF (original images + invisible text layer so the document becomes copyable and searchable), or a ZIP containing both. DPI control (150/200/300), four page segmentation modes (auto, single column, single block, sparse text), custom page ranges, up to 100 pages per job. Returns OCR confidence score (per-word Tesseract TSV confidence averaged across all pages), word count, and character count. Live text preview tab in the browser — preview extracted text without downloading. |

### Automation

| Tool | Link | Description |
|---|---|---|
| **Workflow Builder** | [/pdf/tools/workflow.php](https://pqcrypta.com/pdf/tools/workflow.php) | Chain operations visually: add steps from the picker, configure per-step parameters, drag to reorder. Supported steps: rotate, compress, watermark, protect, unlock, grayscale, flatten, repair, extract pages, delete pages, reorder pages, convert to PDF/A, **sign** (typed visual / typed visual + digital / digital-only with auto self-signed cert), **redact** (permanent text-pattern removal, case-sensitive option, black or white fill), and **split every N pages** (terminal step — outputs a ZIP of equal-sized PDF chunks, useful for batch-scanned documents). Save named workflows to localStorage; **Load** replaces current steps, **+ Append** joins a saved workflow onto the end of the current one — enabling complex pipelines composed from saved building blocks. Export / import workflows as JSON. Runs the full sequence on one or more uploaded PDFs. |

---

## PDF Threat Scanner — 20 Detection Engines

[/pdf/tools/scan.php](https://pqcrypta.com/pdf/tools/scan.php)

15 static analysis engines, one dynamic behavioral sandbox, an ML Intelligence Engine, a differential parsing engine, a polyglot/embedded binary detector, and a JavaScript AST deobfuscation engine — the PDF is rendered through three independent interpreters inside isolated Linux namespaces with full syscall tracing, and every scan is persisted to PostgreSQL to continuously improve the ML models. All 20 engines run server-side in a single request. The file is held in a temporary directory during the scan and deleted immediately after (or after a sanitize follow-up using the session token issued with the report).

### How It Works

1. The PDF is uploaded and saved to an isolated temporary directory. A session token is returned for optional sanitize follow-up.
2. A Python script runs all 13 static heuristic engines (including ExifTool, qpdf, YARA, and PeePDF) against the raw bytes and parsed object graph via PyMuPDF.
3. Engine ⑭ renders the PDF through Ghostscript, MuPDF, and Poppler inside isolated Linux namespaces (`unshare --net --pid --mount`) with all syscalls captured by `strace`. Detects runtime behavior invisible to static analysis.
4. Engine ⑯ calls the local ClamAV daemon (`clamdscan --fdpass`) in the same request, falling back to `clamscan` if the daemon returns an error.
5. Engine ⑰ extracts a 38-feature vector from all preceding engine outputs, applies Bayesian contextual scoring, runs IsolationForest anomaly detection (unsupervised — works from scan 1), and RandomForest classification (activates at ≥50 labeled samples). Reports per-scan feature importance (Explainable ML). Scan features, scores, and auto-inferred labels are persisted to PostgreSQL. Training runs every 30 minutes via cron.
6. Engine ⑱ re-runs MuPDF (`mutool`), Poppler (`pdfinfo`/`pdfdetach`), and Ghostscript independently and compares page counts, object counts, JavaScript presence, and embedded file counts. Parser disagreement signals hidden objects, shadow object trees, or deliberate parser-confusion exploits.
7. Engine ⑲ scans every stream (raw and decompressed) for file magic byte signatures — ZIP, Windows PE, Linux ELF, Mach-O, Java class, OLE/CFBF, RAR, 7-Zip, embedded PostScript — to detect polyglot files that embed executable droppers inside a valid PDF container.
8. Engine ⑳ extracts JavaScript from `/JS` literals and keyword-bearing compressed streams, parses each through the Acorn AST parser, and walks the AST detecting obfuscation constructs invisible to text-pattern matching: `eval()` chains, `String.fromCharCode()` arrays (shellcode staging), `unescape()` decode pipelines, large numeric arrays (heap spray), and `new Function()` dynamic construction.
9. All indicators are deduplicated, sorted by risk level, and returned as JSON with a composite risk score and ML malicious-probability score.
10. The client renders an eight-tab report: Summary, 🧠 ML (probability bar, explainable feature importance, feedback), 🔬 Parsing (differential parser comparison), 🧬 Polyglot (magic-byte + AST deobfuscation), Threats, URLs, Streams, Metadata. Clickable stat cards on the Summary tab navigate directly to the corresponding tab. An animated engine-chip strip shows each engine completing in sequence during the upload.

### Scoring

Each indicator contributes base points multiplied by `min(count, 3)`:

| Risk Level | Base Points |
|---|---|
| Critical | 50 |
| High | 25 |
| Medium | 10 |
| Low | 3 |

The Correlation Engine (⑮) adds weighted bonus points (35–100) on top for dangerous indicator combinations. Final score is capped at 999.

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

**Shellcode & heapspray signatures**
`%u9090` (Unicode NOP sled), `%u4141` (classic heapspray fill), `%u0c0c%u0c0c` (0x0C heap-fill pattern), `%u0d0d%u0d0d` (0x0D heap-fill pattern)

### Engine ③ — Stream Decompressor & Content Inspector

Opens every object in the xref graph (up to 6,000 objects) via PyMuPDF and inspects each stream:

- **Decompression** — calls `doc.xref_stream(xref)` to decompress FlateDecode and other encoded streams, then re-scans the decompressed content — catching JavaScript and shellcode hidden inside compressed objects that raw-byte scanners miss entirely
- **Shannon entropy** — calculates per-stream entropy (0–8 bits/byte); values above 7.2 on non-image streams flag encrypted, packed, or obfuscated payloads. Image streams (`/DCTDecode`, `/JPXDecode`, `/CCITTFax`, `/JBIG2`) are excluded from entropy flagging.
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

URLs are de-duplicated and capped at 150. The Correlation Engine cross-references extracted URLs and flags three patterns as High: IP-literal addresses (`http://1.2.3.4/...`), raw port numbers (`http://host:4444/...`), and 12+ character random-looking subdomains.

### Engine ⑥ — Metadata Analyzer

Reads all standard PDF metadata fields via PyMuPDF (`title`, `author`, `subject`, `keywords`, `creator`, `producer`, `creationDate`, `modDate`, `format`) and performs three checks:

- **Exploit-tool strings** — scans `Creator` and `Producer` fields for known strings: `exploit`, `metasploit`, `canvas`, `core impact`, `meterpreter`, `shellcode`, `payload`, `pdfcrack`, `hashcat`, `dompdf exploit`. A match reports the exact field value as Critical.
- **Empty metadata** — flags PDFs with no title, author, creator, or producer. Exploit-crafted PDFs routinely strip all metadata to reduce forensic attribution and avoid reputation-based sandbox triggers. Reported as Low; escalated by the Correlation Engine when combined with JavaScript or embedded files.
- **XMP stream** — reads the XML metadata stream and flags any `<script` or `javascript` reference as High.

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

### Engine ⑩ — ExifTool Metadata Forensics

Runs `exiftool -PDF:all -XMP:all` to extract metadata layers that are invisible to PyMuPDF's document model:

- **Exploit-kit fingerprinting** — scans Creator, Producer, Author, Subject, Keywords, and XMPToolkit fields for known exploit-tool strings: Metasploit, msfvenom, Canvas, Core Impact, shellcode, payload, pdfcrack, dompdf exploit
- **XFA confirmation** — independently verifies `HasXFA` from EXIF metadata (cross-check against Engine ④)
- **Embedded attachment detection** — surfaces `EmbeddedFileSize` / `EmbeddedFile` fields visible only via EXIF layer
- **Summary export** — Creator, Producer, CreateDate, ModifyDate, PDFVersion, Linearized, PageCount, HasXFA, and Encryption exported to the structure dictionary for the Metadata tab
- **Feeds Correlation Engine** — sets `exiftool_exploit_found` flag used by Engine ⑮ for compound scoring

### Engine ⑪ — qpdf Structural Integrity

Runs `qpdf --check` to validate cross-reference tables, trailer dictionaries, and overall document structure:

- **XRef reconstruction** — if qpdf must reconstruct the xref table, reports as High risk (deliberately broken xref is a common technique to hide exploit objects from basic parsers while still rendering in vulnerable viewers)
- **Damaged structure** — explicit "damaged" report from qpdf is flagged as High risk (intentional corruption to conceal exploit content from scanners)
- **Structural errors** — other qpdf errors flagged as Medium risk (up to 3× count multiplier)
- **Structural warnings** — minor anomalies flagged as Low risk
- **Status export** — `qpdf_status` (ok / warnings / errors / damaged) exported to structure dictionary
- **Feeds Correlation Engine** — sets `qpdf_damaged` flag used by Engine ⑮ for compound scoring

### Engine ⑫ — YARA Rule Engine

Compiles and matches 11 custom YARA rules targeting PDF-specific attack byte patterns — independent of PDF structure parsing:

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

- **Byte-level independence** — YARA scans raw file bytes, bypassing PDF parser layers entirely; catches patterns hidden in object streams
- **Feeds Correlation Engine** — populates `yara_hits` set used by Engine ⑮ for compound scoring

### Engine ⑬ — PeePDF Deep Analysis

Parses the full PDF object tree using the PeePDF framework — an entirely independent parser from PyMuPDF:

- **Vulnerability patterns** — PeePDF's built-in vulnerability detector flags known CVE pattern combinations; each confirmed pattern reported as Critical
- **Suspicious element location** — reports exact object IDs for dangerous elements from both `Suspicious elements` and `Dangerous elements` dictionaries: `/Launch`, `getIcon()`, `printf()`, `unescape()`, `exportDataObject()`, `submitForm()`, `/EmbeddedFile`, `/JS`, `/JavaScript`, `eval()`, `/OpenAction`, `/AA`, `/XFA`, `/URI`
- **JavaScript object inventory** — lists all PDF object IDs containing JavaScript
- **Independent verification** — cross-checks PyMuPDF-based findings; if both parsers flag the same element, the compound risk in Engine ⑮ is elevated
- **Summary export** — PDF version, object count, stream count, and vulnerability count exported to structure dictionary
- **Feeds Correlation Engine** — sets `peepdf_vuln_count` used by Engine ⑮ for compound scoring

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
- **Feeds Correlation Engine** — sets `sandbox_network_attempts`, `sandbox_mmap_exec_anon`, `sandbox_exec_attempts`, `sandbox_behavioral_score` flags used by Engine ⑮ for compound scoring

### Engine ⑮ — Correlation Engine

Cross-references all findings from Engines ①–⑭ and scores 30+ dangerous indicator combinations that are orders of magnitude more serious than their individual parts. Each matched combination adds a weighted bonus on top of the base indicator scores.

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

**Dampening:** isolated `/OpenAction` with no JavaScript, no `/Launch`, no embedded files, no heapspray, no XFA, and no RichMedia has its score reduced by 7 points — `/OpenAction` alone is common in legitimate PDFs for navigation and zoom.

### Engine ⑯ — ClamAV Signature Scanner

Runs the local ClamAV daemon against the PDF and reports any signature matches:

- **Database** — 700,000+ signatures updated daily via `freshclam`, including the full `Pdf.Exploit.*` family (`Pdf.Exploit.CVE_2009_0927`, `Pdf.Exploit.CVE_2009_4324`, `Exploit.PDF-JS.*`, and many more)
- **Method** — calls `clamdscan --no-summary --fdpass <path>` to pass the open file descriptor directly to the `clamd` daemon, avoiding filesystem permission issues and reusing the already-loaded in-memory signature database for speed (~50–200 ms per scan)
- **Fallback** — if the daemon returns an error (exit code 2), falls back automatically to `clamscan` (in-process load, ~2–5 s)
- **Results** — exit code 0 = clean (engine recorded in `engines_run`, no indicator added); exit code 1 = match found, signature name extracted from output and reported as Critical; exit code 2 = scanner error, recorded in `structure.clamav_error`
- **Distinction** — Engines ①–⑮ use heuristics, structural analysis, and dynamic execution to catch zero-days and novel exploits; Engine ⑯ provides authoritative signature intelligence for confirmed known threats. A ClamAV match auto-labels the scan record as `malicious` in the ML training database.

### Engine ⑰ — ML Intelligence Engine

Extracts a 38-feature vector from all 16 preceding engine outputs and applies a three-layer ML scoring pipeline:

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

**Explainable ML**

When the RandomForest model is active, the per-scan feature importance is computed: for each of the 38 features, the model's `feature_importances_` is multiplied by the feature's presence in the current scan. Top 8 non-zero contributing features are returned as `ml_feature_importance` and rendered as a proportional bar chart in the ML panel.

**Result display**

The Summary tab shows a lime ML panel: malicious probability bar (0–100%), model version, contextual adjustment note, Explainable ML feature importance chart (top contributing features), and false-positive / confirm-threat feedback buttons. Feedback POSTs `scan_id` + `feedback` to `api.php?operation=pdf-scan-feedback`.

### Engine ⑱ — Differential Parsing Detection

Runs three independent PDF parsers against the same file and compares their structural interpretation:

| Parser | Tool | Data extracted |
|---|---|---|
| MuPDF | `mutool show xref` / `mutool info` | Object count (from xref slot count), page count, JavaScript in trailer |
| Poppler | `pdfinfo` / `pdfdetach -list` | Page count, JavaScript yes/no, embedded file count, encryption status |
| Ghostscript | `gs -sDEVICE=ps2write -sOutputFile=-` | Page count (from `%%Page:` markers), JavaScript in output |

**Flags raised:**

- **Page count disagreement** (any difference) → High + 35 pts — indicates hidden incremental update, shadow object tree, or parser-confusion exploit
- **JavaScript visibility discrepancy** (JS visible to some parsers, not others) → Critical + 50 pts — hidden JS behind parser-specific quirks (duplicate object IDs, broken references, malformed stream boundaries)
- **Object count discrepancy >10%** (MuPDF vs any) → Medium + 20 pts — duplicate object numbers or hidden objects in compressed object streams

**Why it matters:** Attackers craft PDFs where one parser recovers hidden exploit objects that another ignores entirely. Standard single-parser scanners miss this by design. This is the same technique used by browser security teams comparing Chromium vs Firefox DOM construction.

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

**Why AST over patterns:** Text-pattern scanners look for the string `eval`. Obfuscated payloads spell it as `e\u0076al`, `window['ev'+'al']`, or build it via `String.fromCharCode`. The AST sees the *meaning* — a `CallExpression` with a callee named `eval` — regardless of how the source was written. Up to 6 fragments are analyzed per scan; each run is capped at 60 KB.

**Dependency:** `acorn` (npm, `/var/www/html/node_modules/acorn/`). Invoked as a Node.js subprocess.

### Report Tabs

The tab bar uses a pill-style design with background highlighting on hover and an amber-tinted active state. Stat cards for **Threats Found**, **URLs Found**, and **Total Streams** are clickable — clicking navigates directly to the corresponding tab and smooth-scrolls the panel into view.

| Tab | Contents |
|---|---|
| **📊 Summary** | Risk banner (Clean / Low Risk / Suspicious / High Risk / Dangerous), composite score meter (0–999), 15-cell stats grid (clickable Threats/URLs/Streams cards), engines-completed pill strip, top 5 threats preview with link to full Threats tab |
| **⚠️ Threats** | All indicators grouped by risk level (Critical → High → Medium → Low), each showing badge, key, description, count, engine attribution, and byte-context snippet |
| **🌐 URLs** | All unique HTTP/HTTPS URLs extracted from raw bytes and decompressed streams, with per-URL copy-to-clipboard button |
| **📦 Streams** | Table of displayed streams (top 40 of N total; explains decompressed count vs skipped images/fonts). Columns: xref, type, decompressed size, Shannon entropy bar, suspicious flag, matched pattern list. Suspicious streams highlighted in amber. |
| **🧠 ML** | ML Intelligence Engine panel: malicious probability bar, model version, contextual dampening/amplification note, Explainable ML feature importance chart, false-positive / confirm-threat feedback buttons. Tab badge shows current malicious % score. |
| **🔬 Parsing** | Differential Parsing Detection panel: per-parser (PyMuPDF / pdfminer / qpdf) page count, object count, JavaScript detection, embedded file count. Mismatch badges highlight discrepancies that indicate parser-evasion attacks. |
| **🧬 Polyglot** | Polyglot/Embedded Binary Detection panel (magic-byte hits with type and risk badge) + JavaScript AST Deobfuscation panel (obfuscation findings — dynamic eval, fromCharCode arrays, unescape calls, large numeric arrays, new Function). |
| **🏷️ Metadata** | Document metadata KV table (title, author, subject, keywords, creator, producer, dates, format, XMP flag) + structure info KV table (version, EOF markers, xref tables, linearized, binary comment, stream counts) |

### Sanitize Options

After a scan, two sanitize methods are available using the session token — the original is never modified.

| Method | How | Safety |
|---|---|---|
| **Flatten to Images** | Renders every page to 144 DPI raster images via PyMuPDF, rebuilds as a new PDF | Maximum — destroys all JavaScript, launch actions, embedded files, XFA forms, rich media, and object streams with absolute certainty. Text becomes non-searchable. |
| **Strip Active Content** | Re-processes through Ghostscript with `-dSAFER` | Preserves searchable text and document structure. Removes JavaScript, launch actions, embedded files, and rich media. Cannot guarantee removal of zero-day or heavily obfuscated exploit structures. |

---

## Edit PDF — 15 Annotation Tools

[/pdf/tools/edit.php](https://pqcrypta.com/pdf/tools/edit.php)

The editor renders each page to a canvas and applies all changes server-side via PyMuPDF.

| Tool | Description |
|---|---|
| **Text** | Click to place a text box. Font family (Helvetica / Times / Courier), font size, bold, italic, left/centre/right alignment, colour. |
| **Draw** | Freehand pen with configurable line width (slider with live preview) and colour. |
| **Eraser** | Freehand white-stroke eraser — paints white over any content. Line width scales with the width slider (2× multiplier, minimum 10 pt). Applied as a white draw path via PyMuPDF on export. |
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

**Live placement preview:** after drawing, typing, or uploading a signature, a preview canvas renders page 1 of the uploaded PDF with the signature composited at the chosen position and size. A dashed amber border highlights the signature bounding box. The preview updates in real time as you adjust position, size, switch tabs, or modify the signature — no server round-trip required.

**Cryptographic metadata (optional, expandable):** signer name, email, reason, location, date stamp. Supports auto-generated or custom certificate (`cert_source: auto | own`, `cert_file`, `cert_password`).

---

## Flatten PDF — Content Scan

[/pdf/tools/flatten.php](https://pqcrypta.com/pdf/tools/flatten.php)

On upload, PDF.js scans the document client-side before submission:

- **Form fields** — counted via `getFieldObjects()`
- **Annotations** — non-link, non-widget annotations counted per page via `getAnnotations()`
- **Layers** — optional content groups counted via `getOptionalContentConfig()`

A scan card appears immediately with a human-readable summary and amber badges per element type:

> *3 form fields · 2 annotations · 1 layer — all will be made permanent*

If no interactive elements are found, a green **Already flat** badge is shown. If the file is encrypted or unreadable, a red **Scan failed** badge appears — flattening can still proceed.

---

## Repair PDF — Corruption Diagnostics

[/pdf/tools/repair.php](https://pqcrypta.com/pdf/tools/repair.php)

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

[/pdf/tools/unlock.php](https://pqcrypta.com/pdf/tools/unlock.php)

When a `.pdf` file is selected, the first 4 KB is read client-side and checked for the `/Encrypt` dictionary marker:

| Result | Badge |
|---|---|
| Password-protected | 🔒 `AES-256 encrypted — password required` (amber) |
| Not encrypted | ✅ `No password protection detected` (green) |

PQC bundles (`.pqcpdf`) are auto-detected by file extension and routed directly to the quantum-safe decryption panel — the encryption badge is not shown for these files as the bundle already contains full algorithm and key metadata.

---

## PDF → Images — Live DPI Preview

[/pdf/tools/to-images.php](https://pqcrypta.com/pdf/tools/to-images.php)

After upload, page 1 is rendered via PDF.js at the currently selected DPI. A preview canvas appears between the DPI selector and the pages options, showing the actual output resolution before any server processing:

- Renders at the selected DPI scale (`dpi / 72` × page viewport), capped to 360 px wide for display
- A hint line shows exact pixel dimensions: e.g. `Actual output: 1240 × 1754 px at 150 DPI`
- Re-renders automatically when you switch DPI — no submit required
- Shows `⚠️ very large files` warning inline at 300 DPI

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
| Threat scanning | PyMuPDF (heuristic engines ①–⑨), ExifTool 12 (⑩), qpdf 11.9 (⑪), YARA 4.5 (⑫), PeePDF 0.4 (⑬), strace 6.8 + unshare (⑭ sandbox), Correlation (⑮), ClamAV 1.4+ (⑯) |
| ML / AI | scikit-learn 1.8.0 (IsolationForest + RandomForest), numpy 1.26.4, joblib, psycopg2 — continuous learning (⑰) |
| PQC crypto | `@noble/post-quantum` |
| PDF.js | Mozilla PDF.js (page preview rendering, DPI preview, content scanning, corruption diagnostics) |
| Colour theme | Amber `#ff8c00` + gold `#ffd700` on deep navy `#040810` |

---

## File Structure

```
pdf/
├── index.php                  # Hub — tool cards, search, drag-drop, IndexedDB file transfer
├── api.php                    # Single POST endpoint — all 40 operations
├── README.md                  # This file
├── css/
│   ├── pdf.css                # Complete UI styles
│   ├── pdf-background.css     # Canvas container
│   ├── pdf-cursor.css         # Ink trail cursor
│   └── scan.css               # Threat scanner styles (risk colours, engine chips, stream table, score meter)
├── js/
│   ├── pdf.js                 # Hub init, search, drag-drop routing
│   ├── pdf-background.js      # Floating docs + particle animation
│   ├── pdf-cursor.js          # Cursor ink trail
│   ├── pdf-processing.js      # Global 3D processing overlay + cross-page hub-drop file delivery via IndexedDB
│   ├── pdf-page-preview.js    # Shared ES module: PdfPagePreview, PdfSplitPreview, PdfReorderPreview, PdfMergePreview, renderSinglePagePreview
│   └── tools/                 # All tool scripts are ES modules (type="module")
│       ├── upload.js          # PdfUploadUtil — shared XHR upload handler
│       ├── scan.js            # Threat scanner — engine strip animation, 8-tab report renderer (Summary/ML/Parsing/Polyglot/Threats/URLs/Streams/Metadata), clickable stat cards, ML panel (probability bar, explainable feature importance, feedback), differential parsing panel, polyglot panel, JS AST panel, sanitize flow
│       ├── merge.js           # Thumbnail preview + drag reorder; upload progress bar; server-phase cycling status messages (6 rotating messages while Ghostscript merges)
│       ├── split.js           # Cut-point preview + range/interval modes
│       ├── compress.js        # DPI slider, before/after split-canvas preview, size comparison
│       ├── convert.js         # PDF → Word/ODT/RTF/TXT + format fidelity star indicator
│       ├── watermark.js       # 8-position, per-page, font style, live canvas watermark preview
│       ├── sign.js            # Draw/type/upload tabs, canvas, live placement preview (PDF.js composite), crypto metadata
│       ├── rotate.js          # Canvas preview, odd/even/range, decimal angles
│       ├── protect.js         # Dual-mode AES + PQC, key management
│       ├── unlock.js          # Encryption type detection (client-side header scan), AES badge, PQC bundle routing
│       ├── to-images.js       # Format, DPI, JPEG quality, page range, live DPI quality preview (PDF.js)
│       ├── extract-text.js    # Layout, encoding, page range
│       ├── extract-pages.js   # Thumbnail selection, range compression
│       ├── pdf-info.js        # Full metadata display + quick page 1 canvas preview
│       ├── flatten.js         # PDF.js content scan (form fields, annotations, layers) with summary card
│       ├── grayscale.js       # Before/after split-canvas preview (color vs. grayscale)
│       ├── repair.js          # PDF.js corruption diagnostics (header, xref, streams, page rendering)
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
│       ├── edit.js            # 15-tool canvas editor, eraser, duplicate page, first/last nav, right-click thumbnail context menu, session timer
│       └── workflow.js        # Visual step builder, drag reorder; 15 step types incl. sign (3 modes), redact, split-every-N (ZIP output)
├── scripts/                   # Python helpers (server-side operations)
│   ├── pdf_to_excel.py        # Table extraction helper (PyMuPDF + openpyxl)
│   └── url_to_pdf.cjs         # URL-to-PDF conversion helper (Node.js)
├── ml/                        # ML Intelligence Engine (Engine ⑰)
│   ├── train.py               # Training script — IsolationForest + RandomForest, runs every 30 min via cron
│   └── models/                # Trained model artefacts (git-ignored)
│       ├── isolation_forest.pkl   # Unsupervised anomaly model
│       ├── random_forest.pkl      # Supervised classifier (written once ≥50 labeled samples)
│       ├── scaler.pkl             # StandardScaler fitted on training data
│       └── meta.json              # Sample counts, contamination rate, CV AUC, feature key list
├── node_modules/acorn/        # JS AST parser for Engine ⑳ (npm install acorn)
└── tools/                     # PHP tool pages
    ├── _tool_head.php         # Shared header (CSP nonces, nav with PDF Home link)
    ├── _tool_foot.php         # Shared footer (cache-busted pdf-processing.js)
    ├── scan.php               # PDF Threat Scanner — 20-engine static + dynamic + ML + differential + polyglot + AST analysis + sanitize
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
