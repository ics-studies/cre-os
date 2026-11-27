# CRE-OS Shared Core

Status: local CRE-OS v1.0.1 structural reference rewired on 2026-11-25 and
revalidated on 2026-11-26. The canonical evidence gate, portable audit record,
measurement kernel, uncertainty engine, six metric contracts, CR-CSP, NDSS, SSTP
and VGT are present and pass the seventeen-check platform gate. All six metrics
now have provisional source-adjudicated twenty-case benchmarks. Independent
construct validation remains open, and commercial release is frozen.

The `ics_cre_os` package is the first shared-core implementation used to test
whether the audit products can share normative, evidence and version semantics
without losing their product boundaries.

## Implemented building blocks

- strict SemVer 2.0 parsing and increment rules;
- source references, evidence grades and human-confirmation requirements;
- a single metric-method registry with source-aligned names, scale types,
  output modes, calibration states and claim boundaries;
- an evidence-linked assessment gate that returns `not_assessable` unless every
  required metric dimension has confirmed evidence, a rubric ID and a rationale;
- a canonical measurement kernel that requires one typed uncertainty envelope
  for every numeric dimension and emits a SHA-256 audit record;
- a risk/Knightian/deep-uncertainty engine whose propagation preserves interval
  bounds and the strongest uncertainty kind without turning unknown scenarios
  into probabilities;
- a metric-calibration gate that records real cases, error, dimension and
  status gaps, source coverage and human-overturn history while excluding
  synthetic data from production claims;
- separate twenty-case public-source benchmarks for UCS, RFD, CDI, MBCL and
  CSIA, in addition to the existing CRV corpus;
- a deterministic CRV text-observation scorer that does not read expected
  labels and retains matched-rule provenance for every dimension;
- separate governance-layer (`G0`-`G4`) and runtime-layer (`R0`-`R4`) namespaces;
- the five canonical red lines, FRL-1 to FRL-5; FRL-5 requires separate
  shutdown-capability and rollback-capability evidence;
- the six indicator reference calculators;
- a finite-action CR-CSP reference solver with context-dependent lexical order;
- a machine-readable CR-CSP proof trace that records redline evidence IDs,
  missing-evidence requests, and the exact score order used for ranking;
- an NDSS decision-support report that exposes metric uncertainty, possible
  ordinal levels and explicit review requirements rather than hiding them
  behind a point estimate;
- a deterministic SSTP scenario runner with four baseline scenario types;
- an SSTP task-quality gate requiring an explicit Boolean outcome, finite
  metrics, failure attribution and reproducibility metadata;
- an append-only VGT version ledger whose rollback operation creates a new
  forward SemVer version and whose history can be exported and restored;
- a VGT dependency-impact graph with shortest impact paths, cycle detection,
  unresolved-artifact reporting and version-linked impact reports;
- VGT branch materialization and approval-gated forward merges that preserve
  both parent versions, source-branch provenance and the merge impact report;
- a neutral `PortableAuditRecord` used by both current audit products.
- a five-module DS-EOS local reference integration that emits the same portable
  audit contract while keeping planetary-protection, mission, and
  resource-fairness claims inside professional review boundaries.
- production MCP and local container responses for AI and ESG now include
  `cre_record`; AI version comparison includes `cre_records.previous`,
  `cre_records.current` and `cre_change`.
- a version-to-version audit comparison for added, removed, improved and
  regressed controls.
- a machine-readable metric and subsystem registry with explicit calibration
  status, output modes and claim boundaries;
- an `ics-cre-os status` command and a seventeen-check platform completion gate
  that distinguishes structural completion from scientific and commercial
  completion.

## Metric boundaries

- UCS returns a structured dimension vector by default. A scalar is generated
  only in explicit research mode and is labelled experimental.
- `RFD_capacity` is a bounded proxy in `[0,1]`. The lower-volume formal
  reachable-value-function ratio is implemented separately as
  `RFD_reachability`; `ΔRFD` remains a separate, signed action-impact value.
- CRV uses the second-volume three-vector and ten-subdimension structure. The
  operational default weights are `0.50/0.30/0.20` for observability and
  `0.40/0.30/0.30` for reversibility.
- CDI uses `EU/IP/NM/AD/NMat`.
- MBCL keeps the `R/I/C/reversibility/self-replication/existential-risk` vector.
  Its normalized risk value is auxiliary; protective gates determine
  classification and unresolved combinations remain undetermined.
- CSIA uses `scope/temporal_depth/severity/reversibility/uncertainty/cascade`.
  Reversibility is stored in its source direction and converted only when a
  risk score is calculated. Critical-dimension and existential-risk thresholds
  can raise the level even when the weighted score alone is lower.

## Usage

