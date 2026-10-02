# Functional Safety Concept

**Document ID:** FSC-01  
**Status:** v0.2 — Functional Baseline

## 1. Purpose

This document defines the Functional Safety Concept for the vehicle-side charging system.

The concept translates the established Safety Goals and Functional Safety Requirements into functional responsibilities, interactions, supervision, safe-state behavior, and restart control.

The final hardware, software, ECU, sensor, communication, and isolation implementation is not defined at this level.

---

## 2. Functional Safety Architecture

The functional safety architecture contains the following functional blocks:

- Session / Mode Qualification
- Charging Path / Mode Control
- Battery Charging Permission / Limit Interface
- AC Charging Power Control
- DC Charging Coordination
- AC Power-Flow Direction Determination
- Charging Safety Supervision
- Safety Decision
- Safe-State Execution
- Safe-State Achievement Verification
- Backup Safe-State Execution
- Restart Permissibility / Verification

### 2.1 Functional Architecture Diagram

The functional safety architecture separates normal charging functions, safety supervision, and safe-state/recovery functions.

```mermaid
flowchart TB

    %% ===== INPUTS =====
    subgraph INPUTS["① Inputs"]
        direction LR
        EXT["External Charging<br/>Interface"]
        BMS["External BMS"]
    end

    %% ===== NORMAL CHARGING =====
    subgraph NORMAL["② Normal Charging"]
        direction TB
        MODE["Session / Mode<br/>Qualification"]
        LIMIT["Battery Charging<br/>Permission / Limits"]
        PATH["Charging Path /<br/>Mode Control"]
        AC["AC Charging<br/>Power Control"]
        DC["DC Charging<br/>Coordination"]
    end

    BAT["HV Battery"]

    %% ===== SAFETY SUPERVISION =====
    subgraph MONITOR["③ Safety Supervision"]
        direction TB
        FLOW["AC Power-Flow<br/>Determination"]
        SUP["Charging Safety<br/>Supervision"]
        DEC["Safety Decision"]
    end

    %% ===== SAFE STATE & RECOVERY =====
    subgraph RECOVERY["④ Safe State & Recovery"]
        direction TB
        SAFE["Safe-State<br/>Execution"]
        VERIFY["Safe-State Achievement<br/>Verification"]
        BACKUP["Backup Safe-State<br/>Execution"]
        RESTART["Restart Permissibility /<br/>Verification"]
    end

    %% ===== NORMAL FLOW =====
    EXT --> MODE
    BMS --> LIMIT

    MODE --> PATH

    PATH -->|AC_VALID| AC
    PATH -->|DC_VALID| DC

    LIMIT --> AC
    LIMIT --> DC

    AC --> BAT
    DC --> BAT

    %% ===== SUPERVISION =====
    EXT --> FLOW
    FLOW --> SUP

    MODE --> SUP
    LIMIT --> SUP
    BAT --> SUP

    SUP --> DEC

    %% ===== SAFETY REACTION =====
    DEC -->|SAFE_STATE_REQUEST| SAFE

    SAFE -.->|Inhibit path| PATH
    SAFE -.->|Inhibit AC charging| AC
    SAFE -.->|Inhibit DC charging| DC

    SAFE --> VERIFY

    VERIFY -->|Confirmed| RESTART
    VERIFY -->|Not confirmed| BACKUP

    BACKUP --> VERIFY

    RESTART -.->|RESTART_PERMITTED| MODE

    %% ===== STYLING =====
    classDef input fill:#f2f2f2,stroke:#666,stroke-width:1.5px
    classDef normal fill:#e8f1fb,stroke:#3d6f9e,stroke-width:1.5px
    classDef monitor fill:#fff0d8,stroke:#b87516,stroke-width:1.5px
    classDef recovery fill:#eee6f8,stroke:#72509b,stroke-width:1.5px
    classDef battery fill:#e8f5e9,stroke:#4f7d53,stroke-width:1.5px

    class EXT,BMS input
    class MODE,LIMIT,PATH,AC,DC normal
    class FLOW,SUP,DEC monitor
    class SAFE,VERIFY,BACKUP,RESTART recovery
    class BAT battery

    style INPUTS fill:#fafafa,stroke:#bbb,stroke-dasharray:4 3
    style NORMAL fill:#f4f8fd,stroke:#9bbbd9
    style MONITOR fill:#fffaf0,stroke:#e0b66e
    style RECOVERY fill:#f8f4fc,stroke:#b9a0d6
```

**Figure 1 — Functional Safety Architecture**

Source: [`02_functional_safety_architecture.mmd`](./diagrams/02_functional_safety_architecture.mmd)
## 3. Functional Responsibilities

