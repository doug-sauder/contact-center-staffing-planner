<!--
Sync Impact Report
Version change: 1.0.0 → 2.0.0
Modified principles:
  - I. Code Quality & Maintainability → I. Code Quality & Maintainability (expanded: readability
    over cleverness, section-heading comment guidance added)
  - II. Test-First Development (NON-NEGOTIABLE) → II. Test-First Development (redefined: strict
    Red-Green-Refactor now scoped to deterministic logic only; stochastic/simulation behavior is
    explicitly carved out and instead requires statistical/property-based tests written after a
    first working implementation exists — a backward-incompatible relaxation of the prior
    blanket test-first mandate, and the removal of the NON-NEGOTIABLE qualifier)
  - IV. Performance & Scalability → IV. Performance & Scalability (redefined: removes the prior
    concrete response-time target (under 2 seconds) and the linear-scaling requirement; performance
    is now explicitly subordinated to correctness/simplicity early on, with targets deferred to a
    later, deployment-scenario-specific specification — a backward-incompatible redefinition of
    what compliance with this principle requires)
Added sections: none
Removed sections: none
Follow-up TODOs: none
-->

# Contact Center Staffing Planner Constitution

## Core Principles

### I. Code Quality & Maintainability
All code MUST be reviewed before merge and MUST pass linting and static analysis with no
suppressed errors. Functions and modules MUST have a single, clearly named
responsibility; duplication MUST be factored out only when a third occurrence appears
(rule of three), not speculatively. Every non-obvious design decision MUST be captured
in a commit message or code comment explaining the "why," not the "what." Dead code and
unused abstractions MUST be removed rather than commented out. Code MUST favor
readability over cleverness: arcane language features and nested anonymous functions
MUST be avoided where a plainer construct would do. Files SHOULD use section-heading
comments marking distinct logical regions so an unfamiliar reader can navigate quickly.

**Rationale**: Staffing calculations and schedules are revisited by different contributors
over time; unreviewed or unexplained code compounds into scheduling errors that are
costly to diagnose once staffing plans are already in production use. Readable,
well-sectioned code shortens that diagnosis time.

### II. Test-First Development
Tests MUST be written before implementation for deterministic logic: input validation,
boundary conditions (zero agents, over-capacity, partial shifts, time-zone edges), and
seed-reproducibility (identical seed and inputs MUST yield identical output). A failing
test MUST exist and be observed failing before the corresponding code is written
(Red-Green-Refactor) for this deterministic logic. For stochastic simulation behavior
(e.g., Monte Carlo output distributions), test-first in the strict sense does not apply,
since the correct output is not known until a reference implementation exists; these
MUST instead be verified with statistical/property-based tests — checking convergence,
expected mean/variance, and distributional properties against tolerance — written once a
first working implementation exists. No pull request MAY merge with reduced test
coverage on the lines it touches.

**Rationale**: Staffing outputs directly affect whether a contact center is adequately
covered; silent regressions in calculation logic translate into real understaffing or
overstaffing that is expensive to detect after the fact. Deterministic logic can and
should be specified before it's written; simulation correctness is inherently validated
against statistical expectations rather than exact pre-known values.

### III. User Experience Consistency
User-facing interfaces (CLI, API responses, or UI) MUST use consistent terminology,
formatting, and error messages across the planner. Every user-facing error MUST state what
went wrong and what the user can do next; errors MUST NOT surface raw stack traces or internal
identifiers to end users. Breaking changes to input formats or output schemas MUST be
versioned and MUST NOT silently change behavior for existing callers.
**Rationale**: Planners and schedulers rely on predictable inputs and outputs to trust the
tool's recommendations; inconsistent behavior erodes that trust and increases support burden.

### IV. Performance & Scalability
Performance MUST be treated as an explicit design consideration from the outset —
algorithmic choices MUST NOT foreclose reasonable performance later — but performance
MUST be subordinate to correctness and simplicity in the early stages of development.
Premature optimization MUST NOT be undertaken before core calculation and scheduling
logic is correct and well-tested. Because actual performance depends on deployment
characteristics (dataset size, site count, infrastructure) as much as on code, specific
performance targets (e.g., response-time thresholds) MUST be deferred until
representative deployment scenarios are known, and MUST then be captured and revisited
as a distinct specification rather than assumed at design time.

**Rationale**: Optimizing before correctness is established risks locking in flawed
logic under a veneer of speed, and performance targets set without knowledge of real
deployment conditions are guesses that create false precision.

### V. Observability & Simplicity
Every component MUST emit structured, actionable logs for calculation inputs, decisions, and
outputs so that a specific staffing recommendation can be traced back to the data that
produced it. New abstractions, dependencies, or configuration options MUST be justified by a
concrete, current requirement — not a hypothetical future one (YAGNI). Prefer the simplest
design that satisfies the current spec; added complexity MUST be called out and justified in
the plan's Complexity Tracking section.
**Rationale**: Staffing recommendations must be explainable to operations stakeholders, and
unexplained complexity makes both debugging and onboarding new contributors disproportionately
slow relative to the value it adds.

## Quality Gates

All pull requests MUST pass: automated tests (unit and integration), linting/static analysis,
and a code review by at least one other contributor before merge. Any exception to a gate
(e.g., merging with a known failing non-blocking test) MUST be explicitly approved by the
reviewer and recorded in the PR description with a rationale and follow-up plan.

## Development Workflow

Features MUST originate from a spec (via `/speckit-specify`) before implementation begins.
Implementation plans MUST include a Constitution Check confirming alignment with the
principles above before task breakdown starts. Any deviation from a principle MUST be
recorded in the plan's Complexity Tracking table with the simpler alternative that was
rejected and why.

## Governance

This constitution supersedes all other project practices, style guides, and undocumented
conventions. Amendments require: (1) a documented rationale for the change, (2) an update to
this file following the Sync Impact Report format, and (3) a version bump following semantic
versioning — MAJOR for incompatible governance or principle removals/redefinitions, MINOR for
new principles or materially expanded guidance, PATCH for clarifications and wording fixes.
All PRs and reviews MUST verify compliance with this constitution; unjustified complexity
MUST be flagged in review and either justified or removed before merge.

**Version**: 2.0.0 | **Ratified**: 2026-09-15 | **Last Amended**: 2026-09-15
