# windows-automation-lab
# Windows Automation Lab: Desktop Cleanup & Task Scheduling

## 📌 Project Overview
This repository serves as a practical learning lab focused on Windows system administration, process management, and native OS automation using PowerShell. 

Instead of writing a script from scratch, I partnered with an AI assistant to generate a foundational cleanup script. My primary objective was to deploy, configure, and troubleshoot this automation within the Windows ecosystem using **Task Scheduler**.

---

## 🛠️ The Script (`AutoCleanup.ps1`)
The script automates system maintenance by checking for a specific target directory, generating a timestamped log, creating a simulated temporary file, and purging `.tmp` clutter.

```powershell
# 1. Define the targets
$TargetFolder = "$Home\Desktop\PowerShell_Hacker"
$LogFile      = "$Home\Desktop\cleanup_log.txt"

# 2. Smart Check: If the folder doesn't exist, create it
if (-not (Test-Path -Path $TargetFolder)) {
    New-Item -Path $TargetFolder -ItemType Directory | Out-Null
    Set-Content -Path $LogFile -Value "Folder was missing. Created target folder at $(Get-Date)"
}

# 3. Create a fake junk file inside it to simulate clutter
New-Item -Path "$TargetFolder\junk_file.tmp" -ItemType File -Force | Out-Null

# 4. Automate the Cleanup
Write-Host "Scanning target folder for junk..." -ForegroundColor Cyan
Start-Sleep -Seconds 2 

# Find any file ending in '.tmp' and destroy it
Get-ChildItem -Path $TargetFolder -Filter "*.tmp" | Remove-Item -Force

# 5. Document the success
Add-Content -Path $LogFile -Value "Successfully wiped all .tmp junk files at $(Get-Date)"
Write-Host "Cleanup complete! System log updated." -ForegroundColor Green