### 3.1 Session / Mode Qualification

Determines the charging mode from available charging-session and electrical-interface information.

Functional outputs are:

- `AC_VALID`
- `DC_VALID`
- `UNKNOWN`
- `CONFLICT`

The resulting mode is used by the charging-path and safety functions to determine whether charging may be permitted.

---

### 3.2 Charging Path / Mode Control

Controls which charging path is permitted according to the established charging mode.

The function inhibits charging paths when required by the safety concept or when the charging mode is not valid.

---

### 3.3 Battery Charging Permission / Limit Interface

Receives battery-side information required to determine whether charging may continue.

Relevant information includes:

- `CHARGE_ALLOWED`
- `V_CHARGE_MAX`
- `I_CHARGE_MAX`
- associated validity / status information

The detailed communication implementation is defined at the technical architecture level.

---

### 3.4 AC Charging Power Control

Controls the vehicle-side AC power-conversion function during AC charging.

For the SG-01 functional concept, the intended charging-energy direction is:

**External AC source → HV battery**

Reverse energy transfer toward the external AC source is not permitted during AC charging.

---

### 3.5 DC Charging Coordination

Coordinates vehicle-side DC charging with the external DC charger.

The external DC charger performs the high-power regulation.

The vehicle provides permitted or requested charging limits and supervises actual charging behavior against the battery charging permission and limits.

---

### 3.6 AC Power-Flow Direction Determination

Determines the direction of real / net AC power flow using AC-side voltage and current information.

Conceptually:

`P = average(v(t) × i(t))`

Functional outputs may include:

- `GRID_TO_VEHICLE`
- `VEHICLE_TO_GRID`
- `UNKNOWN`

This function provides the primary functional evidence used to identify energy export toward the external AC source.

---

### 3.7 Charging Safety Supervision

Evaluates charging-related information and detects conditions relevant to the Safety Goals.

Supervised conditions include:

- `LIMIT_VIOLATION`
- `MODE_MISMATCH`
- `CHARGE_NOT_ALLOWED`
- `SAFETY_INFORMATION_INVALID`
- `REVERSE_POWER_FLOW`

For SG-02, the supervision function evaluates:

- actual battery charging voltage
- actual battery charging current
- battery-temperature information
- currently permitted maximum charging voltage
- currently permitted maximum charging current
- charging permission
- validity and status of safety-relevant information

---

### 3.8 Safety Decision

Evaluates safety-supervision results and determines whether transition to the charging safe state is required.

When required, the function issues:

`SAFE_STATE_REQUEST`

---

### 3.9 Safe-State Execution

Receives `SAFE_STATE_REQUEST` and establishes a condition in which the relevant charging-energy transfer is prevented.

The implementation mechanism is not defined at the functional architecture level.

---

### 3.10 Safe-State Achievement Verification

Determines whether the commanded charging safe state has actually been achieved.

Failure to confirm the safe state may trigger backup safe-state execution.

---

### 3.11 Backup Safe-State Execution

Provides a candidate alternate functional means of establishing the charging safe state when primary safe-state execution cannot be confirmed.

The technical implementation and independence of the primary and backup paths are not established at this stage.

---

### 3.12 Restart Permissibility / Verification

Determines whether charging may resume following a fault or safe-state transition.

Removal of the original fault indication alone is not sufficient to permit restart.

Relevant safe charging conditions must first be re-established.

---

## 4. SG-01 Functional Safety Concept

**SG-01:**  
Prevent unintended backfeed of energy from the HV battery toward the external AC supply during AC charging.

**Working classification:** ASIL D

The SG-01 concept combines prevention, detection, safe-state execution, verification, and controlled restart.

### 4.1 Prevention

```mermaid
flowchart TB

    EXT["External AC<br/>Charging Source"]
    MODE["AC Mode<br/>Qualification"]
    PATH["AC Charging<br/>Path Control"]
    CTRL["AC Charging<br/>Power Control"]
    BAT["HV Battery"]

    MEAS["AC Interface<br/>Voltage + Current"]
    FLOW["AC Power-Flow<br/>Direction Determination"]
    SUP["Charging Safety<br/>Supervision"]
    DEC["Safety Decision"]
    SAFE["Safe-State<br/>Execution"]

    EXT --> MODE
    MODE -->|AC_VALID| PATH
    PATH --> CTRL
    CTRL --> BAT

    EXT --> MEAS
    MEAS --> FLOW
    FLOW -->|GRID_TO_VEHICLE / VEHICLE_TO_GRID / UNKNOWN| SUP

    MODE -->|Mode status| SUP

    SUP -->|REVERSE_POWER_FLOW| DEC
    DEC -->|SAFE_STATE_REQUEST| SAFE

    SAFE -.->|Inhibit AC path| PATH
    SAFE -.->|Inhibit energy transfer| CTRL
```

