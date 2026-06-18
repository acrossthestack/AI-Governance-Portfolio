# Risk Assessment: Snowflake Cortex

| | |
|---|---|
| **Platform** | Snowflake Cortex (Snowflake AI Data Cloud) |
| **Assessment Type** | AI Platform Risk Assessment (pre-approval) |
| **Related Document** | [Platform Assessment: Snowflake Cortex](../01-Platform-Assessments/Snowflake-Cortex.md) |
| **Prepared by** | Michaella Njuakom, AI Governance Lead |
| **Date** | June 2026 |
| **Status** | Draft — for risk review board |

---

## 1. Purpose and Scope

This document assesses the risks associated with adopting Snowflake Cortex as an enterprise AI platform, prior to organisational approval. It covers Cortex's in-platform AI capabilities (AISQL/LLM functions, Cortex Analyst, Cortex Search, Document AI), its agentic capabilities (Cortex Agents, Snowflake Intelligence), and its external integrations (Microsoft Teams/M365 Copilot, Slack, and MCP connectors to systems such as Salesforce, Jira, and Google Workspace).

**Out of scope:** risks of the underlying Snowflake data platform itself (assumed already assessed and approved), and use-case-specific risks, which will be assessed individually as use cases are proposed.

## 2. Methodology

Risks are assessed at **event level** — each entry is a specific failure mode that could occur, rather than a broad risk domain. Risks are grouped under the eight risk domains required for this assessment (Security, Privacy, Compliance, Third-Party/Vendor, Data Protection, Human Oversight, Operational, Reputational) so that each domain is visibly covered, but every individual risk carries its own description, impact, likelihood, mitigation, owner, and rating. This event-level structure supports direct traceability from each risk to a specific control in the Control Library.

**Likelihood** is rated High / Medium / Low based on the probability of the risk materialising within the first 12 months of adoption, assuming no mitigations beyond vendor defaults.

**Impact** is rated High / Medium / Low based on the worst credible outcome for the organisation (financial, regulatory, operational, or reputational).

**Risk ratings** are *inherent* (pre-mitigation), derived from the matrix below. Residual ratings will be assigned after controls are implemented and tested.

| | Impact: Low | Impact: Medium | Impact: High |
|---|---|---|---|
| **Likelihood: High** | Medium | High | Critical |
| **Likelihood: Medium** | Low | Medium | High |
| **Likelihood: Low** | Low | Low | Medium |

Each risk carries an ID (R-01, R-02, …) used for cross-reference from the Control Library.

---

## 3. Risk Register

### Security

#### R-01 — Unauthorised access via default-on entitlement

| Field | Detail |
|---|---|
| **Risk Description** | Snowflake grants the CORTEX_USER and COPILOT_USER database roles to the PUBLIC role by default, meaning every user in the account can invoke Cortex AI functions until access is explicitly revoked. Out of the box, the platform is allow-by-default. |
| **Potential Impact** | Any user can query data through AI functions regardless of business need; sensitive data reachable by users who should not have AI access; least-privilege violated from day one. |
| **Likelihood** | High |
| **Impact** | High |
| **Inherent Rating** | Critical |
| **Proposed Mitigation** | Revoke CORTEX_USER and COPILOT_USER from PUBLIC; grant Cortex access only through approved custom roles on documented business need (deny-by-default). |
| **Risk Owner** | Chief Information Security Officer (CISO) / Security Engineering |

#### R-02 — Data exfiltration via AI functions and agents

| Field | Detail |
|---|---|
| **Risk Description** | AI functions and agents can surface data in generated outputs, and agents can move data into connected systems. Sensitive data may be exposed to users or transmitted beyond intended boundaries. |
| **Potential Impact** | Confidential or regulated data exposed through AI outputs or sent to external systems; loss of control over where data travels. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | RBAC restriction of AI functions; masking upstream of AI processing; DLP monitoring on integration egress; agent permission scoping. |
| **Risk Owner** | CISO / Data Governance |

#### R-03 — Prompt injection

| Field | Detail |
|---|---|
| **Risk Description** | Malicious instructions embedded in documents or data ingested by LLM functions or agents manipulate their behaviour — a novel attack vector that traditional perimeter and access controls do not address. |
| **Potential Impact** | Agents manipulated into unintended actions (e.g., unauthorised record changes in Salesforce or Jira); data disclosure; corrupted outputs. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Treat all agent-ingested content as untrusted input; include prompt-injection scenarios in security testing; least-privilege agent scoping; human-in-the-loop for consequential actions; guardrails. |
| **Risk Owner** | CISO / Security Engineering |

#### R-04 — Integration credential compromise

