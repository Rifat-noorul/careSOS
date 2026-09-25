# Maternal CareSOS — Core Triage & Dispatch Engine (Prototype POC)

> **Submission Track:** Maternal & Child Health (FOGSI)  
> **Core Focus:** Doctor / Care Team Workflow Automation (Strictly Non-Clinical)  
> **Repository:** Event-Driven Transit Telemetry, SBAR Orchestration & Emergency Pre-Alert Pipeline

---

## 1. Prototype Overview & Video Walkthrough

The uploaded prototype video demonstrates the **functional proof-of-concept (POC) for our core event-driven dispatch and alerting architecture**. 

Obstetric transit emergencies require sub-second reliability, offline tolerance, and instantaneous notification before cellular dead-zones can drop the connection. To prove this reliability before building the high-level application layer, we engineered and verified the low-latency relay pipeline using physical edge nodes.

### What is demonstrated in the video:
1. **Edge-Level Trigger (Instant Interrupt):** Physical simulation of an emergency distress event triggering an immediate hardware interrupt rather than relying on delayed polling.
2. **Low-Latency Mesh Relay (ESP-NOW / P2P):** Rapid packet routing across localized edge nodes to prove data survival in zero-infrastructure or poor connectivity corridors.
3. **Gateway Pre-Arrival Alert:** The gateway node instantly parses incoming distress metadata and pushes an audible, visual high-priority alert to the receiver dashboard in under 3 seconds.

---

## 2. Architecture Mapping: Hardware POC to Maternal CareSOS

The underlying "Sense $\rightarrow$ Route $\rightarrow$ Pre-Alert" engineering methodology verified in our physical testbed maps directly to the maternal healthcare workflow:

| Prototyped Hardware Layer (In Video) | Production Healthcare Implementation (Maternal CareSOS) |
| :--- | :--- |
| **Edge Sensor Interrupt** | Frontline 1-tap distress trigger / Vernacular voice capture (ASHA & 108 EMT) |
| **P2P Mesh Data Transmission** | Low-bandwidth offline-first PWA sync + lightweight JSON transit payloads (<100 KB) |
| **Gateway Logic & Filtering** | LangChain metadata parser structuring raw input into standardized SBAR format |
| **Receiver Warning Alarm** | Hospital Emergency Board WebSocket alert + Duty OB-GYN & Blood Bank pre-arrival notification |

---

## 3. Technology Stack

* **Frontline Client:** React Progressive Web App (PWA), Tailwind CSS (Optimized for low-tier mobile devices).
* **Voice Transcription:** Sarvam AI Indic Speech-to-Text API (regional dialect support for Tamil, Hindi, etc.).
* **Handover Orchestration:** Node.js, Express, LangChain (maps transit vitals to Situation-Background-Assessment-Recommendation briefs).
* **Hospital Readiness & State:** Supabase (PostgreSQL), WebSockets for real-time ER triage boards.
* **Emergency Dispatch Fallback:** WhatsApp Business API webhook automation for duty doctors.
* **Edge Testbed (Demonstrated):** Dual ESP32 microcontroller nodes, 2.4 GHz ESP-NOW protocol, shock/acoustic interrupt sensors.

---

## 4. Repository Structure

```text
├── /ambulance-client       # React PWA for frontline ASHAs / EMTs
│   ├── src/components      # 1-Tap Voice recorder & basic vitals input
│   └── src/services        # Offline-first IndexedDB caching & sync
├── /orchestration-engine   # Node.js + LangChain backend
│   ├── routes/transcribe   # Sarvam AI STT ingestion pipeline
│   ├── routes/sbar         # Voice-to-SBAR structured JSON parser
│   └── routes/readiness    # Real-time hospital OT/blood bank matcher
├── /hospital-dashboard     # Real-time ER triage view for duty OB-GYNs
│   └── src/alerts          # WebSocket receiver for audible incoming alarms
└── /hardware-poc           # C++/Arduino firmware for the mesh testbed shown in video
    ├── node_transmitter    # Edge interrupt simulation
    └── gateway_receiver    # Dashboard relay script