---
description: Analyze AdaPos+ systems and generate estimation documents (SOW, BRD, SRS, PRD, WBS, etc.)
---

# /ada-estimate-md - AdaPos+ System Estimation

This workflow automates the analysis of AdaPos+ interface projects and generates a comprehensive set of requirement and estimation documents.

## Guardrails
- **Skill Priority:** Use `../skills/ada-estimate/skill.md` as the primary protocol.
- **Domain Focus:** Combine `adapos`, `adainterface`, and `ada-estimate` rules.
- **Naming Conventions:** Follow Section 11 of `../skills/adapos/SKILL.md`.
- **Estimation Rules:** Strictly follow `../std and condition/Standard Rate Configuration.md`.
- **No Hallucination:** If project specifics are missing, ask the user immediately.

## Step 1: Context Discovery (Question-Driven)
Follow the **Mandatory Question Gate** in `../skills/ada-estimate/skill.md`:
1. **Business Segment:** (Retail, Wholesale, Food Court, Fashion, etc.)
2. **ERP Type & Version:** (SAP B1, SAP HANA, D365 BC, TRCloud, etc.)
3. **Integration Method:** (File-based, REST API, Web Service, DB Staging)
4. **Integration List:** Number of APIs/Interfaces.
5. **Specific Requirements:** (e-Tax, VAT Refund, Duty Free, Multi-Currency)

## Step 2: System Mapping & Analysis
Orchestrate the following specialized agents:
- `mypm`: Project Manager - Logic control and final estimation summary.
- `myba`: Business Analyst - Requirement elicitation and segment mapping.
- `mysa`: System Analyst - Architecture design and technical mapping.

**References to Use:**
- `../skills/ada-estimate/skill.md` (Primary Protocol)
- `../skills/adapos/SKILL.md` (Domain Knowledge)
- `../skills/adainterface/SKILL.md` (Interface Expert)
- `../std and condition/` (Formula & Guidelines)

## Step 3: Document Generation
Generate 8 separate markdown files into the specified output folder (default: `docs/output/`):

1.  **01_System_Architecture_Diagram:** Mermaid/PlantUML diagram of the integration.
2.  **02_Statement_of_Work_SOW:** Scope, boundaries, and deliverables.
3.  **03_Business_Requirement_Document_BRD:** Business flows and process mapping.
4.  **04_Sequence_Diagrams:** Detailed data exchange sequence between POS and ERP.
5.  **05_Software_Requirement_Specification_SRS:** Technical requirements and API specs.
6.  **06_Product_Requirement_Document_PRD:** User stories and feature details.
7.  **07_Work_Breakdown_Structure_WBS:** Task list with phase breakdown.
8.  **08_Manday_Estimation:** Full calculation based on `Standard Rate Configuration.md`.
    -   **Audit Trail:** Include specific breakdowns for SM (Sales) and DI (Internal Cost).
    -   **Multipliers:** Apply Complexity, Project Size, and Dev Level adjustments.
    -   **Holidays:** Incorporate the Official Holiday List into the timeline projection.
    -   **Interactive Check:** Use the logic from `/ada-estimate-tool` to confirm parameters.

## Step 4: Verification
- Verify that **Senior Programmer** rates are used unless specified otherwise.
- Ensure all tables follow `TXXYName` prefix rules.
- Validate that the total sum matches the sub-phase percentages in the calculator.

## Principles
- **Accuracy First:** Data mapping must be precise to avoid financial discrepancy.
- **Traceability:** Interface logs and error handling must be documented.
- **Standardization:** Use the `Interface_Standard_Guideline.md` as the source of truth.

## Reference
- Workflow inspired by `/create` and `/orchestrate`.
- Documentation standards are located in `docs/` and `.agent/skills/`.
