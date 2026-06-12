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
