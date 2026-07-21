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
- Prediction quality metrics chosen by implementation PR
- Calibration or threshold rationale
- SHAP compatibility
- Runtime latency and artifact size
- Reproducibility behavior

## Data Boundary

Synthetic data still carries purpose and provenance metadata. Phase 2 must not assume unrestricted reuse of FDS or document-derived signals.
