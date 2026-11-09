# Phase 1 & 2 Deliverable: Opportunity Scan & Quick Problem Cards
**Unit:** Vin Smart Future — Vingroup  
**Author / Contributor:** AI Product Engineering Team  

---

## 🏛️ Context & Background
As AI Product Engineers at **Vin Smart Future**, our mandate is to identify operational bottlenecks across Vingroup subsidiaries (VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl) and scope high-impact, operationally safe AI features.

---

# 🔍 Phase 1 — SCAN: Operational Opportunity Table

Using the **4 Lenses of AI Scoping** (Repetitive, Time-consuming, AI-upgrade, Stakeholder Pain), we mapped the following 6 real-world operational challenges:

| # | Subsidiary | Operational Problem / Bottleneck | Scoping Lens | Current Pain & Evidence |
|---|------------|-----------------------------------|--------------|-------------------------|
| 1 | **Xanh SM (GSM)** | Real-time emergency battery depletion triage & dispatching | Time-consuming | Drivers running out of battery call dispatchers; dispatchers manually cross-check GPS coordinates, VinFast charging slot availability, and compose routing instructions (takes 12–15 min/incident). |
| 2 | **Vinhomes** | Resident complaint & service ticket classification/routing | Repetitive | CSKH staff manually read and route >1,200 daily requests (water leaks, elevator noise, parking) across 30+ urban sub-areas; responses take up to 12 hours. |
| 3 | **VinFast** | Partner charging station invoice reconciliation | Repetitive | Finance teams manually audit weekly energy consumption logs against third-party partner charging station bills across 3,000+ charging points. |
| 4 | **Vinmec** | Clinical discharge summary drafting from electronic medical records | Time-consuming | Physicians spend 20–30 minutes per patient summarizing lab test histories, surgery notes, and prescription advice into patient-friendly discharge letters. |
| 5 | **Vinpearl** | Multi-channel guest sentiment & critical review alerts | Stakeholder Pain | Guest service managers manually aggregate daily reviews from Agoda, Booking.com, and Google Maps to flag urgent sanitation or service complaints. |
| 6 | **VinFast** | EV acoustic anomaly & fault symptom classification | AI-upgrade | Vehicle service centers receive vague customer descriptions (e.g., "rattling noise under front axle during deceleration") requiring lengthy manual diagnostic triage. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

From the scan above, we selected the top 3 high-leverage opportunities:

### Quick Problem Card #1 (Selected for Deep-Dive)
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Problem: Xanh SM drivers reporting critical battery depletion mid-shift      │
│          require immediate triage, station routing, or mobile rescue dispatch│
│ Subsidiary: [x] Xanh SM (GSM)                                               │
│                                                                             │
│ Actor: Dispatcher at Hanoi/HCMC Central Operations Center                   │
│                                                                             │
│ Current Manual Workflow (5 steps):                                          │
│   1. Driver calls hotline reporting low battery / impending shutdown       │
│   → 2. Dispatcher queries vehicle telemetry for current GPS & battery state  │
│   → 3. Dispatcher checks VinFast station dashboard for vacant compatible plug│
│   → 4. Dispatcher manually drafts turn-by-turn routing text and sends SMS   │
│   → 5. Dispatcher calls mobile rescue charging team if battery < 5%         │
│                                                                             │
│ Most time-consuming / error-prone step: Steps 3 & 4 (⏱ 10–12 min/incident)   │
│ Where AI can step in: Steps 3 & 4 (Parse telemetry -> query nearest vacant   │
│                       plug -> draft driver guidance with safety guardrails)  │
│                                                                             │
│ Success Metric:                                                             │
│   - Reduce end-to-end incident handling time from 15 min down to < 3 min     │
│   - Zero incidents of vehicles dying en route due to invalid station routing│
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Co-pilot with strict draft approval)    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #2
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Problem: Inefficient manual triaging of resident maintenance requests        │
│          submitted via the Vinhomes Resident Mobile Application.             │
│ Subsidiary: [x] Vinhomes                                                    │
│                                                                             │
│ Actor: Front-desk Customer Support Agent & Building Management Staff        │
│                                                                             │
│ Current Manual Workflow (4 steps):                                          │
│   1. Resident submits unstructured text/photo complaint in app              │
│   → 2. Support agent reads ticket and classifies category (Plumbing/Electric)│
│   → 3. Agent checks technician roster and assigns ticket to building team    │
│   → 4. Agent types confirmation SMS/notification back to resident           │
│                                                                             │
│ Most time-consuming / error-prone step: Step 2 & 3 (⏱ 8 min/ticket; backlog) │
│ Where AI can step in: Step 2 (Automated classification and priority scoring)│
│                                                                             │
│ Success Metric:                                                             │
│   - Ticket first-response SLA decreased from 12 hours to under 15 minutes    │
│   - Routing accuracy to appropriate maintenance unit > 95%                  │
│                                                                             │
│ Quick Architecture: [x] Rule-based classifier + Simple LLM intent router    │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #3
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Problem: Vinmec physicians spend excessive time synthesizing complex        │
│          EMR records into layperson-friendly discharge summaries.           │
│ Subsidiary: [x] Vinmec                                                      │
│                                                                             │
│ Actor: Attending Physician & Inpatient Ward Nurse                           │
│                                                                             │
│ Current Manual Workflow (4 steps):                                          │
│   1. Physician reviews multi-day lab test results, imaging notes, and vitals│
│   → 2. Physician dictates or types discharge medical summary into HIS portal │
│   → 3. Physician manually translates clinical jargon into home-care advice   │
│   → 4. Nurse prints document and verbally explains medication schedules      │
│                                                                             │
│ Most time-consuming / error-prone step: Step 2 & 3 (⏱ 25 min/patient)        │
│ Where AI can step in: Step 3 (Drafting clear, patient-friendly guidance from │
│                       verified clinical EHR structured outputs)              │
│                                                                             │
│ Success Metric:                                                             │
│   - Physician discharge summary drafting time reduced from 25 min to < 5 min │
│   - Zero medical inaccuracies (100% physician sign-off required)             │
│                                                                             │
│ Quick Architecture: [x] LLM Feature with mandatory doctor verification       │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Project Selection Decision & Deprioritization Rationale

The team unanimously selected **Card #1 (Xanh SM Real-time Emergency Battery Depletion Co-pilot)** for full engineering deep-dive:

1. **Urgency & Operational Impact:** Unlike offline back-office tasks, real-time EV stalling directly affects driver earnings, traffic flow, passenger safety, and vehicle asset longevity.
2. **Clear Operational Boundaries:** High stakes mean boundaries are crisp and testable (e.g., if battery < 5%, never dispatch to stations > 5km; mandatory `[DRAFT_ONLY]` to enforce human dispatch review).
3. **Why Cards #2 and #3 were deferred:**
   - *Card #2 (Vinhomes):* Primarily a routing problem where rule-based algorithms with basic NLP classification suffice; high LLM usage would introduce unnecessary latency and inference costs.
   - *Card #3 (Vinmec):* Medical safety regulations and EHR integration require lengthy medical board review and compliance cycles beyond the rapid prototyping scope of this sprint.

