# instructions.md

# Repository Implementation Instructions

These instructions supplement `CLAUDE.md` and apply to code changes in this repository.

## Scope Guard

- Implement only requested work and the approved core requirements.
- Keep this project a focused undergraduate IoT network monitoring dashboard.
- Do not add advanced features (dynamic graph streaming, community detection, deployment, SIEM integrations, multi-model benchmarking, etc.) unless explicitly requested and the core scope is satisfied.
- Do not independently change the approved stack.

## Application and UI

- Use Streamlit for the application UI.
- Prefer clear navigation, simple page layouts, readable labels, and practical tables/charts.
- Show informative empty states when datasets, graphs, detections, model artifacts, or evaluation results are unavailable.
- UI visibility is not an authorization mechanism: verify permissions before executing protected operations.
- Keep user-facing wording consistent with the use-case names.

## Data Validation and Preprocessing

- Accept CSV files only.
- Validate the documented required columns: `timestamp`, `source_ip`, `destination_ip`, `protocol`, `packets`, `bytes`, `duration`, `label`.
- Provide clear errors for missing columns and invalid files.
- Remove duplicate records and handle missing/invalid values using the documented preprocessing logic.
- Convert data types safely.
- Map labels to exactly two classes: Normal and Suspicious.
- Do not invent missing labels or values.
- Preserve an understandable preprocessing summary where possible.
- Do not silently overwrite or discard user data except as documented and confirmed.

## Graph Construction

- Nodes represent unique device/IP addresses.
- Edges represent communication between devices.
- Keep graph construction deterministic and consistent with the feature/model pipeline.
- Treat edge direction and edge-weight choice as decisions that need to match existing implementation/training artifacts; do not make an unreviewed change.
- Avoid rendering huge graphs in a way that freezes the dashboard; use a reasonable, documented approach, but do not silently omit important data from analysis.

## Features and GNN Inference

- Use only the feature set expected by the trained model.
- Feature order, preprocessing, scaling, and graph structure at inference must match training.
- The required project uses one GNN architecture (GCN or GraphSAGE); do not implement or switch between architectures without approval.
- Do not retrain during ordinary dashboard detection unless specifically requested.
- If model artifacts are missing or incompatible, report the problem clearly and do not fabricate output.
- Centralize and document threshold configuration; treat 0.5 as provisional until confirmed.

## Rule-Based Detection and Evaluation

- Keep the rule-based detector separate from the GNN detector.
- Centralize rules and thresholds; avoid spreading threshold constants throughout the UI.
- Evaluate both detectors on the same labeled devices when ground truth is available.
- Use accuracy, precision, recall, F1-score, and confusion matrices as documented.
- Do not show evaluation metrics if ground-truth labels or necessary predictions are unavailable. Show a clear explanation instead.
- Present measured results neutrally. Never imply one method is superior without evidence.

## Alerts

- Follow the approved alert-generation behavior.
- An alert explanation must reference available features, prediction evidence, or triggered rules only.
- Do not fabricate threat attribution or causes.
- Read-only Viewer access must not modify alert statuses.
- Alert-review status changes, if implemented, are restricted to Security Analyst/Admin.
- The Review Alert feature remains subject to the project decision noted in `CLAUDE.md`.

## Authorization, Users, and Roles

- Enforce role authorization in the backend/application logic for every protected operation.
- Roles: Viewer, Security Analyst, Admin, following the documented inheritance model.
- Every user has exactly one role.
- Admin-created accounts only; no self-registration.
- Hash passwords using a recognized secure library; never store or log plaintext passwords.
- Ensure usernames and email addresses are unique.
- Disabled accounts cannot log in.
- Protect the last Admin account.
- Admin cannot modify their own role or disable/delete their own account as documented.
- Default roles cannot be deleted; Admin cannot be renamed/deleted.
- A role assigned to users cannot be deleted until those users are reassigned.
- Admin permissions must retain user and role management access.
- Avoid exposing secrets or sensitive account data in logs or UI.

## Code Quality

- Inspect existing patterns and project structure first.
- Write straightforward, maintainable Python appropriate for an undergraduate project.
- Use descriptive names and type hints where useful.
- Keep functions focused and avoid oversized modules.
- Avoid unnecessary dependencies and abstractions.
- Centralize constants and configuration.
- Avoid duplicating preprocessing, graph, feature, detection, and permission logic.
- Handle exceptions at appropriate boundaries and provide useful user messages.
- Never expose raw exception traces to ordinary users; logs may contain diagnostic details but no secrets.
- Do not claim security guarantees that have not been implemented.

## Changes and Verification

- Make targeted changes; avoid unrelated refactors.
- Before changing files, briefly outline the intended approach and relevant use cases.
- If an unresolved requirement blocks the work, ask for confirmation instead of silently deciding.
- For meaningful changes, add/update tests if the project has testing infrastructure.
- Run appropriate tests or validation when possible.
- Clearly distinguish tests that were actually run from tests that are merely recommended.
- Update documentation when a change affects workflows, features, configuration, or use cases.

## Final Response Format for Work Done

When finishing a repository task, provide:

1. What changed.
2. Files changed.
3. Relevant use cases or requirements affected.
4. How to run or verify it.
5. Tests performed and their actual results.
6. Any assumptions, unresolved decisions, or limitations.

## Network and GNN Implementation Rules

- Inspect the existing graph pipeline and trained model before modifying graph construction or feature extraction.
- Preserve graph semantics, node identity, edge representation, directionality, and aggregation behavior unless an approved change is required.
- Do not independently change self-loop behavior, isolated-node handling, or edge weighting.
- Maintain the exact feature schema expected by the trained model, including feature order, dimensions, data types, and normalization.
- Prevent label leakage in training, preprocessing, feature extraction, and evaluation.
- Do not use ground-truth labels as inference features.
- Do not retrain the model during ordinary dashboard detection.
- Do not change the GNN architecture or trained model artifacts without approval.
- Validate model compatibility before performing inference.
- Keep model thresholds and rule-based thresholds centralized.
- Do not fabricate detection results when model loading or inference fails.
- Keep GNN predictions and rule-based predictions separate.
- Evaluate both methods using the same labeled samples whenever comparison is performed.
- Do not report evaluation metrics without valid predictions and ground-truth labels.
- Preserve the separation between training, inference, and evaluation.
- Verify that any change to graph construction or features remains compatible with the trained model.
- Avoid memory-intensive graph processing or dashboard rendering that unnecessarily compromises usability.
- Document any change that affects model behavior, graph structure, feature definitions, thresholds, or evaluation methodology.
