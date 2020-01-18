# Introduction 
Automatically apply setting referenced in white paper:
                  "Optimizing Windows 10, Build 1803, for a Virtual Desktop Infrastructure (VDI) role"
                  URL: https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-vdi-recommendations-1803 

# Getting Started
 DEPENDENCIES    1. LGPO.EXE (available at https://www.microsoft.com/en-us/download/details.aspx?id=55319)
                 2. Previously saved local group policy settings, available on the GitHub site where this script is located
                 3. This PowerShell script

- REFERENCES:
https://social.technet.microsoft.com/wiki/contents/articles/7703.powershell-running-executables.aspx
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-6
https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-service?view=powershell-6
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-6
https://msdn.microsoft.com/en-us/library/cc422938.aspx

- Appx package cleanup                 - Complete
- Scheduled tasks                      - Complete
- Automatic Windows traces             - Complete
- OneDrive cleanup                     - Complete
- Local group policy                   - Complete
- System services                      - Complete
- Disk cleanup                         - Complete
- Default User Profile Customization   - Complete

This script is dependant on three elements:
LGPO Settings folder, applied with the LGPO.exe Microsoft app

# IMPORTANT ISSUE (01/17/2020)
IMPORTANT: There is a setting in the current LGPO files that should not be set by default. As of 1/17/10...
a fix has been checked in to the "Pending" branch.  Once we confirm that resolves the issue we will merge...
into the "Master" branch.  The issue is that Windows will not check certificate information, and thus...
program installations could fail.  The temporary workaround is to open GPEDIT.MSC on the reference image...
The set the policy to "not configured".  Here is the location of the policy setting:

**Local Computer Policy \ Computer Configuration \ Administrative Templates \ System \ Internet Communication Management \ Internet Communication settings**

```
Turn off Automatic Root Certificates Update
```
