# CLAUDE.md

# IoT Network Monitoring Tool Using GNN

## 1. Project Identity and Goal

This repository implements **IoT Network Monitoring Tool Using Graph Neural Networks**, an undergraduate graduation project.

The system processes IoT network traffic from a CSV dataset, preprocesses it, builds a device communication graph, extracts node features, applies a trained GNN node-classification model and a rule-based baseline, compares their results, and presents monitoring information in a Streamlit dashboard.

Keep the implementation practical, understandable, modular, and appropriate for an undergraduate project. Do not turn it into a production SOC, enterprise SIEM, or unnecessarily complex research platform.

## 2. Approved Technology Stack

Use the following technologies unless the repository already contains an explicit, approved change:

- Python
- Streamlit: dashboard and application UI
- Pandas: data loading and preprocessing
- NetworkX: communication graph construction and graph analysis
- PyTorch and PyTorch Geometric: GNN implementation and inference
- Scikit-learn: evaluation metrics and supporting utilities

Do not introduce React, Next.js, FastAPI, Flask, a separate frontend/backend architecture, a cloud deployment platform, a database, or other major technologies without explicit approval and a clear need. Streamlit is the approved dashboard approach.

## 3. Scope and Workflow

The expected core workflow is:

1. Upload IoT traffic CSV.
2. Validate file type and required columns.
3. Preprocess data.
4. Build a graph of devices and their communications.
5. Extract and normalize node features.
6. Run GNN detection and rule-based detection.
7. Store/display both results for the analyzed devices.
8. Generate alerts for devices classified as suspicious by the GNN (follow the current requirements unless explicitly revised).
9. View the dashboard, graph, devices, alerts and model evaluation.
10. Compare GNN and rule-based results against ground-truth labels when labels are available.
11. Allow Security Analysts to review alerts if that functionality is included in the implementation.
12. Allow Admins to manage users and roles/permissions as specified.

Avoid implementing optional extensions unless the core workflow is working and the user explicitly requests an extension.

## 4. Dataset Contract

The expected CSV columns are:

- `timestamp`
- `source_ip`
- `destination_ip`
- `protocol`
- `packets`
- `bytes`
- `duration`
- `label`

Treat this as the current documented dataset contract. If an existing dataset or repository implementation uses different column names, do not silently change the contract: inspect it, document the mapping, and ask or make a clearly explained compatibility mapping.

Labels are simplified into exactly two classes:

- Normal
- Suspicious

Preprocessing should address duplicate records, missing values, data types, invalid records, and label mapping. Explain and report removed or invalid records when practical. Do not silently invent data or labels.

Only CSV uploads are in scope. A newly uploaded dataset may replace the current analysis dataset according to the documented behavior, but replacement behavior, file size limits, and other items explicitly marked TBD must not be treated as final decisions without confirmation.

## 5. Graph and Node Feature Contract

- A node represents one unique IP address/device.
- An edge represents communication between two devices.
- Edge weights represent a communication-volume measure, such as packets or bytes. The final choice is a project decision/TBD; do not assume it is finalized.
- Features currently documented include number of connections, degree, total packets, total bytes, and number of unique destinations.
- Feature normalization is expected.
- The final feature list, order, preprocessing, and normalization must be consistent with those used to train the GNN.

Do not change feature definitions, graph semantics, edge directionality, weighting, or normalization casually. Inspect existing code/model artifacts first and maintain training/inference consistency. If an unresolved design decision is blocking, explain it and ask rather than silently choosing.

## 6. Detection and Model

- The task is node classification.
- The GNN outputs a suspicious/anomaly probability per device.
- Only one GNN architecture should be used for the required version: GCN or GraphSAGE. The final architecture is not yet treated as confirmed unless the repository or user explicitly confirms it.
- The documented default classification threshold is 0.5, but it is marked as subject to confirmation. Do not hard-code it throughout the project; centralize it and make its status clear.
- The model is trained beforehand (for example, in a training notebook or dedicated training script). The dashboard's Run Detection workflow is primarily inference; do not retrain on every dashboard interaction unless specifically requested.
- Handle missing/unavailable model artifacts gracefully. Do not fabricate predictions.

## 7. Rule-Based Baseline and Comparison

Implement the rule-based detector as a separate baseline, with rules and thresholds centralized and documented. Thresholds are still a team decision/TBD where not specified.

Both methods should be evaluated on the same devices and the same ground-truth labels where labels exist. Report measured results honestly; do not assume the GNN is better.

Metrics documented for evaluation/comparison:

- Accuracy
- Precision
- Recall
- F1-score
- Confusion matrix

Do not calculate or present metrics as valid ground-truth evaluation when labels are missing. Explain when evaluation is unavailable.

## 8. Roles, Inheritance, and Authorization

The documented actor inheritance is:

- **User**: Log In, Log Out
- **Viewer** inherits User and can access read-only monitoring use cases.
- **Security Analyst** inherits Viewer and can run the analysis pipeline and review alerts (review-alert functionality is subject to the documented confirmation).
- **Admin** inherits Security Analyst and can manage users and roles.

