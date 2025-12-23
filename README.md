# AI-Enabled Pull Production (JIT)  
### ANN-Based Cycle Time Prediction + Explainable Fuzzy Decision System

This repository implements an **industry-style decision support system** for **Pull Production / Just-In-Time (JIT)** manufacturing.  
The system combines **predictive AI (ANN)** with **explainable fuzzy logic** to decide **when and how much to release into production**, while clearly explaining *why* each decision is made and *what action to take*.

---

## 🔍 Problem Statement
Traditional JIT and Kanban systems struggle in real factories due to:
- Uncertain processing times
- WIP congestion and bottlenecks
- Quality rework and failures
- Demand urgency changes

Rigid rules (fixed Kanban limits or averages) often fail under these conditions.

---

## 🎯 Solution Overview
This project uses a **two-layer AI architecture**:

### 1️⃣ Predictive Layer — ANN (Artificial Neural Network)
- Predicts **cycle time** for each production order using MES-like data
- Learns non-linear effects of:
  - WIP
  - Machine & process conditions
  - Product/workstation characteristics
- Output:  
  `ann_predicted_cycle_time_min`

### 2️⃣ Prescriptive Layer — Explainable Fuzzy Logic
- Converts predictions + shop-floor context into a **Pull Score (0–100)**
- Generates actionable decisions:
  - **No Pull**
  - **Partial Pull**
  - **Full Pull**
- Provides:
  - Human-readable **decision reason**
  - Practical **recommended action** for operators/planners

---

## 🧠 Decision Logic (High Level)

Each order starts with a **base score = 50**, then:

### Penalties (risk control)
- **High WIP** → strong penalty
- **Long / very long predicted cycle time** → penalty
- **Rework** → penalty
- **Fail** → hard block (score = 0)

### Bonuses (flow enablement)
- **High demand urgency** → boost
- **Fast predicted cycle time** → bonus
- **Very low WIP** → bonus

### Final Mapping
| Pull Score | Decision |
|-----------|----------|
| 0 – 34 | No Pull |
| 35 – 74 | Partial Pull |
| 75 – 100 | Full Pull |

This mirrors how real factories balance **flow, risk, and quality**.

---

## 📊 Outputs (Explainable & Auditable)

The final output file includes:

- `pull_score_0_100`  
- `fuzzy_predicted_pull_decision`  
- `decision_reason` – *why this decision was made*  
- `recommended_action` – *what to do next*

