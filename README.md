# 🕵️‍♀️ KAPE + PowerShell Forensic Toolkit

This repository contains PowerShell scripts and automation examples for extracting, processing, and analyzing forensic artifacts using [Kroll Artifact Parser and Extractor (KAPE)](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape). It demonstrates how to perform targeted digital forensics and incident response tasks efficiently with custom scripting. All information is from https://ericzimmerman.github.io/KapeDocs/#!index.md

---

## Purpose

These simple scripts can be modified to fit your needs and used to automate forensic collection and analysis using KAPE and PowerShell. Use cases include:

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
|DNS Cache  | Temp database that has recent DNS lookup records. |
|Shadow Copies  | Point-in-time snapshot of files or entire volumes created by VSS in Windows |
|Running Services  | Can identify operations that are happening at the time of an incident like network connections, file access, track file execution, and user behavior. |
|Browser History  | Gives insight into user's online activities, downloads, and search keywords. |
|Windows Event Logs  | Provides detailed record of system and user activities including authentication attempts, process executions, and file access. |

---

## Scripts

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

---

## DNS Cache

```cmd
ipconfig /displaydns
```

```powershell
Get-DnsClientCache
```

```powershell
Get-DnsClientCache | Selection-Object Entry,Name,Status,Type,TimeToLive,Data | Export-Csv -NoTypeInformation -Path C:\Temp\DnsCache.csv
```

Note:
- Note
- Note
- Note

---


## Shadow Copies

```powershell
.\VSCMount.exe --dl C --mp \demo
```

Note:
- VSCMount.exe
- Note
- Note

---

## Running Services

```powershell
Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object -Property Name, Display, Status | Export-Csv -Path C:\Temp\Services.csv -NoTypeInformation
```

```powershell
Get-CimInstance Win32_Process | Select ProcessID, ProcessName, Path, CommandLine, Description, ParentProcessID, CreationDate, Handle, HandleCount, @{Label='MD5'; Expression={(Get-FileHash -Algorithm MD5 -Literal Path $_.Path).Hash}} | Export-Csv -NoTypeInformation -Path C:\PWSH-Get-CIM_ProcessList.csv
```

Note:
- Most are stored in C:\Windows\System32 or C:\Program Files
- Note
- Note

---

## Browser History

- Retrieve the db browser files and upload to https://extendsclass.com/sqlite-browser.html#
- Query: select * from urls;
- Run
- Export results to csv
- Save as xlsx
- Change webkit time to human readable time, enter forumula: =(F2/1000000 - 11644473600) / 86400 + DATE(1970,1,1), Right click, format cells, Date, choose best date format.


Note:
- Chrome History file path C:\Users\<user>\AppData\Local\Google\Chrome\User Data\Default\History
- Edge History file path C:\Users\<user>\AppData\Local\Microsoft\Edge\User Data\Default\History
- Firefox History C:\Users\<user>\AppData\Roaming\Mozilla\Firefox\Profiles\<ProfileName>\places.sqlite
- Safari History :/Users/<username>/Library/Safari/History.db

---

## Windows Event Logs

Security Logs: 

```powershell
.\EvtxECmd.exe -f C:\Windows\System32\winevt\Logs\Security.evtx --csv .
```

Task Scheduler Events: 
```powershell
.\EvtxECmd.exe -f "C:\Windows\System32\winevt\Logs\Microsoft-Windows-TaskScheduler%4Operational.evtx" --csv .
```

PowerShell: 
```powershell
 .\EvtxECmd.exe -f C:\Windows\System32\winevt\Logs\Windows PowerShell.evtx" --csv .
```

Applications: 
```powershell
 .\EvtxECmd.exe -f C:\Windows\System32\winevt\Logs\Application.evtx --csv .
```

System: 
```powershell  
 .\EvtxECmd.exe -f C:\Windows\System32\winevt\Logs\System.evtx --csv .
```

Note:
- Note
- Note