| Field | Detail |
|---|---|
| **Risk Description** | The Teams, Slack, and MCP integrations rely on credentials and service accounts that, if compromised, grant access across the connected systems. |
| **Potential Impact** | Lateral access into connected enterprise systems; data access or manipulation under a trusted identity. |
| **Likelihood** | Low |
| **Impact** | High |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Scoped service accounts with least privilege; secret management and rotation; security review and penetration testing of each integration before enablement. |
| **Risk Owner** | CISO / Security Engineering |

### Privacy

#### R-05 — Processing of personal data without lawful basis

| Field | Detail |
|---|---|
| **Risk Description** | Personal data held in Snowflake may be processed by LLM functions without a defined lawful basis or compatibility with the purpose for which it was collected. Natural-language interfaces make it easy for non-technical users to query personal data without recognising the implications. |
| **Potential Impact** | UK/EU GDPR infringement exposure (fines up to 4% of global turnover); data-subject complaints; mandatory breach notification. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | DPIA before processing personal data with Cortex; restrict AI functions on schemas containing personal data; update privacy notices and records of processing; DPO involvement in use-case approval. |
| **Risk Owner** | Data Protection Officer (DPO) |

#### R-06 — PII exposure in prompts and outputs

| Field | Detail |
|---|---|
| **Risk Description** | Personal identifiers may be passed into prompts and returned in AI-generated outputs, or surfaced to users who would not normally be permitted to see them. |
| **Potential Impact** | Unauthorised disclosure of personal data; privacy breach; erosion of data-subject trust. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Apply masking and the AI_REDACT function to PII upstream of AI processing; restrict outputs by role; classification gating before AI enablement. |
| **Risk Owner** | Data Protection Officer (DPO) |

### Compliance

#### R-07 — Unclassified use cases reaching production

| Field | Detail |
|---|---|
| **Risk Description** | Because Cortex makes AI deployment fast and self-service, use cases can reach production without EU AI Act risk classification or the documentation required by emerging standards (ISO/IEC 42001, NIST AI RMF). |
| **Potential Impact** | Regulatory enforcement or forced withdrawal of non-compliant use cases; failed audits; inability to evidence compliance. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Mandatory AI use-case intake and classification (mapped to EU AI Act risk tiers) before production; maintain an AI use-case inventory; align documentation to NIST AI RMF and ISO/IEC 42001. |
| **Risk Owner** | Head of Compliance / AI Governance Lead |

#### R-08 — Missing audit evidence for AI activity

| Field | Detail |
|---|---|
| **Risk Description** | Without comprehensive logging of AI interactions, the organisation cannot evidence how AI was used, by whom, and against what data — undermining audits and incident investigation. |
| **Potential Impact** | Failed audits; inability to investigate incidents or demonstrate compliance; regulatory criticism. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Enable and retain comprehensive AI interaction logs (function calls, role, target objects, model, timestamps); minimum 12-month retention; export to central log platform. |
| **Risk Owner** | Head of Compliance / Data Platform |

### Third-Party / Vendor

#### R-09 — Model or service deprecation

| Field | Detail |
|---|---|
| **Risk Description** | Snowflake or a foundation-model provider may deprecate, change, or re-version a model or service at its discretion, disrupting dependent production workloads. |
| **Potential Impact** | Production disruption; output-quality changes; rework to migrate to alternative models. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Maintain a model inventory with documented fallback models per use case; monitor vendor change notifications; assess change impact before it reaches production. |
| **Risk Owner** | Procurement / Vendor Risk Management |

#### R-10 — Unreviewed third-party terms on data handling

| Field | Detail |
|---|---|
| **Risk Description** | Data sent through the Teams, Slack, and MCP integrations is governed by third-party terms; Snowflake disclaims responsibility for data once it leaves its service boundary. Per-model terms also govern how foundation-model providers may handle data. |
| **Potential Impact** | Data handled under terms the organisation has not reviewed; uncertainty over whether data is used for model training; unmanaged third-party data exposure. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Legal review of Snowflake's AI terms and per-model terms (confirm data not used for training); review Microsoft/Slack terms for integration flows; record outcomes in vendor management. |
| **Risk Owner** | Procurement / Legal |

### Data Protection

#### R-11 — Sensitive data enabled for AI without classification

| Field | Detail |
|---|---|
| **Risk Description** | Datasets may be enabled for AI processing without prior sensitivity classification, allowing confidential or regulated data into AI functions without appropriate restriction. |
| **Potential Impact** | Exposure of confidential or regulated data through AI processing; breach of internal data-handling policy. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Mandatory data classification before AI enablement; DPO sign-off for Confidential-and-above data; masking/row-level security applied ahead of AI functions. |
| **Risk Owner** | Chief Data Officer (CDO) / Data Governance |

