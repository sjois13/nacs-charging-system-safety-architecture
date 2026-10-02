```md
# System Context and Item Definition

**Document ID:** SYS-DEF-01  
**Status:** v0.1 — Work in Progress

## 1. Purpose and Scope

The item in scope is the vehicle-side EV charging system.

Its intended purpose is to receive electrical energy from an external charging source and transfer that energy toward the vehicle HV battery during AC or DC charging.

The item includes the vehicle-side functions required to control, monitor, and interrupt the charging process.

Detailed hardware and software implementation is not defined at this stage.

---

## 2. Item Boundary

The item boundary covers the vehicle-side charging system from the vehicle charge inlet up to the interfaces with the HV battery system and the Battery Management System (BMS).

The BMS and HV battery system are treated as external systems.

### 2.1 Elements Within the Item Boundary

The following elements are currently considered part of the item:

- Vehicle charge inlet
- On-Board Charger (OBC)
- Vehicle-side DC charging path
- Charging-related switching and isolation elements
- Voltage and current measurement functions used by the charging system
- Charging control functions
- Charging safety supervision functions
- Safe-state execution functions
- Restart and recovery functions

The detailed allocation of these functions to hardware and software is not yet defined.

### 2.2 External Systems and Exclusions

The following systems are outside the item boundary:

- Electrical grid
- EVSE / external charging equipment
- HV battery pack and battery cells
- Battery Management System (BMS)
- Vehicle propulsion system
- Other vehicle systems not directly part of the charging function

The BMS is treated as an external interacting system that provides battery-related information and charging permission to the charging system.

The detailed BMS interface is TBD.

---

## 3. System Context

The charging system forms the vehicle-side connection between the external charging equipment and the HV battery system.

During charging, the item interacts primarily with:

- **EVSE** — supplies electrical energy and provides charging-related information
- **BMS** — provides battery-related information and charging permission
- **HV Battery System** — receives charging energy
- **Other Vehicle Systems** — provide vehicle-state or charging-related information where required

The electrical grid is outside the vehicle boundary and supplies the external charging equipment.

### 3.1 System Context Diagram

![Vehicle Charging System Context](system_context.png)

**Figure 1 — Vehicle Charging System Context**

Solid connections represent electrical-energy flow. Dashed connections represent information or control interactions.

Detailed signal definitions and interface ownership are TBD.

---

## 4. Major Item Elements

The major elements currently identified within the charging-system boundary are:

| Element | High-Level Purpose |
| --- | --- |
| Vehicle Charge Inlet | Provides the vehicle-side physical connection to external charging equipment |
| On-Board Charger (OBC) | Converts incoming AC electrical power to DC during AC charging |
| DC Charging Path | Provides the vehicle-side electrical path for externally supplied DC charging energy |
| Charging-Related Switching / Isolation | Establishes or interrupts charging-related electrical paths |
| Voltage / Current Measurement | Provides electrical measurements for charging control and safety supervision |
| Charging Control | Coordinates normal charging operation |
| Charging Safety Supervision | Evaluates charging-related conditions relevant to safety |
| Safe-State Execution | Executes the required charging shutdown or isolation response when commanded |
| Restart / Recovery | Controls whether charging may resume following a fault or safe-state transition |

Detailed functional decomposition and technical implementation are defined during later architecture development.

---

## 5. Operating Modes

The following high-level operating modes are currently considered.

### 5.1 No Charging

No active transfer of charging energy from external charging equipment toward the HV battery is taking place.

### 5.2 AC Charging

The vehicle is connected to an AC charging source.

AC electrical energy is supplied through the vehicle charge inlet to the OBC. The OBC converts the incoming AC power to DC before energy is transferred toward the HV battery.

### 5.3 DC Charging

The vehicle is connected to a DC charging source.

DC electrical energy is supplied through the vehicle charge inlet and transferred through the vehicle-side DC charging path toward the HV battery.

The OBC is not used for power conversion in this mode.

Detailed charging states, including connection establishment, charging preparation, active charging, normal termination, fault handling, and restart, are TBD.

---

## 6. Engineering Assumptions

The following engineering assumptions are currently used:

- **A-01:** The BMS is outside the charging-system item boundary.
- **A-02:** The HV battery pack and battery cells are outside the charging-system item boundary.
- **A-03:** The charging system supports both AC and DC charging.
- **A-04:** Charging-related voltage and current information is available to charging control and/or safety-supervision functions where required by the functional architecture.

---

## 7. Design Decisions

The following item-level design decisions are currently established:

- **DD-01:** The vehicle-side charging system is treated as the item.
- **DD-02:** The BMS is modeled as an external interacting system.
- **DD-03:** Charging safety supervision, safe-state execution, and restart/recovery are represented as separate functional responsibilities in the functional architecture.

---

## 8. Open Questions

The following topics remain open:

- **TBD-01:** Detailed BMS-to-charging-system signal set and interface ownership
- **TBD-02:** Detailed charging state and mode model
- **TBD-03:** Detailed allocation of charging functions to ECUs, hardware, and software
- **TBD-04:** Detailed switching and isolation architecture
- **TBD-05:** Detailed measurement architecture and sensor allocation
- **TBD-06:** Detailed EVSE communication and charging-session interface
- **TBD-07:** Detailed restart and recovery conditions
```