**Figure 2 — SG-01 Backfeed Prevention and Detection**

Source: [`03_sg01_backfeed.mmd`](./diagrams/03_sg01_backfeed.mmd)

Before AC charging is permitted, a valid AC charging mode is established.

The charging path and AC power-conversion function are configured for AC charging.

During AC charging, the permitted charging-energy direction is:

**External AC source → HV battery**

The functional architecture does not intentionally permit energy transfer from the HV battery toward the external AC charging source.

---

### 4.2 Reverse Power-Flow Detection

AC-side voltage and current information are evaluated by AC Power-Flow Direction Determination.

A `VEHICLE_TO_GRID` result indicates net energy transfer from the vehicle toward the external AC source.

Charging Safety Supervision uses this result to detect `REVERSE_POWER_FLOW`.

---

### 4.3 Invalid Safety Information

If information required to determine safe energy-flow conditions becomes unavailable, invalid, implausible, or stale, Charging Safety Supervision treats the charging condition as unsafe.

Continued AC charging is then inhibited through the common safety-decision and safe-state path.

---

### 4.4 SG-01 Fault Reaction

The functional reaction to detected unintended reverse energy transfer is:

```text
REVERSE_POWER_FLOW
        ↓
Charging Safety Supervision
        ↓
Safety Decision
        ↓
SAFE_STATE_REQUEST
        ↓
Safe-State Execution
        ↓
Charging energy transfer inhibited
        ↓
Safe-State Achievement Verification
        ↓
Safe state confirmed
```

Restart Permissibility / Verification prevents resumption of AC charging until safe charging conditions have been re-established.

---

### 4.5 SG-01 Safe State

The SG-01 safe state is:

> A state in which hazardous net energy transfer from the HV battery toward the external AC charging source is prevented.

**FTTI:** TBD

### 4.6 Technical Follow-Up

The technical architecture shall determine how unintended HV Battery → external AC source energy flow is prevented or controlled under relevant faults of the AC power-conversion path.

---

## 5. SG-02 Functional Safety Concept

**SG-02:**  
Prevent a thermal event caused by overcharging of the HV battery.

**Working classification:** ASIL C

The SG-02 concept combines battery charging permission, permitted charging limits, monitoring of actual charging conditions, safe-state execution, verification, and controlled restart.

### 5.1 Charging Permission and Limits

```mermaid
flowchart TB

    BMS["External BMS"]

    LIMIT["Battery Charging<br/>Permission / Limits"]

    CTRL["Charging Control<br/>AC / DC"]

    BAT["HV Battery"]

    ACTUAL["Actual Charging Conditions<br/>Voltage • Current • Temperature"]

    SUP["Charging Safety<br/>Supervision"]

    DEC["Safety Decision"]

    SAFE["Safe-State<br/>Execution"]

    BMS -->|CHARGE_ALLOWED<br/>V_CHARGE_MAX<br/>I_CHARGE_MAX<br/>Validity| LIMIT

    LIMIT -->|Permitted charging envelope| CTRL
    CTRL -->|Charging energy| BAT

    LIMIT -->|Permission + limits| SUP

    BAT --> ACTUAL
    ACTUAL --> SUP

    SUP -->|LIMIT_VIOLATION<br/>CHARGE_NOT_ALLOWED<br/>SAFETY_INFORMATION_INVALID| DEC

    DEC -->|SAFE_STATE_REQUEST| SAFE

    SAFE -.->|Inhibit charging| CTRL
```

**Figure 3 — SG-02 Overcharge Prevention and Supervision**

Source: [`04_sg02_overcharge.mmd`](./diagrams/04_sg02_overcharge.mmd)

Charging permission and charging-limit information are received through the Battery Charging Permission / Limit Interface.

Relevant information includes:

- `CHARGE_ALLOWED`
- `V_CHARGE_MAX`
- `I_CHARGE_MAX`
- associated validity / status information

Charging is permitted only when the required battery charging permission and safe charging conditions have been established.

---

### 5.2 Charging-Condition Supervision

During charging, Charging Safety Supervision evaluates the actual charging condition against the currently permitted battery charging conditions.

The supervised information includes:

- actual battery charging voltage
- actual battery charging current
- battery-temperature information
- permitted maximum charging voltage
- permitted maximum charging current
- charging permission
- validity and status of the required safety-relevant information

A charging condition outside the permitted battery charging limits or conditions results in `LIMIT_VIOLATION`.

Loss or invalidity of required safety-relevant information results in `SAFETY_INFORMATION_INVALID`.

