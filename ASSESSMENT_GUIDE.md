# Assessment Team Evaluation Guide

This document maps the Both& assessment criteria to specific, auditable artifacts across the five repositories. Every claim is verifiable -- follow the links.

---

## 1. Process Analysis Capability

**Question:** Can the candidate analyse a process critically, or do they build whatever is drawn without questioning it?

**Evidence:**

The original expense approval flowchart contained seven deliberate or structural flaws. All seven were identified _before_ building:

| # | Flaw | Where it's documented | Where it's fixed |
|---|---|---|---|
| 1 | Threshold gate (R1,000) only fires on timeout path | Analysis doc, Section 1 | [`expense_claim.on_validate.dg`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/src/deluge/form-workflows/expense_claim.on_validate.dg) |
| 2 | Timeout = perverse incentive (LM can dodge accountability) | Analysis doc, Change 2 | [`sla_enforcement_daily.dg`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/src/deluge/scheduled/sla_enforcement_daily.dg) -- escalation, never bypass |
| 3 | Rejection is terminal (no resubmit) | Analysis doc, Change 3 | [`expense_claim.on_edit.dg`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/src/deluge/form-workflows/expense_claim.on_edit.dg) -- version tracking |
| 4 | No receipt upload | Analysis doc, Change 4 | Mandatory file upload field + on_validate hard stop |
| 5 | No date split / late submission policy | Analysis doc, Change 5 | 90-day validation in on_validate |
| 6 | No expense category for GL mapping | Analysis doc, Change 6 | GL_Accounts lookup form + auto-population |
| 7 | Hardcoded client list | Analysis doc, Change 7 | Dynamic lookup forms (Departments, Clients) |

**Evaluation scale:**
- Expected: identify 2-3 flaws
- Demonstrated: identified all 7, categorized into logic errors / missing controls / UX, and fixed each with traceable code

---

## 2. Business Context Understanding

**Question:** Does the candidate understand _why_ the system exists (financial controls, governance, compliance), or just _how_ it works (forms, buttons, workflows)?

**Evidence:**

| Context Layer | Artifact |
|---|---|
| **King IV Governance** | [`docs/compliance/king-iv-mapping.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/king-iv-mapping.md) -- maps Principles 1, 7, 11, 13, 15 to specific system controls |
| **SARS Compliance** | [`docs/compliance/sars-requirements.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/sars-requirements.md) -- Section 11(a) deduction formula, VAT invoice thresholds (R50, R5,000), 5-year retention |
| **GL Code Schedule** | [`config/seed-data/gl_accounts.json`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/config/seed-data/gl_accounts.json) -- Deloitte SA convention, 6000-series, SARS_Provision per code |
| **Delegation of Authority** | [`docs/compliance/delegation-of-authority.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/delegation-of-authority.md) -- configurable tiers stored as data, not hardcoded |
| **ESG/ISSB** | [`docs/compliance/esg-reporting-guide.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/esg-reporting-guide.md) -- GL accounts tagged with ESG_Category and Carbon_Factor for Scope 3 |
| **POPIA** | Privacy-by-design in role permissions and data access patterns |
| **Companies Act** | [`docs/compliance/companies-act-alignment.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/companies-act-alignment.md) |
| **International Standards** | [`docs/compliance/international-standards-mapping.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/compliance/international-standards-mapping.md) -- ISO 37001 anti-bribery, COSO framework |

**Depth indicator:** The GL_KingIV_SARS_Compliance_Reference document was produced as a standalone reference -- not as afterthought commentary, but as the _design input_ that drove the system architecture.

---

## 3. Tool Learning Velocity

**Question:** Can the candidate learn a new tool (Zoho Creator / Deluge) under time pressure?

**Evidence:**

| Indicator | Measurement |
|---|---|
| Assessment start | April 6, 2026 |
| Working prototype shipped | April 8, 2026 (3 days) |
| Development _engine_ for the tool shipped | April 9, 2026 (4 days) |
| Second application built using the engine | April 10, 2026 (5 days) |
| Web IDE for the tool shipped | April 22, 2026 (17 days) |

The learning trajectory was not "learn tool, use tool" -- it was "learn tool, identify tool's weaknesses, build tooling to address them, apply tooling to a second domain, build an IDE around the tooling."

**ForgeDS capabilities built from zero Zoho experience:**
- 41-rule Deluge linter (static analysis of a proprietary language)
- `.ds` file parser/editor (reverse-engineered Zoho's app definition format)
- Scaffold generator (YAML manifest to Deluge code)
- Knowledge base with structural validation
- Web-based IDE with AI integration

---

## 4. Prototype Quality

**Question:** Can the candidate build a working system, not just wireframes or slides?

**Evidence:**

Both shipped applications include `.ds` files for one-click Zoho Creator import:

| Application | Download | Forms | Scripts | Seed Data |
|---|---|---|---|---|
| **Expense Reimbursement** | [`.ds` file](https://github.com/holgergevers-hub/Expense_Reimbursement_Manager/releases/download/v1.0.0/Expense_Reimbursement_Management-stage.ds) | 7 | 13 | 5 JSON files |
| **Ten Chargeback** | [`.ds` file](https://github.com/holgergevers-hub/Ten_Chargeback/releases/download/v1.0.0/ten_chargeback_management.ds) | Custom | Workflows + scheduled | Exchange rates + merchant data |

Both include:
- Mock data generators for testing (175 synthetic claims with 7 SA personas for expense system)
- Complete build guides for manual construction
- Test scenarios documented
- Demo scripts for stakeholder walkthroughs

**The chargeback dashboard is live:** viewable on [GitHub Pages](https://holgergevers-hub.github.io/Ten_Chargeback/src/dashboard/index.html) without any Zoho account.

---

## 5. Communication Clarity

**Question:** Can the candidate explain what they built, why they made specific design decisions, and how they'd extend it?

**Evidence:**

| Artifact | Purpose | Location |
|---|---|---|
| 4 architecture documents | ERD, state machine, approval routing, 4-layer system architecture | [`docs/architecture/`](https://github.com/HolgerRGevers/expense_reimbursement_manager/tree/main/docs/architecture) |
| System diagrams PDF | Professional presentation-ready diagrams | Original Both& deliverable |
| Build sequence guide | 19-step construction walkthrough for Zoho Creator | [`docs/build-guide/build-sequence.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/build-guide/build-sequence.md) |
| Remediation plan | Gap analysis with prioritized fixes | [`docs/build-guide/remediation-plan.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/docs/build-guide/remediation-plan.md) |
| Process current/future state | Documented transformation narrative | Ten_Chargeback `docs/process-*.md` |
| Changelog | Versioned change history | [`CHANGELOG.md`](https://github.com/HolgerRGevers/expense_reimbursement_manager/blob/main/CHANGELOG.md) |

Every design decision traces to a specific governance requirement, compliance provision, or user need -- not to personal preference or convention.

---

## What Went Beyond the Assessment

The assessment asked for a Zoho Creator expense system. The response included:

1. A **governance-first redesign** of the original process with 7 identified flaws
2. A **compliance reference document** mapping King IV, SARS, and Companies Act to system controls
3. A **development engine** (ForgeDS) that can build the _next_ Zoho Creator app faster
4. A **second application** (Ten Chargeback) proving the engine generalizes
5. A **mathematical framework** (HRC) formalizing the structural validation approach
6. A **C runtime** (VLMN) making the framework deployable at scale
7. A **web IDE** (ForgeDS IDE) bringing it full circle to developer experience

Each step was motivated by a concrete need identified in the previous step -- not by scope creep or theoretical ambition.
