# Phase 2 Model and Data Plan

## Goal

Phase 2 produces a reproducible Loan Proposal from a versioned synthetic financial Snapshot using a tabular model.

## Included

- Synthetic loan dataset
- Versioned Feature Schema
- Feature preprocessing
- XGBoost and LightGBM comparison
- Model selection rationale
- Model Binary Artifact reference and digest
- Tabular Model Manifest
- SHAP explainer metadata
- Prediction threshold version
- Reproducibility report

## Excluded

- Real customer data
- Online learning
- Fine-tuning
- Local LLM inference
- External LLM APIs
- Final loan approval or rejection execution

## Reproducibility Definition

For the same:

- Snapshot ID or Snapshot content digest
- Feature Schema Version
- Preprocessing Version
- Model Version
- Model Binary Artifact Digest
- Threshold Version
- Random Seed
- Runtime Version
- Runtime Image Digest or immutable image reference
- Python Version
- Platform Architecture
- Thread Count
- Deterministic Configuration
- Model Serialization Format
- Dependency Lock Digest when available

the following must match within implementation-defined tolerance:

- Proposal outcome
- Score or probability
- Applied threshold
- Main SHAP contributions
- Evidence Reference
- Model provenance
- Failure classification

Numeric tolerance and comparison method are measured and fixed in the `rippleguard-agent-runtime` implementation PR.

## Model Comparison Criteria

XGBoost and LightGBM comparison must record:

- Data partition and label version
- Feature schema version
- Preprocessing version
- Framework and library version
- Evaluation dataset version
- Primary metric: PR-AUC when the selected synthetic label is meaningfully imbalanced; otherwise ROC-AUC. The implementation PR must record which condition applied before comparing results.
- Guardrail metric: recall at a fixed approval-risk or false-positive threshold.
- Calibration metric: Brier score or expected calibration error.
- False approval and false rejection cost interpretation.
- Class imbalance handling.
- Calibration and threshold rationale.
- SHAP compatibility
- Runtime latency and artifact size
- Reproducibility behavior
- Artifact portability across local and container runtime

If neither candidate satisfies the guardrail and reproducibility criteria, Phase 2 must not silently select the higher headline score. The Agent Runtime PR records the failed comparison and proposes a revised data or model plan.

## Data Boundary

Synthetic data still carries purpose and provenance metadata. Phase 2 must not assume unrestricted reuse of FDS or document-derived signals.
