## Platform Overview: Snowflake Cortex

| | |
|---|---|
| **Platform** | Snowflake Cortex (Snowflake AI Data Cloud) |
| **Vendor** | Snowflake Inc. |
| **Assessment Type** | Initial Platform Assessment (pre-approval) |
| **Prepared by** | Michaella Njuakom|
| **Date** | June 2026 |
| **Status** | Draft — for leadership review |

---

## 1. Executive Summary

Snowflake Cortex is a fully managed suite of AI capabilities embedded directly inside the Snowflake AI Data Cloud. Rather than sending enterprise data out to external AI tools, Cortex brings large language models (LLMs), semantic search, natural-language analytics, document intelligence, and AI agents to where the data already lives.

From a governance perspective, this "AI inside the data platform" architecture is Cortex's defining characteristic. It means AI workloads inherit Snowflake's existing security perimeter, role-based access control (RBAC), and governance policies by default — a meaningful risk-reduction compared to standalone AI tools. However, newer capabilities (agentic workflows, external integrations into Microsoft Teams, Slack, Salesforce, and Jira) extend Cortex beyond that perimeter, and these data flows require explicit governance attention before approval.

**Recommendation:** Suitable for a governed pilot, with conditions outlined in Section 8.

---

## 2. What Is It?

Snowflake Cortex is Snowflake's native AI service layer — a suite of features that use large language models to understand unstructured data, answer freeform questions, and provide intelligent assistance, all within the Snowflake platform.

Key characteristics:

- **Fully managed / serverless.** The LLMs are hosted and managed by Snowflake. There is no infrastructure to provision, no model deployment, and no GPU management required by the customer.
- **Multi-model access.** Cortex provides curated access to industry-leading foundation models — including models from Anthropic (Claude), OpenAI, Meta (Llama), Mistral, Google, and Reka — alongside Snowflake's own Arctic model. Model choice is made per-call or per-workload.
- **Data stays in place.** AI functions execute against data already stored and governed in Snowflake (including Iceberg tables and external object storage such as Amazon S3), avoiding data movement to third-party AI services.
- **Accessed through familiar interfaces.** Capabilities are exposed through SQL functions, Python, REST APIs, and no-code interfaces (Snowflake Intelligence, Cortex Code), making AI accessible to both technical and business users.

In short: Cortex converts an organisation's existing Snowflake estate from a data warehouse into an AI execution environment.

---

## 3. What Does It Do? (Core Capabilities)

| Capability | What it does | Typical interface |
|---|---|---|
| **Cortex AISQL / LLM Functions** | SQL-callable AI functions (e.g., `AI_COMPLETE`, summarise, translate, classify, extract sentiment, redact PII) that run LLMs directly against table data, including multimodal data (text, images, audio) | SQL |
| **Cortex Analyst** | Converts natural-language business questions into SQL against governed, structured data using semantic models — "talk to your data" for non-technical users | REST API, chat apps |
| **Cortex Search** | Hybrid semantic + keyword search over unstructured content (documents, transcripts, knowledge bases), commonly used as the retrieval layer for RAG applications | API / SQL |
| **Document AI** | Extracts structured data from PDFs and other unstructured documents (invoices, contracts, forms) | UI / SQL |
| **Cortex Agents** | Orchestrates multi-step tasks across structured and unstructured data — planning, choosing tools (Analyst, Search), executing actions, and generating cited responses | REST API, Teams, Slack |
| **Snowflake Intelligence** | A conversational interface for business users to analyse and act on enterprise data in natural language, without writing SQL | Web / mobile app |
| **Cortex Code** | AI coding agent that generates SQL, Python, dbt models, and pipelines for data engineering work within Snowflake's RBAC | CLI / IDE |
| **Fine-tuning & Cortex Training** | Customisation of smaller models on organisation-specific data for cost-efficient, specialised use cases | SQL / API |
| **Guardrails** | Built-in content filtering to reduce harmful outputs from LLM functions | Configuration |

---

## 4. Who Would Use It? (Intended Users)

