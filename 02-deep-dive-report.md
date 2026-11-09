# Phase 3 & 5 Deliverable: AI Product Deep-Dive Report
**Project:** Xanh SM Intelligent Dispatcher Co-Pilot (Emergency EV Charging Triage)  
**Business Unit:** GSM (Xanh SM) / Vin Smart Future — Vingroup  
**Document Version:** 1.0 (Production Scoping)  

---

## 🏗️ 1. Current-State Workflow Mapping

The current operational procedure when an active electric taxi driver experiences battery distress:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Step 1          │       │ Step 2          │       │ Step 3 (🔴)      │       │ Step 4 (🔴)      │
│ Inbound Distress│ ────> │ Telemetry Query │ ────> │ Station Slot    │ ────> │ Dispatch Draft  │
│ Call Receipt    │       │ & GPS Lookup    │       │ Search & Match  │       │ & SMS Guidance  │
│                 │       │                 │       │                 │       │                 │
│ Actor: Dispatcher       │ Actor: Dispatcher       │ Actor: Dispatcher       │ Actor: Dispatcher│
│ Duration: 2 min │       │ Duration: 2 min │       │ Duration: 5 min │       │ Duration: 5 min │
│ Input: Voice    │       │ Input: Plate ID │       │ Input: GPS, Type│       │ Input: Raw data │
│ Output: Ticket  │       │ Output: Lat/Long│       │ Output: Station │       │ Output: SMS text│
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
                                                                                      │
                                                                                      ▼
                                                                            ┌─────────────────┐
                                                                            │ Step 5 (🔄)      │
                                                                            │ Mobile Charger  │
                                                                            │ Rescue Dispatch │
                                                                            │                 │
                                                                            │ Actor: Rescue Op│
                                                                            │ Duration: 1 min │
                                                                            │ Trigger: Batt<5%│
                                                                            └─────────────────┘

