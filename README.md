```md
# NACS Charging System & Safety Architecture

**Status:** v0.1 — Work in Progress

## Overview

This repository is an independent systems-engineering and functional-safety case study of a vehicle-side EV charging system using NACS / SAE J3400.

The project considers both AC and DC charging and focuses on understanding the charging system from an architecture and safety perspective: system boundaries, charging paths, functions, interfaces, failure propagation, safety requirements, safety mechanisms, and verification.

The work is developed incrementally. Safety requirements and architecture decisions are intended to follow from system understanding, hazard analysis, and failure analysis rather than being selected in advance.

## Initial Safety Goals

Two safety goals are currently used as the starting point for the project.

### SG-01 — Unintended Energy Backfeed

Prevent unintended backfeed of energy from the HV battery toward the external AC supply during AC charging.

**Working classification:** ASIL D

### SG-02 — Battery Overcharge

Prevent a thermal event caused by overcharging of the HV battery.

**Working classification:** ASIL C

These classifications are treated as project / HARA working results and may be refined as the analysis develops.

## Engineering Approach

The project follows the engineering chain:

```text
Item Definition and System Context
        ↓
Hazard Analysis and Risk Assessment
        ↓
Safety Goals
        ↓
Functional Safety Requirements
        ↓
Functional Safety Architecture
        ↓
Technical Safety Requirements
        ↓
Technical Architecture
        ↓
HW / SW Allocation
        ↓
Safety Analysis
        ↓
Verification and Traceability
```

The process is iterative. Later architecture or safety analysis may expose gaps that require earlier requirements, assumptions, or design decisions to be revisited.

## Work Products

The main project work products are:

1. [System Context and Item Definition](docs/01_system_context_and_item_definition.md)
2. [Hazard Analysis and Risk Assessment](docs/02_hara.md)
3. [Safety Goals and Functional Safety Requirements](docs/03_safety_goals_and_fsrs.md)
4. [Functional Safety Concept](docs/04_functional_safety_concept.md)
5. [FSR–Function Allocation](docs/05_fsr_function_allocation.md)
6. [Technical Safety Concept](docs/06_technical_safety_concept.md)
7. [Safety Analysis](docs/07_safety_analysis.md)
8. [Verification Strategy](docs/08_verification_strategy.md)
9. [Open Issues and Engineering Decisions](docs/09_open_issues_and_decisions.md)


## Current Development

The current project phase focuses on the system boundary, functional safety concept, functional architecture, and first-pass allocation of Functional Safety Requirements.

Technical Safety Requirements, technical architecture, and detailed safety analyses will be developed as the corresponding engineering decisions mature.

## Project Basis and Limitations

This is an independent research, learning, and portfolio project. It does not represent work performed for an employer, customer, vehicle manufacturer, charging-equipment manufacturer, or production vehicle program.

The architecture is conceptual and is based on general EV charging engineering principles, public technical information, and explicitly documented project assumptions.

Unknown information is identified as **TBD**, **Open question**, or an explicit **Engineering assumption**.

No production-safety, compliance, or certification claims are made.

This repository is not intended to represent a production vehicle safety case.