#### R-12 — Sensitive data persisting in AI-derived artifacts

| Field | Detail |
|---|---|
| **Risk Description** | Sensitive data may be embedded into search indexes or fine-tuned models and persist beyond retention policies, outside the controls applied to the source data. |
| **Potential Impact** | Sensitive data retained beyond policy in derived artifacts; data-subject deletion requests not fully honoured; residual exposure. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Extend retention and deletion policies to AI-derived artifacts (search indexes, fine-tuned models); inventory derived artifacts; periodic review. |
| **Risk Owner** | Chief Data Officer (CDO) / Data Governance |

#### R-13 — Data-residency breach via model processing region

| Field | Detail |
|---|---|
| **Risk Description** | Foundation-model availability varies by cloud region; processing may occur in a region that conflicts with UK/EU GDPR or contractual data-residency obligations. |
| **Potential Impact** | Breach of data-residency commitments to customers or regulators; regulatory exposure. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Verify model processing region against residency obligations before enabling each model; disable cross-region inference where obligations would be breached; Legal/DPO sign-off. |
| **Risk Owner** | Chief Data Officer (CDO) / DPO |

### Human Oversight

#### R-14 — Over-trust of hallucinated or inaccurate outputs

| Field | Detail |
|---|---|
| **Risk Description** | Users may act on fluent but incorrect AI outputs — for example, inaccurate SQL generated by Cortex Analyst or fabricated answers from LLM functions — without verifying them. The natural-language interface invites non-technical users who may not recognise errors. |
| **Potential Impact** | Business decisions based on incorrect AI outputs; erosion of decision quality at scale. |
| **Likelihood** | High |
| **Impact** | High |
| **Inherent Rating** | Critical |
| **Proposed Mitigation** | Validate semantic models with accuracy testing before business rollout; mandatory user training emphasising output verification; citation review as standard practice; clear accountability that the human user owns AI-assisted decisions. |
| **Risk Owner** | AI Governance Lead / Business Unit Heads |

#### R-15 — Autonomous agent actions without human review

| Field | Detail |
|---|---|
| **Risk Description** | Cortex Agents can execute consequential actions — updating records, creating tickets, sending communications — in external systems without a human in the loop. |
| **Potential Impact** | Erroneous or unauthorised automated actions in connected systems; actions that cannot be easily reversed; unclear accountability. |
| **Likelihood** | High |
| **Impact** | High |
| **Inherent Rating** | Critical |
| **Proposed Mitigation** | Human-in-the-loop approval required for all consequential agent actions (write actions, customer-facing communications, actions on financial/personal data); agents restricted to draft-and-propose mode for these action types; agent action logging and review. |
| **Risk Owner** | AI Governance Lead / Business Unit Heads |

#### R-16 — Bias in AI outputs

| Field | Detail |
|---|---|
| **Risk Description** | Foundation models or Cortex Analyst semantic models may produce skewed or unfair results, with particular impact where outputs inform decisions about individuals (e.g., customers or employees). |
| **Potential Impact** | Unfair or discriminatory outcomes; reputational and regulatory exposure; harm to affected individuals. |
| **Likelihood** | Medium |
| **Impact** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Bias and quality testing before any deployment informing decisions about people; human review of such outputs; restrict high-stakes use cases pending use-case classification. |
| **Risk Owner** | AI Governance Lead |

### Operational

#### R-17 — Uncontrolled consumption cost

| Field | Detail |
|---|---|
| **Risk Description** | Cortex uses consumption-based billing; usage can scale unpredictably ("bill shock") as AI features are adopted across the organisation. |
| **Potential Impact** | Significant unbudgeted spend; loss of cost predictability. |
| **Likelihood** | High |
| **Impact** | Medium |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Budgets and resource monitors with alert thresholds; role-based entitlement so AI functions are a deliberate grant; regular consumption review. |
| **Risk Owner** | Head of Data Platform / FinOps |

#### R-18 — Shadow AI usage outside governance

| Field | Detail |
|---|---|
| **Risk Description** | Self-service access enables teams to adopt AI features outside approved, governed channels, creating workloads invisible to oversight. |
| **Potential Impact** | Ungoverned AI workloads; unassessed risk; inconsistent control application. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Usage anomaly and shadow-adoption monitoring; role-based entitlement; use-case intake requirement; regular usage reviews. |
| **Risk Owner** | Head of Data Platform / Security Operations |

#### R-19 — Preview-feature instability in production