| User group | How they use Cortex |
|---|---|
| **Data engineers** | AISQL functions in pipelines; Cortex Code for AI-assisted development; Document AI for ingestion of unstructured sources |
| **Data analysts / BI teams** | Cortex Analyst and Copilot to accelerate query writing and self-service analytics |
| **Data scientists / ML engineers** | LLM functions, embeddings, fine-tuning, and RAG patterns without managing model infrastructure |
| **Application developers** | Cortex Agents and Search APIs to build AI-powered internal and customer-facing applications |
| **Business users (non-technical)** | Snowflake Intelligence and the Teams/Slack integrations to ask questions of enterprise data in natural language |
| **Knowledge workers** | Document summarisation, search across internal content, and agent-assisted workflows |

A governance-relevant observation: Cortex deliberately **broadens the population of AI users** beyond technical teams. Natural-language access for business users is a core value proposition — and it means access control, output reliability, and acceptable-use policies must account for users with no data or AI expertise.

---

## 5. What Business Problem Does It Solve?

1. **AI without the integration tax.** Traditionally, enterprise AI requires moving data to external services, managing API keys and vendors, and building security and compliance controls around each integration. Cortex eliminates much of this by running AI where the data already lives, under existing governance.

2. **The "last mile" of analytics.** Business users typically wait on analysts and dashboards for insights. Cortex Analyst and Snowflake Intelligence let them ask questions directly in natural language, reducing the analytics backlog and time-to-insight.

3. **Unlocking unstructured data.** Most organisations have large volumes of documents, transcripts, tickets, and images that conventional analytics cannot touch. Document AI, Cortex Search, and multimodal AISQL bring this data into governed analysis alongside structured data.

4. **Reducing AI pilot failure.** Industry research (cited in vendor and analyst commentary) suggests the large majority of AI initiatives stall before production, often due to weak data and governance foundations. Cortex's pitch is that building on already-governed data improves the odds of reaching production.

5. **Developer productivity.** Cortex Code and Copilot accelerate data engineering and SQL development within the platform's existing security model.

---

## 6. What Integrations Exist?

### Within the Snowflake ecosystem
- Native operation across Snowflake tables, **Apache Iceberg** tables, and external object storage (e.g., **Amazon S3**) without data movement
- **Streamlit in Snowflake** for building data/AI applications
- Snowflake Marketplace data and the broader connector ecosystem (ETL/ELT tools such as Fivetran and dbt feed the data Cortex operates on)

### Foundation model providers (consumed *through* Cortex)
- Anthropic, OpenAI, Meta, Mistral, Google, Reka, DeepSeek, and Snowflake Arctic — hosted within Snowflake's managed service

### Enterprise application integrations (data leaves or enters the Snowflake boundary)
- **Microsoft Teams and Microsoft 365 Copilot** — Cortex Agents embedded in Teams chat and Copilot, letting users query Snowflake data in natural language from Microsoft apps
- **Slack** — Cortex Agents accessible via Slack apps
- **MCP (Model Context Protocol) connectors** — a native integration layer connecting Cortex Agents and Snowflake Intelligence to external systems including **Salesforce, Jira, Confluence, GitHub, Gmail, Google Calendar, Google Docs/Drive, and Slack**, enabling agents to both retrieve information and take actions (e.g., create a Jira ticket, update a Salesforce record)
- **REST APIs** — Cortex Agents, Analyst, and Search are all exposed via REST for custom application integration

### Governance note on integrations
The Microsoft/Slack/MCP integrations are where Cortex's "data stays inside Snowflake" assurance has limits. Snowflake's own documentation states that once data is sent to Microsoft services through the Teams integration, it leaves the Snowflake service boundary and is governed by the customer's terms with Microsoft — not by Snowflake. Each external connector therefore represents a distinct data flow requiring its own assessment.

---

## 7. Data Flows (Summary)

