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

**Likelihood** is rated High / Medium / Low based on the probability of the risk materialising within the first 12 months of adoption, assuming no mitigations beyond vendor defaults.

**Impact** is described qualitatively and rated High / Medium / Low based on the worst credible outcome for the organisation (financial, regulatory, operational, or reputational).

**Risk ratings** below are *inherent* (pre-mitigation). The expectation is that the proposed mitigations bring each risk within tolerance; residual ratings will be assigned after controls are implemented and tested.

| | Impact: Low | Impact: Medium | Impact: High |
|---|---|---|---|
| **Likelihood: High** | Medium | High | Critical |
| **Likelihood: Medium** | Low | Medium | High |
| **Likelihood: Low** | Low | Low | Medium |

---

## 3. Risk Register

### 3.1 Security Risk

| Field | Detail |
|---|---|
| **Risk Description** | Cortex expands the platform's attack surface. AI functions are callable by any role granted access, agentic features can execute actions in external systems, and the Teams/Slack/MCP integrations create new ingress and egress paths. Prompt injection — where malicious instructions embedded in documents or data manipulate LLM or agent behaviour — is a novel attack vector that traditional security controls do not address. |
| **Potential Impact** | Unauthorised data access or exfiltration via AI functions; agents manipulated into performing unintended actions in connected systems (e.g., Salesforce, Jira); compromised integration credentials. Impact: **High**. |
| **Likelihood** | Medium |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Restrict Cortex functions and agent capabilities to defined roles via RBAC (deny by default); enable Cortex Guardrails; security review and penetration testing of each external integration before enablement; treat all agent-ingested content as untrusted input; include prompt injection scenarios in security testing; monitor AI query and agent action logs for anomalies. |
| **Risk Owner** | Chief Information Security Officer (CISO) / Security Engineering |

### 3.2 Privacy Risk

| Field | Detail |
|---|---|
| **Risk Description** | Personal data held in Snowflake may be processed by LLM functions without a defined lawful basis, included in prompts and AI-generated outputs, or surfaced to users who would not normally access it. Natural-language interfaces make it easy for non-technical users to query personal data without realising the privacy implications. |
| **Potential Impact** | Processing of personal data incompatible with the purposes for which it was collected; UK/EU GDPR infringement exposure (fines up to 4% of global turnover); data subject complaints; mandatory breach notification. Impact: **High**. |
| **Likelihood** | Medium |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Complete a Data Protection Impact Assessment (DPIA) before processing personal data with Cortex; apply masking and the AI_REDACT function to PII upstream of AI processing; restrict AI functions on schemas containing personal data; update privacy notices and records of processing (RoPA); involve the DPO in use-case approval. |
| **Risk Owner** | Data Protection Officer (DPO) |

### 3.3 Compliance Risk

| Field | Detail |
|---|---|
| **Risk Description** | AI use cases built on Cortex may fall within scope of the EU AI Act (with obligations varying by risk tier), sector regulations, and emerging AI governance standards (ISO/IEC 42001, NIST AI RMF). Because Cortex makes AI deployment fast and self-service, use cases can reach production without regulatory classification or required documentation. |
| **Potential Impact** | Regulatory enforcement, fines, or forced withdrawal of non-compliant AI use cases; failed audits; inability to evidence compliance due to missing documentation. Impact: **High**. |
| **Likelihood** | Medium |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Mandatory AI use-case intake and classification process (mapped to EU AI Act risk tiers) before any Cortex workload reaches production; maintain an AI use-case inventory; align governance documentation to NIST AI RMF and ISO/IEC 42001; periodic compliance audits of deployed AI workloads. |
| **Risk Owner** | Head of Compliance / AI Governance Lead |

### 3.4 Third-Party / Vendor Risk

| Field | Detail |
|---|---|
| **Risk Description** | Cortex creates layered vendor dependency: on Snowflake as the platform operator, and on the foundation model providers (Anthropic, OpenAI, Meta, Mistral, Google, and others) whose models Snowflake hosts. Model availability, behaviour, pricing, and terms can change at the vendor's discretion. The Teams/Slack/MCP integrations additionally place data under third-party terms — Snowflake explicitly disclaims responsibility for data once it leaves its service boundary. |
| **Potential Impact** | Service or model deprecation disrupting production workloads; unexpected changes in model behaviour affecting output quality; data handled under third-party terms not reviewed by the organisation; concentration risk and reduced negotiating leverage. Impact: **Medium**. |
| **Likelihood** | Medium |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Legal review of Snowflake's AI terms, including per-model terms and data-use commitments (confirm customer data is not used for model training); review Microsoft/Slack terms for integration data flows; maintain a model inventory with documented fallback models per use case; include Cortex in vendor management and annual third-party risk reviews; monitor vendor change notifications. |
| **Risk Owner** | Procurement / Vendor Risk Management |

### 3.5 Data Protection Risk

| Field | Detail |
|---|---|
| **Risk Description** | Data sensitivity controls may not survive AI processing: sensitive data can be passed into prompts, embedded into fine-tuned models or search indexes, returned in generated outputs, or transmitted outside the Snowflake boundary via integrations. Model processing regions may also conflict with data-residency obligations, as model availability varies by cloud region. |
| **Potential Impact** | Exposure of confidential or regulated data through AI outputs or external integrations; breach of data-residency commitments to customers or regulators; sensitive data persisting in derived artifacts (indexes, fine-tuned models) beyond retention policies. Impact: **High**. |
| **Likelihood** | Medium |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Data classification enforced before datasets are enabled for AI processing; masking/row-level security applied ahead of AI functions; verify model processing regions against residency obligations before enabling each model; extend retention and deletion policies to AI-derived artifacts (search indexes, fine-tuned models); DLP monitoring on integration egress paths. |
| **Risk Owner** | Chief Data Officer (CDO) / Data Governance |

