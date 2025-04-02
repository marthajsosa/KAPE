# 🕵️‍♀️ KAPE + PowerShell Forensic Toolkit

This repository contains PowerShell scripts and automation examples for extracting, processing, and analyzing forensic artifacts using [Kroll Artifact Parser and Extractor (KAPE)](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape). It demonstrates how to perform targeted digital forensics and incident response tasks efficiently with custom scripting.

---

## 🔍 Purpose

To automate and scale digital forensic collection and analysis using KAPE and PowerShell. Use cases include:

- Collecting specific artifact types from live or offline systems
- Parsing output from KAPE modules
- Filtering forensic data for incident triage
- Automating timeline creation and evidence sorting

---

## 🧰 Toolkit Contents

| Script Name | Description |
|-------------|-------------|
| `Invoke-KapeRun.ps1` | Runs KAPE with JSON input, validates schema, and handles output organization |
| `Parse-SQLECmd.ps1` | Post-processing of SQLECmd results into timeline-friendly format |
| `Timeline-Builder.ps1` | Aggregates outputs from multiple KAPE modules into a unified timeline |
| `KapeSchema.json` | Defines required/optional fields for KAPE job configuration |
| `SampleInputs/` | Example JSON files for common DFIR scenarios |

---

## 🧪 Examples

### 🔹 Running a Targeted KAPE Collection

```powershell
.\Invoke-KapeRun.ps1 -InputConfig .\SampleInputs\BrowserArtifacts.json
# KAPE

## 1. Description

<details open>
<summary>Expand/Collapse</summary>

Descript KAPE and it's value to incident responder

### Tool Access and Provisioning

## Kape Tool 1
<blockquote>quick description</blockquote>

### PowerShell Script for Kape Tool 1

```Logscale
PowerShell Script for Kape Tool 1
```

Note:
- note
- note
- note
  
<br/>


## Kape Tool 2
<blockquote>quick description</blockquote>

### PowerShell Script for Kape Tool 1

```Logscale
PowerShell Script for Kape Tool 1
```

Note:
- note
- note
- note