| Flow | Direction | Governance implication |
|---|---|---|
| SQL/AISQL functions over Snowflake data | Internal | Inherits Snowflake RBAC, masking, row-level security; lowest risk |
| LLM inference (Snowflake-hosted models) | Internal (managed) | Prompts/data processed within Snowflake's perimeter; confirm region/data-residency handling per model |
| Cortex Agents → Teams / Slack | Outbound | Responses (which may contain enterprise data) transit to Microsoft/Slack; outside Snowflake's responsibility boundary |
| MCP connectors (Salesforce, Jira, Gmail, etc.) | Bidirectional | Agents can read from and **write to** external systems; action authorisation, audit, and least-privilege scoping required |
| Document/image ingestion from S3 | Inbound | Source-system access controls and content sensitivity must be assessed pre-ingestion |

---

## 8. Benefits, Risks, and Governance Considerations

### Key benefits
- AI inherits existing Snowflake governance: RBAC, data masking, audit logging, and access policies apply to AI workloads by default
- No data movement to third-party AI vendors for core capabilities
- Explainability support: agent responses include citations; interactions are processed within an auditable environment
- Built-in guardrails for harmful content; PII redaction functions (e.g., AI_REDACT) available
- Single vendor relationship and unified billing/consumption model simplify oversight

### Key risks and considerations

| # | Risk | Governance consideration |
|---|---|---|
| 1 | **Boundary erosion via integrations** — Teams, Slack, and MCP connectors move data and actions beyond Snowflake's perimeter | Assess and approve each connector individually; data handling governed by third-party terms |
| 2 | **Agentic actions** — agents can execute write actions in external systems (tickets, records, emails) | Require least-privilege scoping, human-in-the-loop for consequential actions, action audit logging |
| 3 | **Expanded user base** — natural-language access for non-technical users | Acceptable-use policy, training, and monitoring; users may over-trust AI outputs |
| 4 | **LLM output reliability** — hallucination risk in generated SQL, summaries, and answers | Validate semantic models; require citation review for decisions; define accuracy thresholds for use cases |
| 5 | **Third-party model supply chain** — multiple external foundation models accessed through Cortex | Maintain a model inventory; review Snowflake's terms per model; document in Model Cards (folder 05) |
| 6 | **Cost and shadow usage** — consumption-based AI billing can scale unexpectedly | Budgets, resource monitors, and role-based entitlement to AI functions |
| 7 | **Data residency / regional availability** — model availability and processing location vary by region | Confirm processing regions against data-residency obligations (e.g., UK/EU GDPR) before enabling |
| 8 | **PII exposure in prompts** — sensitive data may be passed to LLM functions | Mandate masking/redaction policies upstream; restrict AI functions on sensitive schemas |

### Framework alignment (initial mapping)
- **NIST AI RMF:** Map (use-case inventory per Cortex capability) → Measure (output accuracy, bias testing for Analyst semantic models) → Manage (connector approvals, agent action controls)
- **NIST SP 800-53 Rev. 5:** AC-6 (least privilege for agent actions), AU-2/AU-12 (audit of AI interactions), SA-9 (external system services — Microsoft/Slack integrations), RA-3 (risk assessment per connector)
- **EU AI Act / ISO 42001:** Classify each use case by risk tier; establish AI management system documentation before production use

---

## 9. Recommendation

Snowflake Cortex is **architecturally well-suited to governed enterprise AI adoption**, because it builds on an existing, governed data foundation rather than introducing a parallel AI stack. The core SQL-based and in-platform capabilities present comparatively low incremental risk where Snowflake governance is already mature.

Approval is recommended **in phases**:

1. **Phase 1 (approve):** In-platform capabilities (AISQL, Cortex Analyst, Cortex Search, Document AI) on non-sensitive, internal datasets, with guardrails enabled and usage monitoring in place.
2. **Phase 2 (conditional):** Snowflake Intelligence rollout to business users, contingent on acceptable-use policy, training, and semantic model validation.
3. **Phase 3 (assess separately):** External integrations (Teams, Slack, MCP connectors) and agentic write-actions — each connector to receive its own risk assessment (folder 02) and entry in the control library (folder 03) before enablement.

---

*Sources: Snowflake product documentation and announcements (docs.snowflake.com, snowflake.com), Snowflake engineering blog, and independent platform analyses, reviewed June 2026. Capabilities marked as preview/GA-soon by Snowflake are evolving and should be re-verified at time of approval.*