```python
from ics_cre_os import SemVer, evaluate_csia

version = SemVer.parse("2.0.0")
result = evaluate_csia(
    {
        "scope": 0.4,
        "temporal_depth": 0.3,
        "severity": 0.6,
        "reversibility": 0.8,
        "uncertainty": 0.5,
        "cascade": 0.2,
    }
)
```

Production metric use must go through the measurement kernel. The lower-level
evidence gate remains available for compatibility and validation:

```python
from datetime import datetime, timezone

from ics_cre_os import (
    DimensionAssessment,
    EvidenceGrade,
    EvidenceRecord,
    EvidenceSpan,
    MetricEvidenceAssessment,
    SourceRef,
    compute_metric_from_evidence,
)

evidence = EvidenceRecord(
    "The policy contains an annual review clause.",
    grade=EvidenceGrade.DOCUMENTED,
    spans=[
        EvidenceSpan(
            "policy:p12",
            SourceRef(0, "external", version=None, work="customer-policy"),
            "paragraph 12",
        )
    ],
)
assessment = MetricEvidenceAssessment(
    metric="UCS",
    items={
        key: DimensionAssessment(
            key=key,
            value=0.5,
            evidence_ids=("policy:p12",),
            rubric_id="UCS-2.0.0",
            rationale="Reviewer applied the published dimension rubric.",
        )
        for key in (
            "information_integration",
            "time_depth",
            "reflexivity",
            "value_sensitivity",
            "sociality",
        )
    },
    reviewer_id="rule-based-review",
    reviewed_at=datetime.now(timezone.utc),
    confirmed=True,
)
computation = compute_metric_from_evidence(assessment, [evidence])
```

The canonical path adds explicit uncertainty and auditability:

```python
from ics_cre_os import (
    MeasurementRequest,
    UncertaintyEnvelope,
    UncertaintyKind,
    measure,
)

measurement = measure(
    MeasurementRequest(
        assessment=assessment,
        evidence=(evidence,),
        dimension_uncertainty={
            key: UncertaintyEnvelope(
                kind=UncertaintyKind.RISK,
                point=0.5,
                interval=UncertaintyInterval(0.4, 0.6),
                rationale="Reviewer confidence interval",
                sources=(SourceRef(4, "4", "4.1"),),
            )
            for key in assessment.items
        },
    )
)
```

The evidence gate uses a conservative all-or-nothing rule: one unevidenced
dimension blocks the metric and identifies the missing dimensions. It records
reviewer judgment; it does not claim that software has automatically inferred
the correct dimension value from text.

Platform status:

```powershell
ics-cre-os status
```

The command reports structural component completion, provisional and
uncalibrated metrics, scientific completion and the external release state.
After the 2026-11-26 benchmark update, all six metrics are provisional and no
metric remains in the uncalibrated list; scientific completion remains false
because provisional cross-metric research calibration is not independent
validation.

Canonical evidence-gated measurement:

```powershell
ics-cre-os measure --input measurement.json
```

The lower-level `metric` command remains a reference-operator interface and is
not a substitute for the evidence and uncertainty gates.

NDSS can be run locally from a JSON input containing candidate actions and
metric results:

```powershell
ics-cre-os decide --input examples\ndss_decision.json
```

The output includes excluded actions, machine-readable redline or evidence-gap
proofs, the ranking scores actually used, and metric details. It remains a
decision-support report, not an institutional decision.

Input may also include `constraints`. Supported operators include existence,
equality, numeric comparisons and set containment. Each violated or unresolved
constraint returns its rationale, source and evidence field. If multiple rules
apply, `deny` takes precedence over `escalate`, which takes precedence over
`defer`.

## Local HTTP API

```powershell
ics-cre-os-api --host 127.0.0.1 --port 8787
```

Endpoints:

- `GET /health`;
- `POST /v1/decisions` using the same action and metric fields as the CLI.

The API is loopback-only. It has no account, billing, tenant isolation, upload
quota or public deployment controls and must not be exposed as a commercial
service in this form.

## Validation

```powershell
python -m pytest tests\test_ics_cre_os*.py
python -m ruff check src\ics_cre_os src\esg_commitment_audit\core_bridge.py
python validation\cre_os_acceptance.py `
  --output validation\cre_os_acceptance_2026-11-26.json
python validation\cre_os_platform_gate.py `
  --output validation\cre_os_platform_completion_2026-11-26.json
```

The acceptance script covers evidence gating, the canonical measurement kernel,
uncertainty propagation, generic constraints, NDSS API, SSTP quality, VGT
branch merge, three-product record reuse and calibration fail-closed behavior.
The platform gate additionally checks the six metric contracts, required
subsystems, all six twenty-case calibration datasets, CR-CSP scale baseline and
SSTP quality baseline.

## Claim boundary

Structural completion means dependent products can build against a stable local
core contract. The provisional benchmarks establish source-linked internal
reproduction under declared crosswalks. They do not establish independent
construct validity, external validation, public-service security, customer
acceptance, or permission to publish externally.