The inheritance in the use-case model is a permissions baseline, not a reason to grant access without checks. Enforce permissions in the application logic, not only by hiding UI controls.

### Viewer capabilities (read-only)

- View Dashboard Overview
- View Network Graph
- Highlight Suspicious Devices
- View Devices Table
- View Device Details
- View Alerts
- View Alert Explanation
- View Model Evaluation
- View Confusion Matrix

Viewers must not run the analysis pipeline or alter alert status, users, roles, or permissions.

### Security Analyst capabilities (inherited)

- Upload Traffic Dataset
- Preprocess Data
- Build Communication Graph
- Extract Node Features
- Run Anomaly Detection
- Run GNN Detection
- Run Rule-Based Detection
- Compare GNN vs Rule-Based Results
- Review Alert (only if retained/implemented according to the confirmed requirement)

### Admin capabilities (inherited plus administrative)

- Manage Users: Add User, Edit User, Delete User
- Manage Roles: Add Role, Edit Role, Delete Role, Assign Permissions, Assign Role to User

The documented default roles are Viewer, Security Analyst, and Admin. Each user has exactly one role. Do not bypass or weaken role restrictions.

## 9. Authentication and Account Rules

The documented requirements include:

- No self-registration; Admin-created accounts only.
- Login requires credentials and an active account.
- Passwords must be hashed; never store plaintext passwords.
- Disabled accounts cannot log in.
- Usernames and emails must be unique.
- Every user has exactly one role.
- Admin cannot disable, delete, or change their own role (as specified in the use cases).
- At least one Admin account must always exist.
- Deletion must preserve alert-review records where required.
- The Admin role cannot be renamed or deleted.
- Default roles cannot be deleted.
- Roles with assigned users cannot be deleted until users are reassigned.
- The Admin role must retain user- and role-management permissions.

Treat the exact hashing library, password length policy, account/session implementation, and persistence technology as implementation choices unless already established. Prefer secure, standard approaches and avoid custom cryptography.

## 10. Use-Case Naming and Requirements Consistency

Use the names in the current Use Case Diagram and Use Case Descriptions as the reference terminology. The documented set is UC-01 through UC-30.

Do not casually rename pages, operations, roles, use cases, or core entities. If a change is necessary, keep the diagram, documentation, requirements, and implementation aligned and explain the change.

The key use cases are:

- UC-01 Log In; UC-02 Log Out
- UC-03 View Dashboard Overview; UC-04 View Network Graph; UC-05 Highlight Suspicious Devices; UC-06 View Devices Table; UC-07 View Device Details; UC-08 View Alerts; UC-09 View Alert Explanation; UC-10 View Model Evaluation; UC-11 View Confusion Matrix
- UC-12 Upload Traffic Dataset; UC-13 Preprocess Data; UC-14 Build Communication Graph; UC-15 Extract Node Features; UC-16 Run Anomaly Detection; UC-17 Run GNN Detection; UC-18 Run Rule-Based Detection; UC-19 Compare GNN vs Rule-Based Results; UC-20 Review Alert
- UC-21 Manage Users; UC-22 Add User; UC-23 Edit User; UC-24 Delete User; UC-25 Manage Roles; UC-26 Add Role; UC-27 Edit Role; UC-28 Delete Role; UC-29 Assign Permissions; UC-30 Assign Role to User

## 11. Dashboard Requirements

The dashboard is a functional project dashboard, not a professional enterprise security product. Its core views include:

- Overview: device count, connection count, suspicious-device count, and model performance summary.
- Network graph: devices/nodes and communications/edges; highlight suspicious nodes.
- Devices table: device IP, relevant features, rule-based result, GNN result, and anomaly probability; sortable where practical.
- Alerts: suspicious-device alerts and available evidence/explanation.
- Model Evaluation: metrics and confusion matrix.
- Comparison: side-by-side GNN and rule-based metrics and disagreements, when labels and results are available.
- Admin pages for user and role management, if implemented.

Respect the documented no-results states. Never fabricate placeholder analysis results as if they were real.

## 12. Explanation and Alerts

Alert explanations should be based only on evidence actually available in the data, model output, or recorded rule triggers. Do not claim a device's intent, root cause, or threat identity unless the system can verify it.

Use the documented GNN result and probability for the GNN-based alert behavior. Whether rule-based results also affect graph highlighting or alert creation is a decision marked for confirmation; do not silently redefine it.

## 13. Engineering Rules

- Inspect the repository before proposing or making changes.
- Follow the existing project structure, naming conventions, and patterns unless they are clearly problematic.
- Keep code modular and readable. Avoid unnecessary abstractions, frameworks, or dependencies.
- Separate preprocessing, graph construction, feature extraction, detection, evaluation, authorization, and UI responsibilities where the repository structure permits.
- Keep configuration, thresholds, column mappings, and feature definitions centralized.
- Avoid duplicating logic across pages.
- Validate input and handle errors with useful messages.
- Never commit credentials, tokens, private dataset contents, or sensitive information.
- Do not add dependencies without explaining why they are needed.
- Do not remove or rewrite working functionality unrelated to the requested change.
- Do not create fake data, fake evaluation scores, or mock detections in production paths.
- Add or update tests for meaningful behavior and bug fixes when test infrastructure exists.
- Maintain consistent code, documentation, and use-case naming.