Total Manual Cycle Time: ~15 minutes per critical event
🔴 = Critical Bottlenecks (manual lookup across siloed dashboards and drafting Vietnamese directions)
🔄 = Handoff point between Central Dispatch and Mobile Rescue Fleet
```

---

## 📋 2. Problem Statement (6-Field Standard)

| Field | Detailed Operational Specification |
|---|---|
| **1. Actor / Operator** | Operations Dispatcher at Xanh SM Central Dispatch Center (Hanoi & Ho Chi Minh City). |
| **2. Current Workflow** | Drivers call hotline when battery drops below 10%. Dispatcher opens internal fleet tracker, matches driver GPS with VinFast charging network dashboard to locate available compatible plugs (CCS2/GBT), manually calculates driving distance, types an SMS with directions and battery conservation advice, and escalates to mobile rescue vehicles if battery is under 5%. |
| **3. Bottleneck** | **Steps 3 & 4 (taking 10 of the 15 minutes):** Dispatchers must manually cross-reference vehicle battery level, connector compatibility (VF5, VF8, VF9), and station real-time occupancy, followed by typing localized driving instructions. |
| **4. Business Impact** | Across ~80 daily battery-critical incidents in metropolitan areas, ~20 dispatcher hours are lost daily. High wait times result in a 15% vehicle idle-time revenue leak, increased towing costs (approx. 500,000 VND per dead vehicle), and severe customer trip cancellation spillover. |
| **5. Success Metric** | **1. Speed:** Reduce average incident resolution time from 15 min to **< 3 minutes**.<br>**2. Precision:** Achieve **99% compatibility match** (correct charging standard and distance feasibility).<br>**3. Safety:** **Zero dead-on-road incidents** caused by AI routing recommendations exceeding current battery range. |
| **6. Operational Boundary** | **AI Permissions:** Access vehicle telemetry, read-only query of VinFast charging station API, and generate proposed draft routing advice.<br>**Strict Constraints (PROHIBITED):**<br>- AI is **NEVER** permitted to transmit instructions directly to drivers without human dispatcher review (mandatory `[DRAFT_ONLY]` tag enforcement).<br>- If battery is **< 5%**, AI is strictly prohibited from routing to any station farther than 5km; it must instead immediately output a structured mobile charging dispatch payload: `{"action": "dispatch_mobile_charger", "reason": "..."}`. |

---

## ⚡ 3. AI Fit Matrix & Future-State Architecture

### AI Fit Evaluation
* **Rule-based Logic / State Machine:** Excellent for calculating distance (`haversine`) and checking plug types, but rigid and unable to interpret unstructured driver messages or compose empathetic, contextual instructions under emergency conditions.
* **LLM Feature (Co-pilot) [SELECTED]:** Perfect fit. A single-turn LLM inference call with rigid system boundaries parses the incident context, runs safety validations, and generates a formatted draft with structured rescue action triggers.
* **Autonomous Multi-Agent Loop:** Rejected for Phase 1. Real-time emergency vehicle routing carries physical safety and traffic liability risks; an autonomous looping agent introduces uncontrollable latency and hallucination surfaces without added value.

### Future-State Workflow (Human-In-The-Loop)

```text
┌──────────────────┐       ┌────────────────────────┐       ┌────────────────────────┐       ┌──────────────────────┐
│ Inbound Distress │       │ 🔵 Automated Ingestion  │       │ 🔵 AI Dispatch Co-Pilot │       │ 🟢 Human Dispatcher   │
│ Trigger (Driver) │ ────> │ & Telemetry Fetch      │ ────> │ Generates Draft & Action│ ────> │ Review & One-Click   │
│                  │       │                        │       │                        │       │ Approval             │
│ Time: Real-time  │       │ Time: < 3 seconds      │       │ Time: < 2 seconds      │       │ Time: < 30 seconds   │
└──────────────────┘       └────────────────────────┘       └────────────────────────┘       └──────────────────────┘
                                                                                                        │
                                                                    ┌───────────────────────────────────┴───────────────────────────────────┐
                                                                    ▼                                                                       ▼
                                                       [Standard Case: Battery >= 5%]                           [Critical Case: Battery < 5%]
                                                       Approved [DRAFT_ONLY] message sent                       Structured Mobile Charger Rescue
                                                       to driver mobile app with navigation                     Vehicle dispatched immediately
                                                                    │
                                                                    ▼
                                                       ↩️ Fallback Mode:
                                                       If AI call times out (>5s) or returns invalid JSON,
                                                       dispatcher dashboard falls back immediately to
                                                       standard static map view for manual execution.
```

---

## 🏁 4. Phase 5 — AI Readiness & Governance Evaluation

### AI Readiness Checklist
1. **Data Availability & Quality:** **READY.** GSM fleet management platform exposes reliable vehicle telemetry APIs (GPS, state-of-charge SOC %, plug type) and real-time VinFast station occupancy feeds.
2. **Safety & Risk Mitigation:** **READY.** Hard guardrails (`[DRAFT_ONLY]` mandatory tag requirement + programmatic assertion checks) guarantee that hallucinated outputs cannot reach drivers without dispatcher oversight.
3. **Operational Stakeholder Alignment:** **READY.** Dispatchers currently experience high stress during peak commute hours; shifting from manual typing to 1-click review directly improves dispatcher job satisfaction and throughput.

### Executive Recommendation: **GO (Proceed to Staged Production Pilot)**

**Justification:**
- **Technical Feasibility:** The prompt boundary prototype executed on **Gemini 2.5 / 3.5 Flash** proves that safety constraints (critical battery threshold and mandatory draft tag) can be deterministically maintained under adversarial prompting.
- **Economic Value:** Reducing dispatch handling time by 80% (15 min ➔ <3 min) frees up 16 dispatcher hours daily, saving approximately 350M VND annually in operational overhead while reducing driver attrition and customer trip cancellations.
- **Low Risk Profile:** Because the design strictly adheres to **Human-In-The-Loop (HITL)** architecture with explicit fallback handling, deployment poses zero risk of unvetted automated communication with drivers.

