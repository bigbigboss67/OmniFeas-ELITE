---
name: omnifeas-underwriting
description: >-
  Deterministic UNIDO and banking credit feasibility appraisal engine. Generates 5-year financial statements, DSCR loan covenants, NPV, IRR, and 10-module UNIDO feasibility dossiers for any business idea or industrial project.
---

# OmniFeas Underwriting Skill

## Overview
Automates bank-grade feasibility studies and institutional credit underwriting complying with **UNIDO Volumes I–III** and **GCC Development Funds (SIDF / ADF / Kafalah)** standards. Computes deterministic 5-year financial statements, debt amortization schedules, DSCR debt covenants, NPV, IRR, and downside sensitivity stress testing.

## Quick Start

### 1. Generate Bank-Ready Feasibility Study
```bash
python scripts/underwrite.py generate --project "Specialty Coffee Roastery in Dubai" --capex 650000 --debt-ratio 60 --margin 0.42 --currency AED --output feasibility_report.json
```

### 2. Simulate Revenue Stress Test (Downside Sensitivity)
```bash
python scripts/underwrite.py stress-test --capex 2500000 --margin 0.34 --debt-ratio 60 --shock -20 --output stress_test.json
```

### 3. Export 5-Year Financial Schedules (CSV)
```bash
python scripts/underwrite.py export-csv --capex 2500000 --margin 0.34 --debt-ratio 60 --output model.csv
```

## Workflow

### Step 1: Ingest Project Concept & Jurisdiction
Extract from the user request:
- Project Name & Sector (e.g., Solar EPC, Healthcare Clinic, Food & Beverage)
- Target Country (Defaults: Dubai/UAE -> AED, Saudi Arabia -> SAR)
- Initial Capital Expenditure (CAPEX)
- Debt-to-Equity Ratio (Standard: 60% Debt / 40% Equity)

### Step 2: Deterministic Financial Calibration
Run `underwrite.py generate` to calculate:
- **Min DSCR** (Must be >= 1.35x for bank loan covenant compliance)
- **Project NPV** at 10.5% WACC
- **Equity IRR** (Target >= 22%)
- **Payback Period** in operating years

### Step 3: 10-Chapter UNIDO Dossier Output
Compile the 10 UNIDO Appraisal Modules:
1. Executive Summary & Project Background
2. Market Demand, TAM/SAM/SOM Sizing & Pricing Power
3. Raw Materials & Supply Chain Logistics
4. Siting, Plant Location & Environmental Clearance (EIA)
5. Plant Sizing, Engineering & Tier-1 Machinery CAPEX
6. Organization, Human Resources & Localization Quotas (Saudization/Emiratization)
7. Implementation Milestones & 12-Month Trial Run Critical Path
8. Financial Schedules & Debt Service Coverage
9. National GDP Impact, Iktva & Job Creation
10. Downside Risk Matrix, Break-Even & Switching Values

## Common Mistakes
- **Assuming USD for GCC ventures:** Always verify currency. Dubai/UAE must use AED (د.إ), Saudi Arabia must use SAR (﷼).
- **Ignoring Grace Period:** Bank loan debt service amortization starts post-grace period (Month 12 commercial commissioning).
- **Accepting DSCR < 1.35x:** If modeled DSCR is under 1.35x, flag as bank covenant breach and suggest lowering debt ratio or extending tenor.