## 14. How Claude Should Work

Before coding:

1. Inspect the repository, files, dependencies, and relevant existing implementation.
2. Identify the relevant use cases and scope.
3. Summarize the intended change and any assumptions.
4. Identify whether the task depends on an unresolved project decision.

While coding:

1. Make the smallest complete change that satisfies the request.
2. Do not redesign unrelated parts of the system.
3. Preserve compatibility with existing data/model artifacts when possible.
4. Enforce permissions and validate inputs.
5. Keep changes readable and appropriate for the team's skill level.

After coding:

1. Summarize files changed and behavior.
2. Explain how to run or verify the change.
3. Report tests run and their outcomes; do not claim tests were run if they were not.
4. Mention any assumptions, unresolved decisions, or limitations.
5. Identify any impact on use cases or documentation.

## 15. Decisions Still Marked TBD

Do not treat the following as final without explicit confirmation:

- GCN vs GraphSAGE
- Edge weight measure (packets vs bytes)
- GNN classification threshold (0.5 is documented as a default, not a confirmed final value)
- Rule thresholds and exact rules
- Whether rule-based detections also drive suspicious-node highlighting
- Whether Review Alert is retained in the final dashboard
- Dataset replacement behavior
- Maximum CSV file size
- Minimum password length
- Other unspecified implementation details such as storage/database choice

When a task requires one of these decisions, clearly identify the decision and ask for confirmation, or isolate it as configurable with the uncertainty documented.

## 16. Priority

When instructions conflict, follow this order:

1. The user's explicit instructions in the current task.
2. The repository's existing, clearly established implementation decisions.
3. The latest approved project requirements/use-case documents.
4. This file.
5. General best practices, only when they do not conflict with project scope.

Never silently claim a tentative requirement is confirmed.

## Network & GNN-Specific Constraints

### Graph Construction

- Each node represents a unique IoT device identified by its IP address.
- Each edge represents communication between devices.
- Graph directionality must remain consistent with the approved graph design and the GNN training pipeline.
- Edge weights must be based on a documented traffic measure, such as packets or bytes, when weighting is used.
- Repeated communication records must be aggregated consistently with the graph design.
- Do not introduce self-loops, remove isolated nodes, change edge semantics, or modify graph structure without verifying compatibility with the model.
- Graph construction, node feature extraction, and GNN inference must remain consistent.

### Node Features

- Use only the approved node feature set.
- Feature names, order, dimensions, data types, and preprocessing must remain consistent with the trained model.
- Do not add, remove, or reorder features without updating and verifying the training and inference pipelines.
- Do not use ground-truth labels as model input features.
- Avoid data leakage during feature extraction, normalization, and preprocessing.
- Use the same normalization and preprocessing logic used during training.
- Do not silently change missing-value handling between training and inference.

### GNN Model

- The project uses a GNN for node classification.
- Each device is classified into Normal or Suspicious.
- The model input consists of node features and graph connectivity according to the approved training design.
- Use only the approved GNN architecture (GCN or GraphSAGE).
- Do not switch architectures, add additional GNN models, or modify model architecture without explicit approval.
- Training is separate from ordinary dashboard inference.
- Do not retrain the model every time the user runs anomaly detection unless explicitly requested.
- Model artifacts must be compatible with the model architecture, feature schema, and preprocessing pipeline.
- If a model artifact is missing, corrupted, or incompatible, report the problem clearly instead of generating fake predictions.
- Keep classification thresholds centralized and configurable.
- Treat the documented 0.5 threshold as provisional until explicitly confirmed.

### Training and Evaluation Integrity

- Keep training and testing data appropriately separated.
- Do not use test data for model training.
- Do not use ground-truth labels as inference inputs.
- Prevent data leakage through preprocessing, feature extraction, scaling, or data splitting.
- Document the dataset split strategy.
- Evaluation must use genuine predictions and appropriate ground-truth labels.
- Report Accuracy, Precision, Recall, F1-score, and Confusion Matrix when applicable.
- Do not rely on Accuracy alone for anomaly detection evaluation.
- Do not claim that the GNN is better than the rule-based baseline unless the measured results support that conclusion.

### Detection Consistency

- GNN detection and rule-based detection must remain separate methods.
- Each method must maintain its own prediction results.
- Do not combine predictions into a single classification unless explicitly approved.
- Both methods must be compared using the same devices and ground-truth labels when available.
- Keep rule definitions and thresholds centralized.
- Do not fabricate predictions, evaluation metrics, or detection results.

### Reproducibility and Model Integrity

- Preserve model training configuration, feature schema, and preprocessing compatibility.
- Avoid undocumented changes to model parameters, feature definitions, or thresholds.
- Do not overwrite trained model artifacts without explicit authorization.
- Maintain clear separation between model training, model artifacts, inference, and evaluation.
- When modifying the graph or feature pipeline, verify its compatibility with the trained model before using it for detection.
