# Control Library: Snowflake Cortex
| | |
|---|---|
| **Platform** | Snowflake Cortex (Snowflake AI Data Cloud) |
| **Document Type** | AI Platform Control Library |
| **Related Documents** | [Platform Assessment](../01-Platform-Assessments/Snowflake-Cortex.md) · [Risk Assessment](../02-Risk-Assessments/Snowflake-Cortex.md) |
| **Prepared by** | Michaella Njuakom, AI Governance Lead |
| **Date** | June 2026 |
| **Status** | Draft — for control owner sign-off |
## 1. Purpose

This control library defines the controls required to operate Snowflake Cortex within the organisation's risk tolerance. Each control is traceable to a risk identified in the [Risk Assessment](../02-Risk-Assessments/Snowflake-Cortex.md) — every "Proposed Mitigation" in that document is formalised here as an implementable, auditable control.

The library deliberately separates **traditional controls** — established IT controls applied to the Cortex platform — from **AI-specific controls**, which exist only because of how AI systems behave. This distinction matters: most of governing an AI platform is classic control practice (access management, logging, vendor review), with a smaller layer of genuinely new controls addressing AI-specific risks such as autonomous agent actions, model output reliability, and AI regulation.
## 2. Control ID Scheme

Two ID schemes are used:

| Prefix | Meaning | Examples |
|---|---|---|
| **AC-, DP-, MON-, LOG-, TPR-** | Traditional controls, prefixed by category (Access Control, Data Protection, Monitoring, Logging, Third Party Risk) | AC-01, LOG-01 |
| **SNOWC-** | AI-specific controls for Snowflake Cortex | SNOWC-01 |

Each control is documented with: category, control type (Preventive / Detective), the risk it mitigates (cross-referenced to the Risk Assessment), a NIST SP 800-53 Rev. 5 mapping where applicable, and a standardised implementation statement covering **who** implements it, **how** it works, **when** it occurs, **who** reviews results, and **what** evidence demonstrates operation.

## 3. Control Summary

### Traditional controls

| ID | Control Name | Category | Type | Mitigates Risk |
|---|---|---|---|---|
| AC-01 | Role-Based Entitlement to AI Functions | Access Control | Preventive | 3.1 Security |
| AC-02 | Privileged AI Role Access Review | Access Control | Detective | 3.1 Security |
| DP-01 | PII Masking Before AI Processing | Data Protection | Preventive | 3.2 Privacy |
| DP-02 | Data Classification Gate for AI Enablement | Data Protection | Preventive | 3.5 Data Protection |
| MON-01 | AI Consumption Budgets and Resource Monitors | Monitoring | Detective | 3.7 Operational |
| MON-02 | AI Usage Anomaly and Shadow-Adoption Monitoring | Monitoring | Detective | 3.1 Security / 3.7 Operational |
| LOG-01 | AI Interaction Audit Logging | Logging | Detective | 3.1 Security / 3.3 Compliance |
| TPR-01 | Vendor and Model Terms Review | Third Party Risk | Preventive | 3.4 Third-Party / Vendor |
| TPR-02 | Vendor Change and Deprecation Monitoring | Third Party Risk | Detective | 3.4 Third-Party / Vendor |

### AI-specific controls

| ID | Control Name | Category | Type | Mitigates Risk |
|---|---|---|---|---|
| SNOWC-01 | Least-Privilege Agent and Connector Scoping | Access Control | Preventive | 3.1 Security |
| SNOWC-02 | Data Residency Verification per Model | Data Protection | Preventive | 3.5 Data Protection |
| SNOWC-03 | Agent External Action Log Review | Logging | Detective | 3.6 Human Oversight |
| SNOWC-04 | External Connector Approval Process | Third Party Risk | Preventive | 3.4 Vendor / 3.5 Data Protection |
| SNOWC-05 | Human-in-the-Loop Gate for Consequential Agent Actions | Human Oversight | Preventive | 3.6 Human Oversight |
| SNOWC-06 | Semantic Model Validation Before Business Rollout | Human Oversight | Preventive | 3.6 Human Oversight |
| SNOWC-07 | AI Acceptable Use Training | Human Oversight | Preventive | 3.6 Human Oversight |
| SNOWC-08 | LLM Guardrails Enforcement | Responsible AI | Preventive | 3.8 Reputational |
| SNOWC-09 | AI Use-Case Intake and Risk Classification | Responsible AI | Preventive | 3.3 Compliance |

Traditional control IDs are internal identifiers and do not correspond to NIST 800-53 control numbers; official NIST mappings are stated separately on each control.

Each control is documented with: category, control type (Preventive / Detective), the risk it mitigates (cross-referenced to the Risk Assessment), a NIST SP 800-53 Rev. 5 mapping where applicable, and a standardised implementation statement covering **who** implements it, **how** it works, **when** it occurs, **who** reviews results, and **what** evidence demonstrates operation.

