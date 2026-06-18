# AI Governance Model Card: Snowflake Cortex

| | |
|---|---|
| **Document Type** | AI Governance Model Card |
| **Related Documents** | [Platform Assessment](../01-Platform-Assessments/Snowflake-Cortex.md) · [Risk Assessment](../02-Risk-Assessments/Snowflake-Cortex.md) · [Control Library](../03-Control-Library/Snowflake-Cortex.md) |
| **Prepared by** | Michaella Njuakom, AI Governance Lead |
| **Date** | June 2026 |
| **Status** | Draft — for leadership approval decision |

This model card summarises how Snowflake Cortex will be governed, monitored, and used within the organisation. It consolidates the findings of the Platform Assessment, Risk Assessment, and Control Library into a single governance artifact for stakeholders, leadership, auditors, and future governance teams.

---

## 1. Platform Name

Snowflake Cortex — the native AI feature suite of the Snowflake AI Data Cloud.

## 2. Vendor

Snowflake Inc. Foundation models accessed through Cortex are additionally provided by third parties including Anthropic, OpenAI, Meta, Mistral, Google, and Reka, hosted within Snowflake's managed environment.

## 3. Business Owner

**Data Analytics** (Data & Analytics function). As the team that owns the organisation's Snowflake estate and the data within it, Data Analytics is the natural business owner, with individual use cases co-owned by the business units that propose them (e.g., Sales, Finance, Customer Service).

## 4. Technical Owner

**Head of Data Platform**, supported by an **AI Security Engineer** for security configuration and an **AI Governance Lead** for governance oversight. Day-to-day platform administration sits with the Data Platform team.

## 5. Platform Purpose

Snowflake Cortex brings AI capabilities directly into the Snowflake platform where the organisation's data already lives, rather than requiring data to be moved to external AI services.

**Problem it solves:** It removes the "integration tax" of enterprise AI — the need to export data to third-party AI tools and build security and compliance controls around each connection — by running AI inside the existing, governed data environment. It also reduces the analytics backlog by letting business users query data in natural language instead of waiting on analyst teams.

**Capabilities it provides:** SQL-callable LLM functions (summarisation, classification, translation, sentiment, redaction); Cortex Analyst for natural-language querying of structured data; Cortex Search for semantic search over documents; Document AI for extracting data from unstructured files; Cortex Agents for multi-step tasks across systems; and access to multiple foundation models without separate vendor setup.

**Who will use it:** Data engineers and analysts, data scientists, application developers, and — significantly — non-technical business users accessing data through natural-language interfaces.

## 6. Intended Use

Cortex is approved (subject to the conditions in this card) for the following uses on appropriately classified data:

- **Data analytics** — natural-language querying and analysis of governed structured data via Cortex Analyst.
- **Knowledge search** — semantic search and retrieval over internal documents and knowledge bases via Cortex Search.
- **Content processing** — summarisation, classification, translation, and sentiment analysis of internal text via LLM functions.
- **Document intelligence** — structured data extraction from internal documents via Document AI.
- **Software/data-engineering assistance** — AI-assisted SQL and pipeline development within existing access controls.
- **Workflow automation** — agent-assisted internal workflows, subject to human-in-the-loop controls for consequential actions.

## 7. Prohibited Use

Cortex must **not** be used for:

- Processing personal or regulated data without a completed DPIA and the data-protection controls in place (SNOWC-03, SNOWC-04).
- Enabling AI functions on unclassified datasets, or on Confidential-or-above data without DPO sign-off.
- **Fully autonomous decision-making** on consequential matters — agents must not take write actions in external systems, make customer-facing decisions, or act on financial or personal data without human approval (SNOWC-14).
- Enabling external integrations (Teams, Slack, MCP connectors) before each has passed individual connector approval (SNOWC-13).
- Using AI outputs as the sole basis for decisions affecting individuals' legal or material interests without human review.
- Deploying any use case into production before it has passed use-case intake and risk classification (SNOWC-18).
- Use of preview-stage features in production processes.

## 8. Data Considerations

**Permitted data:** Internal, non-sensitive data classified as Public or Internal, where the dataset has completed sensitivity classification and been approved for AI enablement.

**Restricted data (requires approval):** Data classified Confidential or above, and any personal data, may only be processed with DPO sign-off, a completed DPIA, and masking/redaction applied (SNOWC-03). Regulated data (e.g., special-category personal data, financial records) is restricted pending explicit approval and may be prohibited for specific use cases.

**Governance requirements:**
- **Data classification is mandatory** before any dataset is enabled for AI processing (SNOWC-04).
- **Privacy:** personal data must be masked or redacted upstream of AI processing; a DPIA is required before any personal data is processed; records of processing must be updated.
- **Data residency:** the processing region of each model must be verified against UK/EU GDPR and contractual residency obligations before enablement (SNOWC-11).
- **Retention:** AI-derived artifacts (search indexes, fine-tuned models, logs) fall under the organisation's retention schedule; interaction logs are retained for a minimum of 12 months (SNOWC-07).
- **Boundary awareness:** data sent through external integrations leaves Snowflake's service boundary and becomes governed by third-party terms — a key consideration in connector approval.

## 9. Key Risks

Summarised from the [Risk Assessment](../02-Risk-Assessments/Snowflake-Cortex.md):

