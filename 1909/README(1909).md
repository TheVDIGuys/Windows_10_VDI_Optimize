# Introduction 
Automatically apply setting referenced in white paper:
                  "Optimizing Windows 10, Build 1909, for a Virtual Desktop Infrastructure (VDI) role"
                  URL: https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-vdi-recommendations-1909 

# Getting Started
 DEPENDENCIES    1. LGPO.EXE (available at https://www.microsoft.com/en-us/download/details.aspx?id=55319)
                 2. Previously saved local group policy settings, available on the GitHub site where this script is located
                 3. This PowerShell script

NOTE: This script can take 10 minutes or more to complete on the reference (gold) VM. A prompt to reboot will appear when the script has comoletely finished running. Wait for this prompt to confirm the script has successfully completed.

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

CHANGE HISTORY (WinX 1909)
- Updated user profile settings to include setting background to blue, so not too much black
- Added a number of optimizations to shell settings for performance
- Updated services input file for service names instead of registry locations
- Disabling services using the Service Control Manager tool 'SC.EXE', not setting registry entries manually
- Changed the method of disabling services, with native PowerShell
- Fixed some small issues with input scripts that caused error messages
- Delete unused .EVTX and .ETL files (very small disk space savings)
- Added a reboot option prompt at the conclusion of the PowerShell script
- Added several LGPO settings to turn off privacy settings on new user logon
- Added settings in default user profile settings to disable suggested content in 'Settings'