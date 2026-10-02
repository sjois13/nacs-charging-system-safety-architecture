# Hazard Analysis and Risk Assessment (HARA)

**Project:** NACS Charging System & Safety Architecture  
**Status:** v0.1 — Work in Progress

## 1. Purpose

This document records the current Hazard Analysis and Risk Assessment for the vehicle-side charging system.

The HARA identifies hazardous events, evaluates Severity (S), Exposure (E), and Controllability (C), and derives the associated ASIL classification and Safety Goals.

---

## 2. HARA Table

| HE ID | Charging Mode | Malfunction | Operational Situation | Hazard | Potential Effect | S | E | C | ASIL | Safety Goal ID |
|---|---|---|---|---|---|---|---|---|---|---|
| HE-01 | AC charging | Net energy transfer from the HV battery toward the external AC supply | Vehicle stationary, connected to an AC EVSE, active charging session | Unexpected energization / reverse power into external AC infrastructure | Electrical or thermal injury resulting from abnormal energization, including electric shock, arc-related injury, or fire depending on external network and protection conditions | S3 | E4 | C3 | D | SG-01 |
| HE-02 | AC charging / vehicle connected | Net energy transfer from the HV battery toward the external AC supply | Vehicle connected while upstream AC supply is de-energized or isolated for maintenance / service | Unexpected energization of external AC conductors from the vehicle side | Electrical or thermal injury resulting from abnormal energization, including electric shock, arc-related injury, or fire depending on external network and protection conditions | S3 | E1 | C3 | A | SG-01 |
| HE-03 | DC charging | Net energy transfer from the HV battery toward the external DC charging source | DC fast-charging / connected condition; operational situation requires further refinement | Unexpected energization of the external DC charging source / conductors from the vehicle side | Electric shock or other electrical injury to a person exposed to conductors assumed to be de-energized | S3 | E2 | C3 | B | SG-03 (provisional) |
| HE-04 | AC charging | Charging energy continues beyond the battery-permitted charging limits or conditions | Vehicle stationary, connected to an AC EVSE, active charging session | HV battery overcharge causing excessive electrochemical / thermal stress and possible loss of thermal stability | Fire, smoke, hot-gas release, and resulting injury | S3 | E4 | C3 | D | SG-02 |
| HE-05 | DC charging | Charging energy continues beyond the battery-permitted charging limits or conditions | Vehicle stationary, connected to a DC fast charger, active charging session | HV battery overcharge causing excessive electrochemical / thermal stress and possible loss of thermal stability | Fire, smoke, hot-gas release, and resulting injury | S3 | E2 | C2 | A | SG-02 |

---

## 3. Current Safety Goals

| Safety Goal ID | Safety Goal | Working ASIL |
|---|---|---|
| SG-01 | Prevent unintended backfeed of energy from the HV battery toward the external AC supply during AC charging | D |
| SG-02 | Prevent a thermal event caused by overcharging of the HV battery | C |
| SG-03 | Prevent unintended backfeed of energy from the HV battery toward the external DC charging source during DC charging | Provisional / not yet baselined |

---

## 4. Open Issues

### HARA-OI-01 — HE-03 Operational Situation

The operational situation for HE-03 requires further refinement.

The hazardous event should distinguish normal DC charging from a condition involving de-energized or maintenance-related external DC conductors.

**SG-03 remains provisional.**

### HARA-OI-02 — HE-04 / SG-02 ASIL Inconsistency

HE-04 is currently assessed as:

**S3 / E4 / C3 → ASIL D**

SG-02 is currently carried as:

**ASIL C**

This inconsistency must be resolved before the HARA is baselined.

The current engineering direction is to review whether **C2** can be technically justified. If not, the SG-02 classification must be reconsidered.

### HARA-OI-03 — Exposure Rationale

Each Exposure rating requires a rationale based on the frequency of the defined operational situation rather than the probability of the malfunction occurring.

### HARA-OI-04 — Controllability Rationale

Each Controllability rating requires a rationale describing what an exposed person or vehicle user can realistically do once the hazardous situation develops.

### HARA-OI-05 — Severity Rationale

The current S3 assessments assume that the hazardous events can plausibly lead to severe or life-threatening injury.

The causal chain between the malfunction, hazardous condition, and potential harm must remain explicit. Backfeed should not be assumed to automatically result in electric shock, arc-related injury, or fire under every external-network condition.
