PayrollGuard — Intelligent Reconciliation & Input Control
Final project for the Building AI course

Summary
PayrollGuard detects, links and helps reconcile inconsistent payroll input data before pay runs. It combines deterministic validation rules, fuzzy record linkage, ML-based anomaly detection and a human‑in‑the‑loop reconciliation UI to reduce payroll errors, save time and improve auditability.

Background
Which problems does your idea solve? How common or frequent is this problem? What is your personal motivation? Why is this topic important or interesting?

Payroll operations routinely ingest inputs from many systems and manual edits. That creates frequent data issues that are costly and time-consuming to fix.

Common problems:

Duplicate or missing records (employee appears under multiple name variants).
Mismatched identifiers (bank details, tax IDs, payroll IDs).
Unexpected pay/journal amounts (overtime spikes, retroactive corrections).
Manual late edits introduced just before a pay run.
Why it matters:

Payroll errors lead to refunds, bank fees, regulatory penalties and employee dissatisfaction.
Even small error rates in large organizations create significant operational cost.
Auditing and traceability are legally required in many jurisdictions.
Personal motivation:

Reduce tedious manual reconciliation work and prevent avoidable financial and reputational harm.
Provide transparent, auditable workflows payroll teams can trust.
How is it used?
Describe the process of using the solution. In what kind situations is the solution needed (environment, time, etc.)? Who are the users, what kinds of needs should be taken into account?

Typical usage flow:

Pre‑pay‑run ingestion: connect to HRIS, timekeeping and payroll staging feeds; normalize records.
Automatic checks: apply hard rules (missing tax ID, invalid bank) and compute candidate matches with similarity scores.
Triage dashboard: flagged items sorted by risk (High / Medium / Low). Suggested reconciliations shown with rationale.
Human review: payroll clerks accept/adjust suggestions or escalate; their decisions are recorded (active learning).
Finalize & apply: approved changes flow into the payroll system; audit log created for every action.
Who uses it and needs to be considered:

Payroll clerks — need clear, actionable suggestions; low false positive rate; undo/rollback.
Payroll managers — need KPIs, bulk acceptance workflows, role-based approvals.
HR operations — need root‑cause analytics for upstream data quality issues.
Finance / audit — require full audit trail and exportable reports.
UI features (planned):

Case detail with side‑by‑side comparison and similarity scores.
Batch actions + simulation mode and rollback.
Explainability panel (why this was flagged).
Feedback/labeling to improve models (active learning).
Integration points:

HRIS, timekeeping systems, ERP/GL, banking/payout systems, SSO/Identity providers.
Example usage (pseudo-code):

# ingest normalized CSVs
hr = load_csv('hr_master.csv')
timesheets = load_csv('timesheets.csv')
pay_inputs = load_csv('pay_stage.csv')

# run deterministic checks and blocking
flags = validate_and_block(pay_inputs)
candidates = create_candidate_pool(pay_inputs, hr)

# compute similarity and scores
scores = score_pairs(candidates)

# triage and present top-N to payroll clerks
present_cases(scores, flags)
Data sources and AI methods
Where does your data come from? Do you collect it yourself or do you use data collected by someone else?

Data sources (sensitive PII — treat carefully):

Master data: employee/contractor records, bank last4, tax ID, DOB, addresses.
Transactional: timesheets, payroll journal entries, pay rates, deductions.
Change logs: HR edits, onboarding/offboarding events, free‑text edit reasons.
Reference: tax/benefit tables, approved pay ranges.
AI & algorithm mix:

Deterministic rules: validation & hard blocks for immediate rejects.
Record linkage / entity resolution: blocking + probabilistic linkage (Fellegi–Sunter) or supervised classifiers (features: name similarity, DOB, ID match, address).
Fuzzy matching: RapidFuzz / Jaro‑Winkler / phonetic encodings (Soundex/Metaphone).
Anomaly detection: Isolation Forest, robust z‑scores or clustering to detect hours/pay spikes.
Supervised models: XGBoost or logistic regression to predict “needs review” using historical reconciliations.
NLP (optional): TF‑IDF or sentence embeddings for free‑text reason similarity.
Human‑in‑the‑loop / active learning: uncertain cases labeled by staff are used to retrain models.
Explainability: feature importance, rule provenance and short natural‑language explanations for each suggested action.
Privacy note:

PII must be encrypted at rest and in transit. Use role‑based access and pseudonymization for model training when possible.
Challenges
What does your project not solve? Which limitations and ethical considerations should be taken into account when deploying a solution like this?

Limitations & risks:

Not a replacement for human judgment on legal, union or complex pay rules — high‑risk corrections should remain manual.
Incomplete or poor master data reduces matching accuracy.
False positives waste payroll team time; false negatives let errors pass.
Concept drift (changes in payroll rules, org structure) requires retraining and governance.
Pairwise linkage scales poorly without blocking/indexing for large organizations.
Ethical & compliance considerations:

Strong privacy controls, data retention policies and role‑based access required (GDPR/other local laws).
Transparent explainability is necessary to earn users’ trust.
Keep auditable logs of automated actions and require approvals for payout‑affecting fixes.
What next?
How could your project grow and become something even more? What kind of skills, what kind of assistance would you need to move on?

Phased roadmap:

MVP: CSV ingestion, deterministic checks, blocking + fuzzy linkage, simple web triage UI, audit log.
Productionize: API integrations with HRIS/timekeeping, role‑based access, encryption, monitoring and CI/CD.
Improve models: active learning loop, supervised matching model, anomaly detector ensemble.
Scale & automation: safe auto‑apply rules for low‑risk fixes, rollback, simulation and batch scheduling.
Advanced: root‑cause analytics, drift detection, domain adapters for major payroll platforms, federated privacy-preserving training.
Skills & resources needed:

Payroll domain experts for rules and edge cases.
Data engineers for secure ingestion and blocking/indexing.
ML engineers for models and active learning pipelines.
Frontend/backend developers for reconciliation UI and integrations.
Legal/compliance for retention, access and data‑handling policies.
Acknowledgments
Inspirations: probabilistic record linkage literature (Fellegi–Sunter) and D. Christen’s work on entity resolution.
Open source tools & libraries to consider: python-record-linkage, Dedupe, RapidFuzz, scikit‑learn, XGBoost, sentence-transformers.
Implementation note: do not include or expose real PII in demos; when using public or synthetic datasets, document provenance and licences.
