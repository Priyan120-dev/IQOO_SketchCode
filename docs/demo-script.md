# SketchCode Grand Finale Demo and Pitch Script

3-minute live presentation script covering the problem statement, live phone demonstration, technical architecture, and vision.

**Author: Priyan**  
**Track: Developer Tools (iQOO Hackathon 2026 Grand Finale)**  
**Status: Presentation Script (Grand Finale Plan)**

---

## Pitch Structure Overview

- **Total Duration**: Exactly 3 minutes (180 seconds)
- **Presenter**: Priyan
- **Hardware Setup**: iQOO smartphone running SketchCode PWA + Paired Laptop running iQOO Office Kit
- **Connectivity State**: Phone visibly in **Airplane Mode** (Wi-Fi OFF, Cellular OFF) to prove 100% on-device execution

---

## Script Breakdown

### 0:00 – 0:45 | The Problem & The "Red Light" Moment

*(Presenter stands holding a regular paper notebook with a hand-drawn sketch and an iQOO phone)*

> "Respected judges, millions of young developers and students across India have great app ideas every single day. They sketch them on the back of notebooks during college lectures, on bus rides, or while waiting at a red light.
> 
> But right now, turning that paper sketch into a working prototype requires a laptop, a broadband connection, complex cloud toolchains, and expensive API keys that students simply don't have.
> 
> What if the only development machine you needed was already in your pocket?
> 
> This is **SketchCode**. Our motto is simple: **Sketch it. Say it. Run it.** 
> It is an on-device AI tool that turns a hand-drawn sketch and a voice command in Tamil or English into an interactive, running application preview—right on an iQOO phone, with zero internet, zero cloud dependencies, and zero monthly subscriptions."

---

### 0:45 – 1:45 | The Live Demonstration

*(Presenter mirrors the iQOO phone screen to the main hall projector. Presenter pulls down the quick settings shade to show Airplane Mode enabled).*

> "Notice my phone is in Airplane Mode. No cloud servers are helping us today.
> 
> **Step 1: The Sketch.**  
> Here is a rough sketch on notebook paper of a grocery checklist app called 'Grocery Quick'. I open SketchCode, point the camera, and snap a photo. In less than 200 milliseconds, on-device computer vision detects the rectangular input box, the action button, and the list container.
> 
> **Step 2: The Voice Command.**  
> Now, instead of writing complex logic, I hold down the microphone button and describe what I want in Tamil:
> 
> *'Oru grocery list app venum, item type panni Add click panna list la save aaganum.'*
> 
> Whisper-tiny runs locally on this device via Transformers.js, immediately transcribing the audio.
> 
> **Step 3: The Live App.**  
> I tap Generate. Our on-device language model running over WebGPU synthesizes the vision tokens and spoken intent into a clean, validated JSON UI specification. 
> 
> Look at the screen—the app is live! 
> 
> I can tap the input box, type 'Rice 5kg', tap '+ Add', and it immediately appends to our list. I can check off items. It's not a mockup screenshot; it's a real, interactive web application running right here on the phone."

---

### 1:45 – 2:30 | What the Judges Are Seeing (Under the Hood)

*(Presenter switches slide to the architecture block diagram)*

> "Why does this work reliably when other AI code generators crash on phones?
> 
> Small 1.5B parameter models are notoriously unreliable at writing free-form React or JavaScript code—they miss brackets and hallucinate imports.
> 
> So we invented a **constrained architectural pipeline**:
> 1. The local model is mathematically constrained by a formal grammar mask. It is only allowed to output our strict **SketchCode UI Schema**—a finite set of 7 verified components.
> 2. A deterministic client-side renderer turns that spec into live UI components. Zero syntax errors, zero arbitrary script execution, 100% crash-free.
> 
> And when you are ready to take this to production?
> 
> I tap **'Sync to Laptop'**. Using the **iQOO Office Kit** local bridge, the validated spec is compiled into standard React and Tailwind code, appearing instantly in VS Code on my paired laptop without touching any cloud repository."

---

### 2:30 – 3:00 | The Vision & Closing Line

*(Presenter steps forward with phone)*

> "SketchCode makes developer tools truly mobile-first, privacy-first, and native-language friendly. 
> 
> It empowers the student in a rural college hostel, the commuter on the train, and the developer working in Red Light moments to turn inspiration into working software within seconds.
> 
> No laptop required. No internet required.
> 
> **Sketch it. Say it. Run it.**
> 
> Thank you."

---

## Judge Q&A Anticipated Cheat Sheet

| Question | Planned Response |
| :--- | :--- |
| **Why not generate full React Native or Flutter code directly?** | Small on-device models (1.5B–2B) have high error rates on free-form code. Our constrained JSON spec guarantees a 100% executable UI with zero syntax failures. Full code export is generated deterministically when syncing to the laptop. |
| **How does it perform on non-English speech?** | We use multilingual Whisper-tiny, which handles Tamil phonetics effectively. For noisy environments, we provide 1-tap intent chips as a deterministic fallback. |
| **How does iQOO hardware give this an advantage?** | SketchCode relies heavily on Snapdragon Adreno GPU acceleration for WebGPU tensor arithmetic and iQOO Office Kit for seamless desktop handoff. |