- **Inadequate human oversight (highest-rated risk).** Cortex puts AI in non-technical hands and enables autonomous agent actions; users may over-trust outputs and agents may act without review.
- **Unauthorised access / data leakage.** Cortex is enabled for all users by default (granted to PUBLIC) until restricted; AI functions and integrations expand the paths by which data could be accessed or exfiltrated.
- **Privacy and regulatory non-compliance.** Personal data may be processed without lawful basis; use cases may fall under the EU AI Act without classification.
- **Boundary erosion via integrations.** Teams, Slack, and MCP connectors move data and actions beyond Snowflake's governed perimeter, under third-party terms.
- **Hallucinated / unreliable outputs.** Generated SQL, summaries, and answers may be inaccurate, risking poor decisions.
- **Third-party / model supply-chain risk.** Dependence on Snowflake and multiple foundation-model providers whose terms and availability can change.
- **Uncontrolled cost and shadow usage.** Consumption-based billing and self-service access can lead to unbudgeted spend and ungoverned use.

## 10. Required Controls

The most important controls required before approval, drawn from the [Control Library](../03-Control-Library/Snowflake-Cortex.md):

| Control | Name | Why it matters |
|---|---|---|
| SNOWC-01 | Role-Based Entitlement to AI Functions | Reverses Snowflake's allow-by-default access; enforces least privilege |
| SNOWC-03 | PII Masking Before AI Processing | Protects personal data before it reaches the model |
| SNOWC-04 | Data Classification Gate for AI Enablement | Prevents AI access to unclassified/sensitive data |
| SNOWC-07 | AI Interaction Audit Logging | Provides the audit trail for accountability and compliance |
| SNOWC-13 | External Connector Approval Process | Governs every data flow that crosses the Snowflake boundary |
| SNOWC-14 | Human-in-the-Loop Gate for Consequential Agent Actions | Prevents unreviewed autonomous actions |
| SNOWC-18 | AI Use-Case Intake and Risk Classification | Ensures no use case reaches production unclassified |
| SNOWC-08 | Vendor and Model Terms Review | Confirms data is not used for training and terms are acceptable |
| SNOWC-19 | AI Incident Response Plan | Provides corrective response when other controls miss |

The full set of 19 controls (SNOWC-01 to SNOWC-19) applies; the above are the gating controls for initial approval.

## 11. Human Oversight Requirements

- **Consequential agent actions require human approval.** Any write action to an external system, any customer-facing communication, and any action affecting financial or personal data must be routed to a named approver; agents operate in draft-and-propose mode for these actions (SNOWC-14).
- **AI-assisted decisions remain human-owned.** The human user is accountable for decisions made with AI assistance; AI outputs are decision support, not decision authority.
- **Outputs requiring review:** generated SQL and analytical answers used for business decisions; agent-generated responses with external citations; any output informing decisions about individuals.
- **Decisions that must never be fully automated:** customer-facing determinations, actions on personal or financial data, and any decision with legal or material effect on an individual.
- **Semantic models** must be validated for accuracy before release to business users (SNOWC-15).

## 12. Monitoring Requirements

Post-deployment monitoring includes:

- **Quarterly access reviews** of privileged AI roles (SNOWC-02).
- **Continuous usage monitoring** for anomalies and shadow adoption, triaged weekly (SNOWC-06).
- **Audit log reviews** of AI interactions, by Internal Audit periodically and Security Operations on demand (SNOWC-07).
- **Agent action reviews** — weekly during pilot, monthly sampling thereafter (SNOWC-12).
- **Consumption monitoring** against budgets, reviewed monthly (SNOWC-05).
- **Vendor change monitoring** for model deprecations and terms changes (SNOWC-09).
- **Control effectiveness reviews** — the control library is reviewed quarterly and on material platform changes.
- **Incident tracking** via the AI incident response process (SNOWC-19).

## 13. Approval Recommendation

**Approve with Conditions.**

## 14. Justification

Snowflake Cortex is recommended for **approval with conditions**, implemented in phases.

**Platform Assessment findings** support adoption: Cortex's defining characteristic is that AI runs inside the existing, governed Snowflake environment, so in-platform capabilities inherit established RBAC, masking, and audit controls rather than introducing a parallel, ungoverned AI stack. This materially lowers risk for the core use cases.

**Risk Assessment findings** require conditions rather than unconditional approval. The highest-rated risk is inadequate human oversight — a direct consequence of the platform's core value proposition (AI for non-technical users, autonomous agents) — and several high risks (privacy, data protection, boundary erosion) concentrate at specific, controllable points: sensitive data and external integrations. Notably, Cortex is enabled for all users by default until access is explicitly restricted, so approval cannot be unconditional.

**Control requirements** are achievable and address each significant risk. The gating controls — deny-by-default access (SNOWC-01), data classification and masking (SNOWC-03, SNOWC-04), human-in-the-loop gates (SNOWC-14), connector approval (SNOWC-13), and use-case intake (SNOWC-18) — are implementable with platform-native capabilities and produce auditable evidence.

**Governance considerations** favour a phased rollout:

1. **Phase 1 — Approve:** in-platform capabilities (LLM functions, Cortex Analyst, Cortex Search, Document AI) on Public/Internal data, with gating controls in place and guardrails enabled.
2. **Phase 2 — Conditional:** rollout to business users and processing of Confidential/personal data, contingent on DPIA, training (SNOWC-16), and semantic model validation (SNOWC-15).
3. **Phase 3 — Assess separately:** external integrations (Teams, Slack, MCP connectors) and autonomous agent actions, each subject to individual connector approval and human-in-the-loop controls before enablement.

**Conditions of approval:** the gating controls in Section 10 must be implemented and evidenced before Phase 1 use; no personal or regulated data may be processed until Phase 2 conditions are met; and no external integration may be enabled until it has passed individual assessment under SNOWC-13.

On this basis, the platform can be adopted responsibly while keeping its principal risk — autonomous, unsupervised AI action on sensitive data or in external systems — within organisational tolerance.

---

*Prepared as part of the AI Governance Portfolio. This model card consolidates the Platform Assessment, Risk Assessment, and Control Library for Snowflake Cortex into a single governance summary supporting the approval decision.*
