# 🕵️‍♀️ KAPE + PowerShell Forensic Toolkit

This repository contains PowerShell scripts and automation examples for extracting, processing, and analyzing forensic artifacts using [Kroll Artifact Parser and Extractor (KAPE)](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape). It demonstrates how to perform targeted digital forensics and incident response tasks efficiently with custom scripting. All information is from https://ericzimmerman.github.io/KapeDocs/#!index.md

---

## 🔍 Purpose

To automate and scale digital forensic collection and analysis using KAPE and PowerShell. Use cases include:

- Collecting specific artifact types from live or offline systems
- Parsing output from KAPE modules
- Filtering forensic data for incident triage
---

## Contents

| Artifact Name | Description |
|-------------|-------------|
|Shimcache  | Evidence an executable file had presence in a system within a GUI application, like Windows File Explorer. |
|Amcache | Records the processes recently run on the system and lists the full filepaths of the files executed, timestamp, and SHA1 hash for the binary. |
|Prefetch  | The presence of the prefetch file means the program was executed. |
| File Deletion | Deleting files create an index file, or $I file, that contains metadata about the deletion. |
|  |  |

---

## 🧪 Examples

### Shimcache

```powershell
.\AppCompatCacheParser.exe -f "C:\Temp\SYSTEM" --csv "C:\Temp" --csvf "shimcache.csv"
```

Note:
- note
- note
- note
  
---

## Amcache

```powershell
.\AmcacheParser.exe -f C:\Windows\appcompat\Programs\Amcache.hve --csv "C:\Temp" --csvf "amcache.csv"
```

Note:
- File path: C:\Windows\appcompat\Programs\Amcache.hve
- note
- note

---

## Prefetch

```powershell
.\PECmd.exe -f "C:\Users\<user>\Downloads\POWERPNT.EXE-######.pf" --csv "C:\Temp" --csvf "prefetch.csv"
```

Note:
- SourceCreated column may need to be formatted to show human readable datetime.
- Go to Format Cells > Category: Custom > Type: m/d/yyyy h:mm 
- note
  
---

## File Deletion

```powershell
.\RBCmd.exe -f "C:\`$Recycle.Bin\S-1-5-21-##########-##########-##########-######\`$Idirectoryname.csv"
```

Note:
- Location: $Recycle.Bin
- Edit hte command for the $Recycle.Bin, SID, and/or $I file you're targeting
- Use the backtick escape character ` before $ symbol

---

## Registry Hives

```powershell
.\RBCmd.exe -f "C:\`$Recycle.Bin\S-1-5-21-##########-##########-##########-######\`$Idirectoryname.csv"
```

Note:
- Locations: C:\Windows\System32\Config\SAM, C:\Windows\System32\Config\SYSTEM, C:\Windows\System32\Config\SECURITY, C:\Windows\System32\Config\SOFTWARE, C:\Users\<username>\NTUSER.DAT, C:\Users\<username>\AppData\Local\Microsoft\Windows\UsrClass.dat
- Note
- Note
