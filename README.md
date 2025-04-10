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
|File Deletion | Deleting files create an index file, or $I file, that contains metadata about the deletion. |
|Registry Hive  | Provides detailed insights into system and user activity, malware persistence, and program execution. |
|RDPcache  | RDP bitmap cache files can recover visual data from remote sessions. |
|Jump List | Jump lists is a dynamic menu that shows recently access files, tasks, and applications for a user. |
|Lnk Files  | Windows shortcut that points to an executable file, document, or another resource on the system usually with .lnk extension. |
|Windows Event Timeline  | Captures detailed records of what files the user opened, apps used, and websites visited. |
|WER | .dmp files are WER files created when a program crashes or encounters an error and contains the time of error, memory contents, and code that caused the crash. |
|Scheduled Tasks  | Automated processes set to run at specific times or under certain conditions. |
|Active SMB and Network Shares  | Provides real-time user activity, unauthorized access, and data exfil. |

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
.\RECmd.exe -f C:\path\to\Registry\hive --kn ROOT --nl false --csv C:\output\path\here --csvf "outputname.csv" -q
```

Note:
- RECmd.exe has several pre-built batch files
- -f or -d is required.
- For csv output use batch mode - https://github.com/EricZimmerman/RECmd/tree/master/BatchExamples

---

## RDP Bitmap Cache

```powershell
.\bmc-tools.py -s C:\Users\<User>\path\to\file.bin -d C:\temp\RDPBMpCache -b
```

Note:
- Browse and stitch files with RdpCacheStitcher
- Note
- Note

---

## Jump List

```powershell
.\JLECmd.exe -d C:\Users\<user>\AppData\Roaming\Microsoft\Recent\AutomaticDestinations --ld --mp --dt "ddd MMM DD YYY HH:mm:ss:fffff K" --csv C:Temp\ -q
```

Note:
- Note
- Note
- Note

---

## Lnk Files

```powershell
.\LECmd.exe -d C:\Users\<user>\AppData\Roaming\Microsoft\Windows\Recent --mp --csv C:\Temp -q
```

Note:
- Note
- Note
- Note

---

## Windows Event Timeline

```powershell
.\WxTCmd.exe -f C:\Users\<user>\AppData\Local\ConnectedDevicesPlatform\AAD.c...\ActivitiesCache.db --csv C:\Temp
```

Note:
- File path C:\Users\<user>\AppData\Local\ConnectedDevicesPlatform\L.<SID>\ActivitiesCache.db
- Note
- Note

---

## WER

```powershell
winget install Microsoft.WinDbg
```

```powershell
Start-Process -FilePath ".\kape.exe" -ArgumentList @("--tsource","C:","--target","WER","--tdest","C:\temp\WER","--trace","--debut") -NoNewWindow
```

Note:
- Temporary Directory (App Crash Reports): Path C:\Users\<user>\AppData\Local\CrashDumps
- WER Report Queue Directory path C:\ProgramData\Microsoft\Windows\WER\ReportQueue
- WER Report Archive Directory path C:\ProgramData\Microsoft\Windows\WER\ReportArchive
- System-Level Crash Dumps (BSOD/Kernel Crashes) path C:\Windows\Minidump

---

## Scheduled Tasks

```powershell
winget install Microsoft.WinDbg
```

```powershell
Start-Process -FilePath ".\kape.exe" -ArgumentList @("--tsource","C:","--target","ScheduledTasks","--tdest","C:\temp\toutput\targetoutput","--msource","C:\temp\toutput\targetoutput", "--module","PowerShell_ParseScheduledTasks", "--mdest", "C:\temp\moutput\moduleoutput","--trace","--debut") -NoNewWindow
```

Note:
- Task definitions are stored at C:\Windows\System32\Tasks\
- Task logs are stored at C:\Windows\System32\Winevt\Logs\Microsoft-Windows-TaskScheduler%4Operational.evtx
- Note
- Note

---

## Active SMB and Network Shares

### Network Shares

```cmd
net share
```

```powershell
Get-SmbShare
```

```powershell
Get-WmiObject -Class Win32_Share | Select Name,Path,Description | Export-Csv -Encoding UTF8 -NoTypeInformation -Path 'C:\Temp\NetworkShares.csv'
```

### SMB

```cmd
net session
```

```cmd
net file
```

```powershell
Get-SMBSession | Select SessionId, ClientComputerName, ClientUserName, NumOpens | Export-Csv -NoTypeInformation -Encoding UTF8 -Path 'C:\Temp\SMBSessions.csv'
```

Note:
- Task definitions are stored at C:\Windows\System32\Tasks\
- Task logs are stored at C:\Windows\System32\Winevt\Logs\Microsoft-Windows-TaskScheduler%4Operational.evtx
- Note
- Note