Loss of battery charging permission results in `CHARGE_NOT_ALLOWED`.

---

### 5.3 SG-02 Fault Reaction

The functional reaction to an unsafe battery charging condition is:

```text
Unsafe charging condition
        ↓
Charging Safety Supervision
        ↓
Safety Decision
        ↓
SAFE_STATE_REQUEST
        ↓
Safe-State Execution
        ↓
Charging energy transfer inhibited
        ↓
Safe-State Achievement Verification
        ↓
Safe state confirmed
```

Restart Permissibility / Verification prevents resumption of charging until safe battery charging conditions have been re-established.

---

### 5.4 SG-02 Safe State

The SG-02 safe state is:

> A state in which continued charging cannot cause further hazardous overcharge of the HV battery.

**FTTI:** TBD

---

## 6. AC and DC Charging Responsibility

### 6.1 AC Charging

During AC charging, the vehicle-side AC power-conversion function performs charging-power conversion and regulation.

The charging system supervises:

- valid AC charging mode
- charging permission
- permitted battery charging limits
- actual charging conditions
- AC power-flow direction

---

### 6.2 DC Charging

During DC charging, the external DC charger performs the high-power regulation.

The vehicle communicates permitted or requested charging limits and supervises the resulting battery charging conditions.

The vehicle charging system remains responsible for detecting conditions that require charging to be stopped and requesting the charging safe state.

---

## 7. Common Safe-State and Recovery Concept

The functional architecture uses a common safety-response structure for SG-01 and SG-02.

```mermaid
flowchart TB

    REQ["SAFE_STATE_REQUEST"]

    PRIMARY["Primary Safe-State<br/>Execution"]

    VERIFY["Safe-State Achievement<br/>Verification"]

    SAFE["SAFE_STATE_CONFIRMED"]

    BACKUP["Backup / Escalated<br/>Safe-State Execution"]

    RESTART["Restart Permissibility /<br/>Verification"]

    ENABLE["New Charging<br/>Enable Sequence"]

    REQ --> PRIMARY

    PRIMARY --> VERIFY

    VERIFY -->|Confirmed| SAFE

    VERIFY -->|Not confirmed| BACKUP
    BACKUP --> VERIFY

    SAFE --> RESTART

    RESTART -->|RESTART_PERMITTED| ENABLE
```

**Figure 4 — Safe-State Execution and Recovery**

Source: [`05_safe_state_recovery.mmd`](./diagrams/05_safe_state_recovery.mmd)

```text
Hazard-relevant condition
        ↓
Charging Safety Supervision
        ↓
Safety Decision
        ↓
SAFE_STATE_REQUEST
        ↓
Safe-State Execution
        ↓
Safe-State Achievement Verification
        ↓
Restart Permissibility / Verification
```

The triggering conditions differ between the Safety Goals, but the functional safety-response path is shared.

Safe-State Execution prevents the relevant charging-energy transfer.

Safe-State Achievement Verification checks that the requested safe condition has been established.

Charging may resume only after Restart Permissibility / Verification confirms that the relevant safe charging conditions have been re-established.

---

## 8. Functional Architecture Design Decisions

### DD-FSC-01 — Mutually Exclusive Charging Modes

AC and DC charging functions shall not be simultaneously permitted.

**Rationale:**  
The established charging mode determines the applicable charging-energy path and control strategy. Simultaneous permission of both modes would create an ambiguous charging-path configuration.

**Status:** Accepted functional architecture decision.

---

### DD-FSC-02 — Mode Qualification Uses Multiple Information Sources

Charging-session information and electrical-interface characteristics are used to establish the charging mode.

Conflicting information results in:

`CONFLICT`

Insufficient trustworthy information results in:

`UNKNOWN`

Charging is inhibited when a valid charging mode cannot be established.

**Status:** Accepted functional architecture decision.

---

### DD-FSC-03 — Common Charging Safe State

The functional architecture uses a common charging safe-state concept in which charging-related energy transfer between the external charging source and the HV battery is prevented.

This does not require isolation of the HV battery from unrelated vehicle HV consumers.

The individual SG-01 and SG-02 safe-state definitions remain applicable.

**Status:** Accepted functional architecture decision.

---

### DD-FSC-04 — Backup Safe-State Execution

A candidate backup means of establishing the charging safe state is provided when primary safe-state execution cannot be confirmed.

The implementation mechanism and technical independence of the primary and backup paths are not yet established.

**Status:** Candidate design decision.

---

## 9. FSR Allocation

Detailed allocation of the Functional Safety Requirements to the functional architecture is maintained in:

[FSR–Function Allocation](../05_FSR_Function_Allocation/fsr_function_allocation.md)
