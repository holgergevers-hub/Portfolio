# Skills Journey: From Assessment to Framework

**How a Zoho Creator assessment became five repositories, a development engine, a C runtime, and two research papers -- in 18 days.**

---

## The Starting Point

On April 6, 2026, Both& (a South African Zoho implementation partner) issued an assessment: **design and build an expense reimbursement system in Zoho Creator**. The brief included a process flowchart with a two-tier approval workflow, a R1,000 threshold, and a 24/48-hour SLA escalation mechanism.

The expected deliverable was a working prototype with forms, workflows, and a demo.

What followed was something different.

---

## The Journey Map

```
Apr 6          Apr 9          Apr 10         Apr 13         Apr 18         Apr 22
  |              |              |              |              |              |
  v              v              v              v              v              v
Expense     -> ForgeDS      -> Ten          -> Hologram    -> VLMN C      -> ForgeDS
Reimbursement  (tooling       Chargeback      Reality        Runtime        IDE
Manager        engine)        (2nd domain)    Clause         (systems)      (web app)
                                              (math
                                              framework)
```

### Phase 1: The Assessment (Apr 6-8)

**Repo:** [`expense_reimbursement_manager`](https://github.com/HolgerRGevers/expense_reimbursement_manager)

Before writing a line of code, the original process flowchart was analyzed for flaws. Seven were found, in three categories:

| Category | Flaw | Impact |
|---|---|---|
| **Logic Error** | R1,000 threshold only fires on the timeout path -- a R500K claim approved within 24h never reaches HoD | Material control gap |
| **Logic Error** | Timeout creates perverse incentive -- LM can dodge accountability by not acting | Governance bypass |
| **Logic Error** | Rejection is terminal -- no resubmit loop | Duplicate claims, broken traceability |
| **Missing Control** | No receipt upload, no expense/submission date split, no self-approval prevention | SARS non-compliance, fraud exposure |
| **Missing Control** | Hardcoded client list, no expense category for GL mapping | Unmaintainable, no financial reporting |
| **Poor UX** | No status visibility, no dashboard, one-directional notifications | Employee frustration, bottlenecks |

The redesigned system addressed all seven with a governance-first architecture:

- **7 forms** (Departments, Clients, GL_Accounts, Approval_Thresholds, Compliance_Config, Expense_Claims, Approval_History)
- **13 Deluge scripts** (form workflows, approval handlers, SLA enforcement)
- **44 linter rules** across Deluge, Access SQL, and cross-environment validation
- **Full South African compliance**: King IV governance (Principles 1, 7, 11, 13, 15), SARS Section 11(a) deduction formula, POPIA, ESG/ISSB tagging
- **GL code mapping** following Deloitte SA private-sector chart of accounts convention (6000-series)

**Key design decisions:**
- SLA timeout = escalation, not bypass (no auto-approve anywhere)
- Approval thresholds stored as data (configuration table), not hardcoded in Deluge
- Every state transition writes to Approval_History (King IV Principle 15 audit trail)
- Self-approval prevention (LM submitting own expense bypasses to HoD)

**Skills demonstrated:** Process analysis, business process redesign, South African compliance (King IV, SARS, POPIA), Zoho Creator (Deluge scripting, forms, workflows, approvals), data modeling (ERD, state machines), governance architecture.

---

### Phase 2: The Abstraction (Apr 9)

**Repo:** [`ForgeDS`](https://github.com/HolgerRGevers/ForgeDS)

While building the expense system, patterns emerged that were reusable. Instead of keeping them embedded in one project, they were extracted into a standalone development engine:

| Capability | What it does |
|---|---|
| **Deluge Linter** | 41 static analysis rules (DG001-DG018+) |
| **Access SQL Linter** | 8 rules for legacy database migration (AV001-AV008) |
| **Hybrid Linter** | 16 cross-environment rules (HY001-HY016) |
| **Scaffold Generator** | Generate `.dg` scripts from YAML manifests |
| **.ds Parser/Editor** | Extract, modify, and rebuild Zoho Creator app definitions |
| **Validator** | Pre-flight CSV data validation for imports |
| **Knowledge Base** | Scrape, tokenize, graph, and validate Zoho apps |
| **Reality Check** | HRC projection -- find structural gaps in applications |

This turned "build a Zoho app" into "build a Zoho app _correctly_, with static analysis, reproducible builds, and structural validation."

**Skills demonstrated:** Developer tooling, static analysis, language database design, YAML-driven code generation, Python packaging (`pip install`), CLI tool design.

---

### Phase 3: Domain Transfer (Apr 10)

**Repo:** [`Ten_Chargeback`](https://github.com/HolgerRGevers/Ten_Chargeback)

To validate that ForgeDS was genuinely reusable (not just a convenient refactor of the expense system), it was applied to a completely different domain: **chargeback management for Ten Group** across three merchant platforms (Adyen, Ingenico, Stripe).

- **ETL pipeline** normalizing 9,438+ merchant statement files into a unified schema (Python stdlib only -- zero external dependencies)
- **Multi-currency conversion** (GBP, EUR, ZAR to USD) with configurable exchange rates
- **Zoho Creator app** with lifecycle management (forms, workflows, 30-day dispute deadline alerts)
- **Interactive CFO/COO dashboard** showing $303K total exposure, 324 disputes, regional breakdown
- **GitHub Pages deployment** for the dashboard as a standalone presentation artifact

ForgeDS handled the `.ds` generation, linting, and validation for this second app -- confirming the tooling generalized.

**Skills demonstrated:** ETL pipeline design, multi-format data parsing, financial domain modeling, dashboard/visualization (HTML/CSS/JS), GitHub Pages deployment, domain transfer validation.

---

### Phase 4: The Mathematical Leap (Apr 13-22)

**Repo:** [`hologram-reality-clause`](https://github.com/holgergevers-hub/hologram-reality-clause)

The "Reality Check" feature in ForgeDS -- which detects structural gaps in Zoho Creator apps -- was based on an intuition: _if you look at a system from multiple mathematical perspectives simultaneously, inconsistencies become visible that no single perspective can reveal_.

This intuition was formalized into a mathematical framework:

**Core theorem:** A true model is invariant under change of perspective. A hallucination is not.

| Concept | Definition |
|---|---|
| **Projection functions** | Different mathematical "viewing angles" on code (type flow, data flow, schema structure, access patterns, lifecycle) |
| **Cross-projection constraints** | Rules that must hold between any two projections if the underlying model is real |
| **Residuals** | Scalar measure of constraint violation -- 0 means consistent, >0 means error with diagnostic location |
| **Empty space as attribute** | The closed-world assumption -- what is absent is provably absent |

The framework draws directly from electrical engineering (Kirchhoff's laws, impedance matching, Thevenin equivalents) and maps precisely to established CS concepts (abstract interpretation, type theory, Hoare logic) -- but the _unifying framework_ using multi-perspective consistency as a hallucination detector is the novel contribution.

**Research outputs:**
- Multiple papers in preparation (static analysis, dynamic systems, AI anti-hallucination)
- Core attribution theorem sealed after 4 adversarial review rounds
- Literature review confirming novelty against 15+ existing frameworks
- 4-leg CI pipeline (Ubuntu, macOS ARM64, MinGW64, MSVC)

**Skills demonstrated:** Mathematical proof construction, research paper writing, literature review methodology, adversarial self-review, formal methods, graph theory, attribution theory.

---

### Phase 5: Systems Programming (Apr 18)

**Repo:** [`VLMN`](https://github.com/holgergevers-hub/VLMN)

The Python reference implementation proved the math. The C runtime makes it deployable:

- **Pure C11** with zero external dependencies
- **Arena allocator** for deterministic memory management
- **Plugin architecture** for domain-specific projections
- **ABI-stable headers** with documented stability guarantees
- **CMake build system** with GCC, Clang, and MSVC support
- **Golden tests** ensuring bit-identical results against the Python reference
- **Threat model** documenting security boundaries

Built from scaffold to passing CI in a single day (15 commits).

**Skills demonstrated:** C systems programming, memory management (arena allocators), ABI design, CMake build systems, cross-platform compilation (Linux/macOS/Windows), security threat modeling.

---

### Phase 6: The IDE (Apr 21-22)

**Repo:** [`ForgeDS`](https://github.com/HolgerRGevers/ForgeDS) (continued)

ForgeDS came full circle with a web-based IDE:

- **AI-powered app builder** -- describe your app in plain language, AI generates the code
- **Visual `.ds` editor** with element inspection
- **Database migration manager** (Access to Zoho)
- **Dashboard wizard** with brainstorming engine
- **Multi-agent architecture** (fanout + persona round-table for design decisions)
- **TypeScript/Vite** frontend with design system

**Skills demonstrated:** Web application architecture, TypeScript, Vite build tooling, AI integration patterns, multi-agent orchestration, design systems, developer experience (DX) design.

---

## Skills Matrix

The table below maps every skill demonstrated across the five repositories to the phase where it first appeared and how it deepened.

| Skill Domain | Phase 1 (Expense) | Phase 2 (ForgeDS) | Phase 3 (Chargeback) | Phase 4 (HRC) | Phase 5 (VLMN) | Phase 6 (IDE) |
|---|---|---|---|---|---|---|
| **Process Analysis** | 7-flaw critique, state machines | -- | Current/future state mapping | -- | -- | -- |
| **SA Compliance** | King IV, SARS, POPIA, GL codes | -- | -- | -- | -- | -- |
| **Zoho Creator** | Deluge, forms, approvals | .ds parsing, scaffolding | .ds generation, deployment | -- | -- | Visual editor |
| **Python** | Validation scripts | Linters, CLI tools, packaging | ETL pipeline (stdlib only) | Proofs, reference impl | Golden test generator | -- |
| **Static Analysis** | -- | 65 lint rules (3 linters) | Validated via ForgeDS | -- | -- | -- |
| **Data Modeling** | ERD, 7 forms | YAML schema | Multi-platform normalization | Formal structures | -- | -- |
| **DevOps/CI** | -- | GitHub Pages | GitHub Pages | 4-leg CI matrix | CMake + CTest | Vite build |
| **C Programming** | -- | -- | -- | C acceleration | Full C11 runtime | -- |
| **Mathematics** | -- | -- | -- | Proofs, papers, theorems | ABI formalization | -- |
| **Web/Frontend** | -- | -- | Dashboard (HTML/CSS/JS) | -- | -- | TypeScript, Vite, design system |
| **Architecture** | 4-layer system | Plugin architecture | ETL pipeline | Formal structures | Arena + plugin + ABI | Multi-agent patterns |
| **Documentation** | Build guide, compliance docs | 13 lint rule docs | Process docs, README | 2 papers, lit review | Threat model, ABI docs | Design specs |

---

## The Thread

Every phase builds on the previous one. The expense system revealed reusable patterns (ForgeDS). ForgeDS enabled a second domain (chargebacks). The "Reality Check" feature in ForgeDS formalized into mathematics (HRC). The math needed a fast runtime (VLMN). The tooling needed a user interface (IDE).

The original Both& assessment asked: _"Can you build a working expense system in Zoho Creator?"_

The answer was five repositories, three programming languages, two research papers, a mathematical theorem, and a development engine that can build the next one faster.

---

## Repository Index

| Repository | Description | Languages | Status |
|---|---|---|---|
| [`expense_reimbursement_manager`](https://github.com/HolgerRGevers/expense_reimbursement_manager) | Governance-first expense system with SA compliance | Python, Deluge | Shipped (v1.0.0) |
| [`ForgeDS`](https://github.com/HolgerRGevers/ForgeDS) | Zoho Creator rapid development engine + AI-powered IDE | Python, TypeScript, Deluge | Active development |
| [`Ten_Chargeback`](https://github.com/HolgerRGevers/Ten_Chargeback) | Chargeback management across 3 merchant platforms | Python, JavaScript, Deluge | Shipped (v1.0.0) |
| [`hologram-reality-clause`](https://github.com/holgergevers-hub/hologram-reality-clause) | Mathematical framework for multi-perspective invariant analysis | Python, C, TeX | Active research (v0.2.0) |
| [`VLMN`](https://github.com/holgergevers-hub/VLMN) | C runtime for Very Large Memory Neurons | C, CMake | Scaffold + Slice 1 |

---

## For the Both& Assessment Team

This showcase is designed to answer the five evaluation criteria identified in the assessment analysis:

1. **Can you analyse a process critically?** -- Phase 1 found 7 flaws in the original flowchart before writing code.
2. **Do you understand the business context?** -- GL mapping, King IV governance, SARS compliance, delegation of authority matrices -- the system was designed around _why_ expense controls exist, not just _how_ they work.
3. **Can you learn a new tool under pressure?** -- Zoho Creator was self-taught for the assessment. The response was to build a development engine for it.
4. **Can you build a working prototype?** -- Two shipped applications (expense + chargeback), both with `.ds` files ready for one-click Zoho import.
5. **Can you communicate what you built and why?** -- Every design decision is documented with rationale. Every compliance control traces to a specific King IV principle or SARS provision.

### What this journey demonstrates beyond the assessment

- **Abstraction instinct**: Seeing reusable patterns and extracting them into tools (ForgeDS) rather than copy-pasting between projects.
- **Domain transfer**: Proving the tools work on a different problem (chargebacks) validates the abstraction was real, not accidental.
- **Depth over breadth**: Going from "build a form" to "prove the mathematical invariants that guarantee the form is correct" -- each layer justified by concrete need, not theoretical ambition.
- **Ship velocity**: 5 repos, 133+ commits, 3 languages, 2 shipped products, 2 research papers, 1 theorem -- 18 days. Every artifact is public, auditable, and functional.

---

## Timeline (Commit-Level)

| Date | Repository | Milestone |
|---|---|---|
| Apr 6 | expense_reimbursement_manager | First commit -- forms, validation, approval workflows |
| Apr 7 | expense_reimbursement_manager | Access SQL linter, hybrid linter, mock data generator (6 SA personas) |
| Apr 8 | expense_reimbursement_manager | Two-key threshold auth, ESG tracking, .ds editor deployment |
| Apr 9 | ForgeDS | Extracted as standalone engine -- lint, scaffold, parse, validate |
| Apr 10 | Ten_Chargeback | ETL pipeline, Deluge workflows, CFO dashboard, GitHub Pages |
| Apr 13 | hologram-reality-clause | Initial framework -- multi-perspective invariant analysis |
| Apr 13 | expense_reimbursement_manager | README restyled, .ds download links via GitHub Releases |
| Apr 14 | Ten_Chargeback | README restructured for CTO interview presentation |
| Apr 14 | hologram-reality-clause | Schema extraction as third projection source |
| Apr 17 | hologram-reality-clause | Generalized proof, observability analysis, technical brief |
| Apr 18 | hologram-reality-clause | Package restructure, formal proofs, symmetry analysis |
| Apr 18 | VLMN | Full scaffold to Slice 1 ABI + golden tests (15 commits in 1 day) |
| Apr 21 | hologram-reality-clause | Core attribution theorem sealed after 4 adversarial rounds |
| Apr 21-22 | ForgeDS | Web IDE -- dashboard wizard, brainstorming engine, design system |
| Apr 22 | hologram-reality-clause | v0.2.0 shipped -- attribution Python + C + 4-leg CI |

---

*Built with Python, C, TypeScript, Deluge, and Claude Code.*
