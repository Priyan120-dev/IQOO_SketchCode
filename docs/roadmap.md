# SketchCode Roadmap and 48-Hour Hackathon Build Plan

Phased development milestones, definition of done criteria, and 48-hour Grand Finale execution plan.

**Author: Priyan**  
**Track: Developer Tools (iQOO Hackathon 2026 Grand Finale)**  
**Status: Forward-Looking Roadmap & Plan (No product code pre-event)**

---

## 1. Project Milestones

```
[Milestone 1: Spec & Renderer] ---> [Milestone 2: Vision & Voice] ---> [Milestone 3: Office Kit Bridge]
```

### Milestone 1: Core Spec Schema & Deterministic Renderer
- **Scope**:
  - Implement the JSON UI Specification validation engine (7 core component primitives).
  - Build the deterministic React component dictionary (screens, navigation, inputs, lists, buttons).
  - Implement the in-memory state store for user interactions (adding items, toggling checkboxes, navigating between views).
- **Definition of Done (DoD)**:
  - Feeding any valid `ui-spec.json` file into the renderer immediately displays a responsive, interactive mobile interface.
  - State mutations (e.g., typing into an input field and clicking "+ Add") append items without page reloads.
  - Schema validation catches malformed output; invalid output falls back to safe defaults without runtime crashes.

### Milestone 2: Multimodal Perception (Camera OCR + Whisper Speech)
- **Scope**:
  - Implement HTML5 Camera capture with real-time perspective correction and contrast binarization.
  - Integrate Tesseract.js WASM worker to detect rectangular component bounds and extract handwritten text labels.
  - Integrate quantized Whisper-tiny via Transformers.js in a dedicated Web Worker to transcribe spoken English (primary) and Tamil (experimental) voice commands into intent strings.
- **Definition of Done (DoD)**:
  - Photographing a hand-drawn sketch correctly isolates at least 3 distinct UI bounding boxes on an iQOO phone screen.
  - Speaking a voice command in English or Tamil produces usable intent text in under 3.5 seconds, with 1-tap intent chips available as fallback.
  - Visual and audio pipelines execute entirely offline after initial model weight download.

### Milestone 3: On-Device LLM Synthesis & Office Kit Bridge Sync (Planned)
- **Scope**:
  - Connect WebLLM with quantized 1.5B/2B parameter models (e.g., Qwen 2.5 1.5B or Gemma 2 2B) running via WebGPU shaders (baseline).
  - Implement grammar constraints and the self-healing AST parser to format LLM output directly into the validated UI spec.
  - Build the export module to package the validated spec into clean React + Tailwind source code and validate integration with the iQOO Office Kit bridge.
- **Definition of Done (DoD)**:
  - End-to-end run: Camera photo + voice audio generates a live interactive preview in a single pass on phone hardware.
  - Tapping "Sync to Laptop" validates project packaging and initiates transfer via Office Kit bridge (with local directory/zip download as proven fallback).

---

## 2. 48-Hour Grand Finale Build Plan

This planned schedule organizes the 48-hour Grand Finale window into five distinct technical phases.

| Phase | Time Window | Focus Area | Deliverables (Planned) |
| :--- | :--- | :--- | :--- |
| **Phase 1: Foundations** | Hours 00 – 10 | Schema & Deterministic Engine | • Setup Vite PWA template and service worker shell.<br/>• Build component dictionary (`Navbar`, `Button`, `Input`, `List`, `Screen`).<br/>• Build AST validator and safe state management store. |
| **Phase 2: Vision System** | Hours 10 – 20 | Camera & OCR Pipeline | • Camera viewfinder integration via Web MediaStream.<br/>• Canvas edge detection and contour extraction for UI boxes.<br/>• Tesseract.js WASM background worker pipeline. |
| **Phase 3: Speech Worker** | Hours 20 – 30 | On-Device Whisper Audio | • Audio capture via Web Audio API (16kHz PCM downsampling).<br/>• Transformers.js Whisper-tiny worker (English primary, Tamil experimental).<br/>• Real-time speech transcription card and fallback intent chips. |
| **Phase 4: Local LLM Engine** | Hours 30 – 40 | WebGPU Synthesis & Repair | • WebLLM initialization with quantized 1.5B/2B model weights.<br/>• Context prompt assembly (combining bounding boxes + voice intent).<br/>• Self-healing JSON syntax repair and validation loop. |
| **Phase 5: Bridge & Polish** | Hours 40 – 48 | Office Kit Sync & Demo Prep | • Project packager (React + Tailwind boilerplate generator).<br/>• Planned: sync project files via Office Kit (to be validated) + zip export fallback.<br/>• Rehearse 3-minute pitch and live demo flow. |
