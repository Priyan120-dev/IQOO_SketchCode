# SketchCode Architecture Decision Records (ADRs)

Key architectural and technical decisions governing the SketchCode on-device developer tool.

**Author: Priyan**  
**Track: Developer Tools (iQOO Hackathon 2026 Grand Finale)**  
**Status: Architecture Decision Records (Plan & Rationale)**

---

## ADR 001: Progressive Web App (PWA) over Native Android APK

### Context
SketchCode targets developers and students who need instant access without app store friction, permission gatekeeping, or massive install payloads. Furthermore, the tool itself previews web and mobile applications directly in a browser context.

### Decision
Build SketchCode as an installable Progressive Web App (PWA) using Vite and modern Web APIs (WebGPU, WebAssembly, Web Audio, MediaStream).

### Consequences & Trade-offs
- **Positives**:
  - Instant loading via Service Workers; zero app store packaging or review cycles.
  - Native hardware acceleration via WebGPU (Direct Adreno GPU / Snapdragon compute).
  - Cross-platform portability (runs identically on phone and paired desktop).
- **Trade-offs**:
  - Direct low-level NPU (Hexagon DSP) access via Android NDK is not directly accessible through web standards without WebNN. NPU targeting is a stretch goal; WebGPU compute shaders on the Adreno GPU provide the reliable baseline.
  - Browser memory limits (typically 2GB–4GB per tab) require strict memory management for model weights.

---

## ADR 002: Constrained JSON UI Specification over Free-Form Code Generation

### Context
Commercial LLMs with hundreds of billions of parameters running in data centers can generate full React or Flutter files with moderate success. However, 1.5B–2B parameter models running locally on mobile hardware suffer from severe syntax errors, missing closing brackets, and imaginary dependencies when writing unconstrained code.

### Decision
Prohibit free-form code output. Constrain the local model's output strictly to a validated JSON UI specification with seven primitive components (`screen`, `navbar`, `text`, `input`, `button`, `list`, `image`). Render the UI using a fixed, deterministic component engine.

### Consequences & Trade-offs
- **Positives**:
  - **Predictable Error Handling**: Schema validation catches malformed output; invalid output falls back to a safe default rather than crashing the entire app.
  - **Security**: Eliminates arbitrary code execution (`eval()`, dynamic scripts) in the preview environment.
  - **Reliability on Small Models**: The local model only fills well-defined schema slots, dramatically improving completion success.
- **Trade-offs**:
  - Constrained to standard mobile layouts; custom canvases or bespoke animations are deferred to desktop export.

---

## ADR 003: On-Device Inference (Offline After One-Time Model Download)

### Context
SketchCode addresses the real-world friction experienced by students and mobile-first developers in India: spotty 4G/5G connections, depleted mobile data packs, high cloud API subscription costs, and sensitive early-stage IP privacy.

### Decision
All perception, speech transcription, language modeling, and rendering steps run on the user's iQOO device, operating completely offline after a one-time initial model download.

### Consequences & Trade-offs
- **Positives**:
  - **Zero Operating Cost**: No monthly API billing, no server hosting bills, completely free for students.
  - **Offline After Setup**: Once weights are cached, works reliably on trains, rural regions, college labs, and during power outages.
  - **Total Privacy**: User sketches, voice clips, and app concepts never leave the physical device.
- **Trade-offs**:
  - Initial one-time asset cache (model weights) requires ~1 GB download on Wi-Fi/hotspot.
  - Peak battery consumption during generation cycles.

---

## ADR 004: Whisper-Tiny for Speech (English Primary, Tamil Experimental)

### Context
Users should be able to describe interaction intent casually using speech in English or Tamil without sending raw audio to cloud speech endpoints (e.g., Google Cloud Speech, OpenAI Whisper API).

### Decision
Use quantized `whisper-tiny` packaged in ONNX format via `@xenova/transformers` running inside a dedicated background Web Worker, treating English as primary and Tamil as experimental.

### Consequences & Trade-offs
- **Positives**:
  - Compact footprint (~39MB–75MB quantized), fitting comfortably within mobile RAM budgets.
  - English transcription is relatively robust for simple app commands.
  - Isolated thread execution prevents UI thread stuttering during voice recording.
- **Trade-offs**:
  - Tamil transcription accuracy is limited on the tiny model variant and noisy environments. This is mitigated through on-screen intent chips and manual transcript touch-up.

---

## ADR 005: Tesseract.js (WASM) & Heuristic Contours over Heavy Vision-Language Models

### Context
Multimodal Vision-Language Models (such as LLaVA or MiniCPM-V) capable of understanding images directly are typically 3B–8B parameters in size, demanding 4GB–8GB of VRAM and causing out-of-memory (OOM) browser crashes on mid-range phones.

### Decision
Decouple layout recognition into two lightweight, classical steps:
1. Heuristic geometric contour detection on an HTML5 canvas for bounding boxes.
2. Quantized Tesseract.js WASM worker for text/label extraction.
Pass the extracted geometric metadata and tokens as text context to the 1.5B LLM.

### Consequences & Trade-offs
- **Positives**:
  - Instant geometric detection (<150ms on mobile CPU).
  - Tesseract.js WASM requires under 25MB of memory.
  - Frees up almost the entire WebGPU memory budget exclusively for the text LLM.
- **Trade-offs**:
  - Requires drawings to have reasonably distinct geometric boundaries (e.g. boxed buttons, clear underlines).

---

## ADR 006: Planned Office Kit Bridge (To Be Validated)

### Context
Users developing an idea on their phone eventually reach a stage where they want to open the full project in an IDE (e.g., VS Code) on a laptop for production backend coding. Traditional tools require setting up cloud sync accounts, Git repositories, and web authentication.

### Decision
Planned: explore syncing project files via iQOO Office Kit (to be validated during the Grand Finale). When the user taps "Sync to Laptop", SketchCode plans to serialize the spec into a standard React + Tailwind project and transfer it via local Office Kit file sharing or multi-device clipboard.

### Consequences & Trade-offs
- **Positives**:
  - Explores hardware ecosystem integration with iQOO laptops and tablets.
  - Avoids cloud credentials, internet connectivity, or remote accounts.
- **Trade-offs**:
  - The exact programmatic API and reliability of Office Kit file bridge for automated PWA handoff is unverified and will be validated during the hackathon. Local file download serves as the reliable fallback.
