# Safety Goals and Functional Safety Requirements

**Document ID:** SG-FSR-01  
**Status:** v0.1 — Baselined FSR Set

## 1. Purpose

This document defines the current Safety Goals and associated Functional Safety Requirements for the vehicle-side charging system.

The Safety Goal classifications are based on the current HARA working results.

---

## 2. SG-01 — Unintended Energy Backfeed

**Safety Goal:**  
Prevent unintended backfeed of energy from the HV battery toward the external AC supply during AC charging.

**Working classification:** ASIL D

### 2.1 Functional Safety Requirements

**FSR-01.1**  
During an AC charging session, the charging system shall not permit net energy transfer from the HV battery toward the external AC charging source.

**FSR-01.2**  
During an AC charging session, the charging system shall detect net energy transfer from the HV battery toward the external AC charging source.

**FSR-01.3**  
Upon detection of net energy transfer from the HV battery toward the external AC charging source, the charging system shall interrupt that reverse energy transfer and transition to the SG-01 safe state within the applicable FTTI.

**FSR-01.4**  
Following interruption due to detection of unintended reverse energy transfer, the charging system shall prevent resumption of AC charging until safe charging conditions have been re-established.

**FSR-01.5**  
If safety-relevant information required to determine safe energy-flow conditions is unavailable, invalid, implausible, or stale, the charging system shall prevent continued AC charging and transition to the SG-01 safe state.

**FSR-01.6**  
Before enabling AC charging, the charging system shall establish a valid AC charging mode. If a valid AC charging mode cannot be established, energy transfer for AC charging shall not be permitted.

### 2.2 Safe State

A state in which hazardous net energy transfer from the HV battery toward the external AC charging source is prevented.

**FTTI:** TBD

---

## 3. SG-02 — HV Battery Overcharge

**Safety Goal:**  
Prevent a thermal event caused by overcharging of the HV battery.

**Working classification:** ASIL C

### 3.1 Functional Safety Requirements

**FSR-02.1**  
Charging of the HV battery shall only be permitted when valid battery charging permission is available and safe battery charging conditions have been established.

**FSR-02.2**  
During charging, the charging system shall monitor the actual battery charging voltage, actual battery charging current, battery temperature, and the currently permitted maximum charging voltage and maximum charging current.

**FSR-02.3**  
If information required to determine safe battery charging conditions becomes unavailable, invalid, implausible, or stale, the charging system shall prevent continued charging of the HV battery and transition to the SG-02 safe state.

**FSR-02.4**  
If the actual battery charging conditions violate the currently permitted battery charging limits or conditions, the charging system shall prevent continued charging outside the permitted charging envelope and transition to the SG-02 safe state within the applicable FTTI.

**FSR-02.5**  
Following interruption due to a violation of safe battery charging conditions, the charging system shall prevent resumption of charging until safe battery charging conditions have been re-established.

### 3.2 Safe State

A state in which continued charging cannot cause further hazardous overcharge of the HV battery.

**FTTI:** TBD