---

## 4. Traditional Controls

### Access Control

#### AC-01 — Role-Based Entitlement to AI Functions
**Type:** Preventive · **Mitigates:** Risk 3.1 (Security) · **NIST 800-53:** AC-3, AC-6

The Data Platform team implements AC-01 by establishing deny-by-default access to Cortex: revoking the SNOWFLAKE.CORTEX_USER and SNOWFLAKE.COPILOT_USER database roles from the PUBLIC role — which Snowflake grants by default, enabling all users in the account to use Cortex AI functions — and granting Cortex access only through explicitly approved custom roles. The organization uses Snowflake role-based access control (RBAC) grants and database roles to ensure AI capabilities are a deliberate entitlement rather than the platform default. This occurs at platform configuration and upon every new role or user provisioning request. Results are reviewed by the Security Engineering team. Evidence includes the executed REVOKE statements showing CORTEX_USER and COPILOT_USER removed from PUBLIC, RBAC grant reports, role definition scripts in version control, and access provisioning tickets.

#### AC-02 — Privileged AI Role Access Review
**Type:** Detective · **Mitigates:** Risk 3.1 (Security) · **NIST 800-53:** AC-2, AC-6(7)

The Identity and Access Management (IAM) team implements AC-02 by recertifying all privileged roles with Cortex administration rights or broad AI function access. The organization uses its access governance tooling to generate entitlement reports and route them to role owners for attestation, with unattested access revoked. This occurs quarterly. Results are reviewed by the CISO and the AI Governance Lead. Evidence includes recertification campaign reports, attestation records, and revocation logs.

### Data Protection

#### DP-01 — PII Masking Before AI Processing
**Type:** Preventive · **Mitigates:** Risk 3.2 (Privacy) · **NIST 800-53:** SI-19, AC-3

The Data Governance team implements DP-01 by applying dynamic data masking policies and the AI_REDACT function to personal data before it can be passed to any Cortex LLM function. The organization uses Snowflake masking policies, tag-based policy enforcement, and AI_REDACT in approved pipelines to ensure personal identifiers are removed or obfuscated upstream of AI processing. This occurs continuously, enforced at query time on all tagged columns. Results are reviewed by the Data Protection Officer monthly. Evidence includes masking policy definitions, data tag inventories, and sample query results demonstrating masked output.

#### DP-02 — Data Classification Gate for AI Enablement
**Type:** Preventive · **Mitigates:** Risk 3.5 (Data Protection) · **NIST 800-53:** RA-2

The Data Governance team implements DP-02 by requiring every dataset to carry a completed sensitivity classification before Cortex functions may be enabled against it, with datasets classified Confidential or above requiring DPO sign-off. The organization uses Snowflake object tagging and an AI-enablement checklist within the data catalogue to block AI access to unclassified data. This occurs upon every request to enable a dataset for AI processing. Results are reviewed by the Chief Data Officer. Evidence includes classification tags in the data catalogue, AI-enablement checklists, and DPO approval records.

### Monitoring

#### MON-01 — AI Consumption Budgets and Resource Monitors
**Type:** Detective · **Mitigates:** Risk 3.7 (Operational) · **NIST 800-53:** no direct analog — operational/financial control (800-53 is a security catalogue and does not address cost management)

The FinOps team implements MON-01 by setting consumption budgets and alert thresholds on all Cortex AI workloads to prevent uncontrolled spend. The organization uses Snowflake resource monitors and budget objects to alert at 75% and suspend non-critical AI workloads at 100% of monthly budget. This occurs continuously, with thresholds reviewed monthly. Results are reviewed by the Head of Data Platform and Finance. Evidence includes resource monitor configurations, budget alert notifications, and monthly AI consumption reports.

#### MON-02 — AI Usage Anomaly and Shadow-Adoption Monitoring
**Type:** Detective · **Mitigates:** Risks 3.1 (Security), 3.7 (Operational) · **NIST 800-53:** AU-6, SI-4

The Security Operations team implements MON-02 by monitoring Cortex usage patterns for anomalies, including unusual query volumes, AI access to sensitive schemas, and AI usage by roles or teams with no approved use case. The organization uses Snowflake query history, account usage views, and SIEM correlation rules to detect deviations from approved usage baselines. This occurs continuously, with alerts triaged within one business day. Results are reviewed by the Security Operations lead weekly, with shadow-usage findings escalated to the AI Governance Lead. Evidence includes SIEM alert records, weekly monitoring reports, and shadow-usage investigation tickets.

### Logging

#### LOG-01 — AI Interaction Audit Logging
**Type:** Detective · **Mitigates:** Risks 3.1 (Security), 3.3 (Compliance) · **NIST 800-53:** AU-2, AU-12, AU-11