| Field | Detail |
|---|---|
| **Risk Description** | Many Cortex capabilities are in preview and may change or be withdrawn; adopting them into production processes risks disruption when they change. |
| **Potential Impact** | Business-process disruption; rework; unreliable production behaviour. |
| **Likelihood** | Medium |
| **Impact** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Policy restricting preview features to non-production use; document feature/model dependencies per use case with fallback plans; track preview-to-GA transitions. |
| **Risk Owner** | Head of Data Platform |

### Reputational

#### R-20 — Harmful or biased public-facing output

| Field | Detail |
|---|---|
| **Risk Description** | A harmful, biased, or visibly wrong AI output in a customer- or employee-facing context can damage trust. As an early adopter, missteps may attract disproportionate attention. |
| **Potential Impact** | Loss of customer trust and brand damage; negative coverage; increased regulatory scrutiny; internal loss of confidence slowing the AI programme. |
| **Likelihood** | Low |
| **Impact** | High |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Guardrails and output filtering enabled; bias and quality testing before customer-facing deployment; phased rollout starting with internal low-stakes use cases; transparency about AI involvement; AI incident response plan with communications protocol. |
| **Risk Owner** | Chief Risk Officer / Communications |

---

## 4. Risk Summary

| ID | Risk | Domain | Likelihood | Impact | Inherent Rating |
|---|---|---|---|---|---|
| R-01 | Unauthorised access via default-on entitlement | Security | High | High | **Critical** |
| R-02 | Data exfiltration via AI functions and agents | Security | Medium | High | **High** |
| R-03 | Prompt injection | Security | Medium | High | **High** |
| R-04 | Integration credential compromise | Security | Low | High | **Medium** |
| R-05 | Processing of personal data without lawful basis | Privacy | Medium | High | **High** |
| R-06 | PII exposure in prompts and outputs | Privacy | Medium | High | **High** |
| R-07 | Unclassified use cases reaching production | Compliance | Medium | High | **High** |
| R-08 | Missing audit evidence for AI activity | Compliance | Medium | Medium | **Medium** |
| R-09 | Model or service deprecation | Third-Party / Vendor | Medium | Medium | **Medium** |
| R-10 | Unreviewed third-party terms on data handling | Third-Party / Vendor | Medium | Medium | **Medium** |
| R-11 | Sensitive data enabled for AI without classification | Data Protection | Medium | High | **High** |
| R-12 | Sensitive data persisting in AI-derived artifacts | Data Protection | Medium | Medium | **Medium** |
| R-13 | Data-residency breach via model processing region | Data Protection | Medium | High | **High** |
| R-14 | Over-trust of hallucinated or inaccurate outputs | Human Oversight | High | High | **Critical** |
| R-15 | Autonomous agent actions without human review | Human Oversight | High | High | **Critical** |
| R-16 | Bias in AI outputs | Human Oversight | Medium | High | **High** |
| R-17 | Uncontrolled consumption cost | Operational | High | Medium | **High** |
| R-18 | Shadow AI usage outside governance | Operational | Medium | Medium | **Medium** |
| R-19 | Preview-feature instability in production | Operational | Medium | Medium | **Medium** |
| R-20 | Harmful or biased public-facing output | Reputational | Low | High | **Medium** |

**Headline finding:** the three Critical risks — R-01 (default-on access), R-14 (over-trust of outputs), and R-15 (autonomous agent actions) — cluster around a single theme: Cortex's core design makes AI immediately available to all users and lets agents act autonomously. The platform's most celebrated features are also its largest governance exposures, and the mitigations are largely organisational (revoking defaults, training, approval gates) rather than purely technical.

A second theme across the Security, Data Protection, and Vendor risks is the **Snowflake boundary**: exposure is materially lower for in-platform capabilities, which inherit existing governance, and concentrates where data or actions cross into external systems via the Teams, Slack, and MCP integrations.

## 5. Conclusion and Next Steps

No identified risk is assessed as unmanageable; all are addressable through the proposed mitigations, consistent with the phased adoption recommended in the Platform Assessment. Recommended next steps:

1. Risk review board to validate ratings and confirm risk owners
2. Mitigations to be converted into formal controls in the Control Library (folder 03), with each control traceable to one or more risk IDs above
3. DPIA initiated before any personal data is processed by Cortex (R-05, R-06)
4. Residual risk ratings to be assigned after Phase 1 controls are implemented and tested
5. This assessment to be reviewed quarterly, or upon material platform changes (new integrations, new model availability, features moving from preview to GA)

---

*Prepared as part of the AI Governance Portfolio. Methodology designed by the author in the absence of an organisational standard; risks assessed at event level to support direct traceability to controls.*
