# SketchCode Offline Architecture and Privacy Model

Technical specification for zero-egress, 100% on-device operation, storage boundaries, and model caching.

**Author: Priyan**  
**Track: Developer Tools (iQOO Hackathon 2026 Grand Finale)**  
**Status: Architecture Plan (Pre-event design)**

---

## 1. Core Principle: Offline After One-Time Model Download

SketchCode is built to run entirely offline once initialized. The application requires an initial network connection for a one-time download of quantized model weights (~1 GB total). Once cached into the browser's persistent storage, the entire tool functions 100% offline with zero external network calls.

```
[One-Time Setup: Download & Cache Models (~1 GB)]
                        |
                        v
+-------------------------------------------------------------+
|               iQOO Smartphone (100% Offline Mode)           |
|                                                             |
|  [ Camera ]    --> [ Memory Canvas ]     --> (In-Memory)   |
|  [ Microphone] --> [ 16kHz PCM Buffer ]  --> (WASM Worker) |
|  [ Sketch Spec]--> [ Local IndexedDB ]   --> (Device Only) |
|                                                             |
|  XXXXXXXXXXXX ZERO TRAFFIC (POST-DOWNLOAD) XXXXXXXXXXXX    |
+-------------------------------------------------------------+
```

---

## 2. Local Data Boundaries

All transient and persistent artifacts remain within the sandboxed browser environment:

| Data Type | Lifecycle | Storage Mechanism | Remote Egress |
| :--- | :--- | :--- | :--- |
| **Camera Frames** | Ephemeral | 2D Canvas buffer; cleared immediately after contour scan | **None (0 bytes)** |
| **Microphone Audio** | Ephemeral | Int16 Web Worker array; garbage collected after Whisper pass | **None (0 bytes)** |
| **Transcribed Text** | Session | React state; persisted to local project schema if saved | **None (0 bytes)** |
| **UI Spec JSON** | Persistent | Browser IndexedDB / Origin Private File System (OPFS) | **None (0 bytes)** |
| **Model Weights** | Cached | WebGPU Cache Storage API & IndexedDB blob storage | **None (0 bytes)** |
| **Telemetry / Logs** | Ephemeral | In-memory console log only; no analytics SDKs bundled | **None (0 bytes)** |

---

## 3. Model Weight Caching and Distribution Plan

Running AI models locally requires a deliberate approach to weight delivery, especially for users with limited data quotas.

### Planned Model Footprint

| Component | Target Model | Quantization | Approximate Storage | Execution Target |
| :--- | :--- | :--- | :--- | :--- |
| **Vision / OCR** | Tesseract.js WASM + Eng/Traineddata | Compact WASM | ~18 MB | CPU (WebAssembly) |
| **Speech-to-Text** | Whisper-tiny (English primary, Tamil experimental) | ONNX int8 | ~42 MB | CPU / Web Worker |
| **LLM Engine** | Qwen 2.5 1.5B or Gemma 2 2B | q4f16_1 (WebLLM) | ~950 MB | GPU via WebGPU |
| **Application Bundle**| Vite + React + Lucide Icons | Brotli compressed | ~1.8 MB | Service Worker |

### Caching Architecture

1. **Service Worker Layer**: Standard web app assets (HTML, CSS, JS, icon SVGs) are aggressively pre-cached on installation.
2. **IndexedDB Weight Store**: Quantized ONNX and WebLLM tensor shards are verified with SHA-256 checksums and written to persistent IndexedDB partitions.
3. **Storage Persistence Request**: SketchCode invokes `navigator.storage.persist()` on launch to prevent browser eviction during storage pressure.

### Delivery Strategies for Low-Connectivity Contexts

- **Campus Wi-Fi / One-Time Seed**: Users download the ~1 GB offline payload once over a campus Wi-Fi connection or local community hotspot.
- **Local P2P / USB Sideload Plan**: For users without any broadband, SketchCode plans support for importing an offline model bundle (`sketchcode-models.tar`) directly from a USB-OTG drive or local shared folder via the HTML5 File System Access API.

---

## 4. Privacy Advantage for Independent Creators

Student developers, hackathon participants, and early entrepreneurs frequently sketch proprietary product ideas, innovative algorithms, or sensitive business models on paper.

- **No Prompt Logging**: Unlike cloud-based AI code assistants, prompts are not retained for model training.
- **No IP Leakage**: Intellectual property never transits third-party cloud infrastructure.
- **Red Light Feasibility**: Developers can prototype ideas in secure environments, flight cabins, or remote field locations with zero wireless footprint.