The Data Platform team implements LOG-01 by enabling and retaining comprehensive logs of all Cortex activity, including AI function calls, the invoking role, target objects, model used, and timestamps. The organization uses Snowflake account usage and access history views, exported to the central log platform, to maintain a tamper-evident audit trail of AI interactions. This occurs continuously, with logs retained for a minimum of 12 months in line with the organisation's retention schedule. Results are reviewed by Internal Audit during periodic audits and by Security Operations on demand. Evidence includes log export configurations, retention policy documentation, and sample audit trail extracts.

### Third Party Risk

#### TPR-01 — Vendor and Model Terms Review
**Type:** Preventive · **Mitigates:** Risk 3.4 (Third-Party / Vendor) · **NIST 800-53:** SA-9, SA-4

The Legal team, supported by Procurement, implements TPR-01 by reviewing Snowflake's AI service terms and the per-model terms for every foundation model before it is enabled, confirming in particular that customer data is not used for model training and that liability, data handling, and availability commitments meet organisational standards. The organization uses a vendor review checklist and contract repository to document each review. This occurs before initial platform approval, before each new model is enabled, and upon notification of terms changes. Results are reviewed by the Head of Legal and the Vendor Risk Manager. Evidence includes completed review checklists, contract repository entries, and a maintained model inventory with terms status.

#### TPR-02 — Vendor Change and Deprecation Monitoring
**Type:** Detective · **Mitigates:** Risk 3.4 (Third-Party / Vendor) · **NIST 800-53:** SA-9

The Data Platform team implements TPR-02 by monitoring Snowflake release notes, model deprecation notices, and terms-change notifications, and assessing the impact of each change on production AI workloads. The organization uses subscriptions to vendor communication channels and a change-impact log to ensure model deprecations or behaviour changes are identified before they disrupt dependent use cases, each of which has a documented fallback model. This occurs continuously, with a formal review of accumulated changes monthly. Results are reviewed by the Head of Data Platform, with material changes escalated to the AI Governance Lead. Evidence includes the change-impact log, fallback documentation per use case, and monthly review minutes.

---
## 5. AI-Specific Controls

### Access Control

#### SNOWC-01 — Least-Privilege Agent and Connector Scoping
**Type:** Preventive · **Mitigates:** Risk 3.1 (Security) · **NIST 800-53:** AC-6, CM-7

The Security Engineering team implements SNOWC-01 by scoping every Cortex Agent and MCP connector to the minimum permissions required for its approved use case, including read-only access wherever write actions are not explicitly justified. The organization uses connector-level permission configuration and scoped service accounts to ensure agents cannot access or modify systems beyond their defined task. This occurs at agent or connector deployment and upon any change to an agent's purpose. Results are reviewed by the CISO during quarterly access certification. Evidence includes connector permission configurations, service account scope documentation, and change approval records.

### Data Protection

#### SNOWC-02 — Data Residency Verification per Model
**Type:** Preventive · **Mitigates:** Risk 3.5 (Data Protection) · **NIST 800-53:** SA-9(5)

The Data Platform team implements SNOWC-02 by verifying the processing region of each foundation model before it is enabled, confirming compatibility with UK/EU GDPR and contractual data-residency obligations, and disabling cross-region inference where obligations would be breached. The organization uses Snowflake's model availability documentation and cross-region inference configuration settings to control where AI processing occurs. This occurs before each new model is enabled and upon Snowflake regional availability changes. Results are reviewed by the Data Protection Officer and Legal. Evidence includes a model-region register, configuration screenshots, and legal review sign-offs.

### Logging

#### SNOWC-03 — Agent External Action Log Review
**Type:** Detective · **Mitigates:** Risk 3.6 (Human Oversight) · **NIST 800-53:** AU-6(1), AC-6(9)

The AI Governance Lead implements SNOWC-03 by reviewing logs of all write actions performed by Cortex Agents in external systems (e.g., records updated in Salesforce, tickets created in Jira, messages sent to Teams or Slack). The organization uses agent action logs correlated with external system audit logs to verify that every automated action traces to an approved agent, an authorised trigger, and — where required by SNOWC-05 — a human approval. This occurs weekly during the pilot phase, moving to monthly sampling once controls are mature. Results are reviewed by the AI Governance Lead with exceptions reported to the risk review board. Evidence includes agent action log extracts, review checklists, and exception reports.

### Third Party Risk

#### SNOWC-04 — External Connector Approval Process
**Type:** Preventive · **Mitigates:** Risks 3.4 (Vendor), 3.5 (Data Protection) · **NIST 800-53:** CA-3, SA-9

