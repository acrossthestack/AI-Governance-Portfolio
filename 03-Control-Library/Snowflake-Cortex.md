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
