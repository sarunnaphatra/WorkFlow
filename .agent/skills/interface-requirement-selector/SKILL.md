---
name: interface-requirement-selector
description: Master skill to identify the ERP type of an AdaPos+ project and route to the specific requirement skill (SAP, D365, TRCloud, or General).
---

# Interface Requirement Selector

This skill analyzes the project context (files, names) to suggest the best prompting skill.

## 1. Project Analysis
Scan the project folder (e.g., `adaproject/Project Name`) for keywords:
- **SAP Keys:** `SAP`, `HANA`, `B1`, `IDoc`, `BAPI`, `RFC`
- **D365 Keys:** `D365`, `Dynamics`, `AX`, `Entity`, `OData`
- **TRCloud Keys:** `TRCloud`, `Cloud ERP`, `REST`, `JSON`
- **General Keys:** `API`, `Web Service`, `CRM`, `Custom`

## 2. Skill Selection
- If **SAP HANA** or **ECC** detected -> Suggest/Use `interface-requirement-sap-hana`
- If **SAP Business One** or **B1** detected -> Suggest/Use `interface-requirement-sap-b1`
- If **D365** detected -> Suggest/Use `interface-requirement-d365`
- If **TRCloud** detected -> Suggest/Use `interface-requirement-trcloud`
- If **Buzzebees** detected -> Suggest/Use `interface-requirement-buzzebees`
- If **e-Tax** or **RD** detected -> Suggest/Use `interface-requirement-etax`
- If **Primo** detected -> Suggest/Use `interface-requirement-primo`
- Else -> Suggest/Use `interface-requirement-general`

## 3. Usage Example
"Analyze `Project Leonian`" -> Detected `D365` -> "Please use `interface-requirement-d365` to gather requirements."

## 4. Batch Processing
If "Gather all" is requested:
1. Iterate through directories in `adaproject/`.
2. For each, determine the type.
3. List the recommended prompt command for each.
   Example:
   - Project Baimiang: `SAP`
   - Project Leonian: `D365`
   - Project Journal: `General`