The AI Governance Lead implements SNOWC-04 by requiring every external integration (Microsoft Teams, Slack, and each MCP connector) to pass an individual risk assessment and formal approval before enablement, recognising that data crossing the Snowflake boundary is governed by third-party terms rather than Snowflake's. The organization uses a connector assessment template covering data flows, third-party terms, permission scope, and DLP requirements, with approval recorded by the risk review board. This occurs before any connector is enabled and upon material changes to a connector's capabilities. Results are reviewed by the risk review board. Evidence includes completed connector assessments, approval minutes, and the connector register with approval status.

### Human Oversight

#### SNOWC-05 — Human-in-the-Loop Gate for Consequential Agent Actions
**Type:** Preventive · **Mitigates:** Risk 3.6 (Human Oversight) · **NIST 800-53:** AC-6(9) · **NIST AI RMF:** Manage 2.4

Business unit owners implement SNOWC-05 by requiring explicit human approval before a Cortex Agent executes any consequential action — defined as any write action to an external system, any customer-facing communication, or any action affecting financial or personal data. The organization uses agent workflow configuration to route consequential actions to a named approver, with the agent restricted to draft-and-propose mode for these action types. This occurs on every consequential action, with the definition of "consequential" reviewed quarterly by the AI Governance Lead. Results are reviewed by business unit owners, with approval rates and overrides reported monthly. Evidence includes agent workflow configurations, approval records, and monthly oversight reports.

#### SNOWC-06 — Semantic Model Validation Before Business Rollout
**Type:** Preventive · **Mitigates:** Risk 3.6 (Human Oversight) · **NIST AI RMF:** Measure 2.5

The Analytics Engineering team implements SNOWC-06 by testing every Cortex Analyst semantic model against a benchmark set of business questions with known correct answers before the model is released to business users. The organization uses a documented validation procedure requiring a minimum accuracy threshold (target ≥90% on the benchmark set) and sign-off from the data owner. This occurs before initial release and after any material change to the semantic model or underlying data. Results are reviewed by the Chief Data Officer. Evidence includes benchmark question sets, validation test results, and release sign-off records.

#### SNOWC-07 — AI Acceptable Use Training
**Type:** Preventive · **Mitigates:** Risk 3.6 (Human Oversight) · **NIST 800-53:** AT-2, AT-3

The Learning and Development team, with content owned by the AI Governance Lead, implements SNOWC-07 by requiring all users to complete AI acceptable-use training before being granted access to Cortex interfaces, covering output verification, hallucination risk, data sensitivity, and user accountability for AI-assisted decisions. The organization uses the corporate learning management system to assign, track, and gate training completion against access provisioning. This occurs before first access and annually thereafter. Results are reviewed by the AI Governance Lead, with completion rates reported quarterly. Evidence includes training content, LMS completion records, and access requests showing the training gate.

### Responsible AI

#### SNOWC-08 — LLM Guardrails Enforcement
**Type:** Preventive · **Mitigates:** Risk 3.8 (Reputational) · **NIST AI RMF:** Manage 1.3

The Data Platform team implements SNOWC-08 by enabling Cortex Guardrails on all LLM functions and agent deployments to filter harmful, unsafe, or inappropriate content from model outputs, with guardrails mandatory (non-disableable by users) on any interface exposed to business users or customers. The organization uses Cortex Guardrails configuration enforced through standardised deployment templates to ensure consistent application. This occurs at deployment of every AI function or agent, with configuration verified quarterly. Results are reviewed by the AI Governance Lead. Evidence includes deployment template code, guardrail configuration audits, and filtered-output sample logs.

#### SNOWC-09 — AI Use-Case Intake and Risk Classification
**Type:** Preventive · **Mitigates:** Risk 3.3 (Compliance) · **NIST AI RMF:** Map 1.1 · **EU AI Act:** risk-tier classification

The AI Governance Lead implements SNOWC-09 by requiring every proposed Cortex use case to pass through a formal intake process before development, classifying it against EU AI Act risk tiers and organisational risk appetite, and recording it in the AI use-case inventory with documentation requirements scaled to its tier. The organization uses an intake form and the AI use-case inventory to ensure no AI workload reaches production unclassified, with prohibited-tier use cases rejected at intake. This occurs upon every new use-case proposal and at annual re-review of the inventory. Results are reviewed by the AI governance committee. Evidence includes intake submissions, classification records, the use-case inventory, and committee approval minutes.

---

## 6. Maintenance

This library is reviewed quarterly by the AI Governance Lead and upon material platform changes — new Cortex capabilities, new external connectors, or features moving from preview to general availability. New controls follow their respective numbering sequences; retired controls are marked deprecated rather than deleted, preserving the audit trail.

---

*Prepared as part of the AI Governance Portfolio. Control statements follow a standard formula: who implements, how it works, when it occurs, who reviews, and what evidence demonstrates operation.*

