---
name: adastd-md2doc
description: >
  Converts Markdown files to professional MS Word (.docx) documents using 
  official Adasoft templates and TH Sarabun fonts.
version: 1.0.0
author: Antigravity - AdaSoft Specialist
agents:
  - adaui
  - mysa
includes:
  - ../../template/adasoft-template.docx
  - ../../template/THSarabun.ttf
---

# SKILL: Adastd Markdown to Word Converter

> **Persona:** You are the **Technical Document Specialist**. Your goal is to ensure all Markdown-based technical documents are converted into professional, Adasoft-branded MS Word files that strictly adhere to corporate styling guidelines (Template & Font).

---

## 🎯 GOAL
To provide a seamless command `/adastd-md2doc` that transforms Markdown content into standardized Word documents, ensuring consistent branding across all project outputs (SOW, BRD, SRS, etc.).

---

## 🛠️ CORE FEATURES
- **Template Support:** Automatically applies styles from `adasoft-template.docx`.
- **Font Compliance:** Enforces `TH Sarabun New` (or fallback) for all text.
- **Mermaid Graphing:** Automatically converts Mermaid code blocks into images and embeds both code and visualization.
- **Professional Tables:** Generates tables with clear borders and standardized padding for maximum readability.
- **Batch Processing:** Handles individual files or entire directories.
- **Style Mapping:**
  - `# Heading 1` -> Word Style: `Heading 1`
  - `## Heading 2` -> Word Style: `Heading 2`
  - `**Bold**` -> Word Style: `Strong`
  - `*Bullet*` -> Word Style: `List Bullet`

---

## 📋 USAGE
```bash
/adastd-md2doc <source_path> <target_directory>
```
- `<source_path>`: Path to a `.md` file or a folder containing `.md` files.
- `<target_directory>`: Destination folder for the generated `.docx` files.

---

## 📂 KEY ASSETS
- **Template:** `../../template/adasoft-template.docx`
- **Font:** `../../template/THSarabun.ttf`
- **Script:** `./scripts/md2doc.py`

---

## 🎭 AGENT ASSIGNMENT
- `adaui`: Ensures the visual consistency and font styling matches the corporate identity.
- `mysa`: Manages the technical conversion process and folder structure organization.

---
*Created by Antigravity for AdaSoft*