### 3.6 Human Oversight Risk

| Field | Detail |
|---|---|
| **Risk Description** | Cortex is designed to put AI in the hands of non-technical users and to let agents act autonomously (creating tickets, updating records, sending communications). Users may over-trust fluent AI outputs, act on hallucinated or inaccurate answers (e.g., incorrect SQL generated by Cortex Analyst), or allow agents to take consequential actions without review. Accountability for AI-assisted decisions may be unclear. |
| **Potential Impact** | Business decisions based on incorrect AI outputs; erroneous automated actions in connected systems; inability to attribute responsibility for AI-driven outcomes; erosion of decision quality at scale. Impact: **High**. |
| **Likelihood** | High |
| **Inherent Rating** | Critical |
| **Proposed Mitigation** | Human-in-the-loop required for all consequential agent actions (write actions to external systems gated behind approval); validated semantic models for Cortex Analyst with accuracy testing before business rollout; mandatory user training and acceptable-use policy emphasising verification of AI outputs; clear accountability statement — the human user owns decisions made with AI assistance; citation review encouraged as standard practice. |
| **Risk Owner** | AI Governance Lead / Business Unit Heads |

### 3.7 Operational Risk

| Field | Detail |
|---|---|
| **Risk Description** | Consumption-based AI billing can scale unpredictably as usage grows ("bill shock"), and self-service access enables shadow AI usage outside governed channels. Production workloads become dependent on Cortex service availability and on models that may be deprecated or updated. Rapid feature evolution (many capabilities in preview) creates instability risk if preview features are adopted into production processes. |
| **Potential Impact** | Significant unbudgeted spend; ungoverned AI workloads invisible to oversight; business process disruption from service incidents or model deprecations; rework when preview features change. Impact: **Medium**. |
| **Likelihood** | High |
| **Inherent Rating** | High |
| **Proposed Mitigation** | Budgets and resource monitors with alerts on AI consumption; role-based entitlement so AI functions are a deliberate grant, not a default; policy restricting preview features to non-production use; document model/feature dependencies per use case with fallback plans; regular usage reviews to detect shadow adoption. |
| **Risk Owner** | Head of Data Platform / FinOps |

### 3.8 Reputational Risk

| Field | Detail |
|---|---|
| **Risk Description** | Public-facing or employee-facing AI failures — harmful or biased outputs, a privacy incident involving AI processing, or a widely visible wrong answer from an AI assistant — can damage trust with customers, employees, and regulators. As an early-adopting organisation, missteps may attract disproportionate media and regulatory attention. |
| **Potential Impact** | Loss of customer trust and brand damage; negative press coverage; increased regulatory scrutiny; internal loss of confidence slowing the broader AI programme. Impact: **High**. |
| **Likelihood** | Low |
| **Inherent Rating** | Medium |
| **Proposed Mitigation** | Phased rollout starting with internal, low-stakes use cases (per the phased recommendation in the Platform Assessment); guardrails and output filtering enabled; bias and quality testing before any customer-facing deployment; AI incident response plan including communications protocol; transparency with users about AI involvement in interactions. |
| **Risk Owner** | Chief Risk Officer / Communications |

---

## 4. Risk Summary

| # | Risk Category | Likelihood | Impact | Inherent Rating |
|---|---|---|---|---|
| 3.1 | Security | Medium | High | **High** |
| 3.2 | Privacy | Medium | High | **High** |
| 3.3 | Compliance | Medium | High | **High** |
| 3.4 | Third-Party / Vendor | Medium | Medium | **Medium** |
| 3.5 | Data Protection | Medium | High | **High** |
| 3.6 | Human Oversight | High | High | **Critical** |
| 3.7 | Operational | High | Medium | **High** |
| 3.8 | Reputational | Low | High | **Medium** |

**Headline finding:** the highest-rated risk is **Human Oversight (Critical)** — not a traditional security or technical risk. This reflects Cortex's core design choice: making AI accessible to non-technical users and enabling autonomous agent actions. The platform's most celebrated feature is also its largest governance exposure, and the mitigations for it are organisational (training, approval gates, accountability) rather than technical.

A consistent theme across Security, Privacy, Data Protection, and Vendor risk is the **Snowflake boundary**: risks are materially lower for in-platform capabilities (which inherit existing governance) and concentrate at the points where data or actions cross into external systems via the Teams, Slack, and MCP integrations.

## 5. Conclusion and Next Steps

No identified risk is assessed as unmanageable; all are addressable through the proposed mitigations, consistent with the phased adoption recommended in the Platform Assessment. Recommended next steps:

1. Risk review board to validate ratings and confirm risk owners
2. Mitigations to be converted into formal controls in the Control Library (folder 03)
3. DPIA initiated before any personal data is processed by Cortex
4. Residual risk ratings to be assigned after Phase 1 controls are implemented and tested
5. This assessment to be reviewed quarterly, or upon material platform changes (new integrations, new model availability, features moving from preview to GA)

---

*Prepared as part of the AI Governance Portfolio. Methodology designed by the author in the absence of an organisational standard; proposed for reuse across future platform risk assessments.*
