# Introduction 
Automatically apply setting referenced in white paper:
                  "Optimizing Windows 10 for a Virtual Desktop Infrastructure (VDI) role"
                  URL: https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-vdi-recommendations-1803 
                  URL: Update once later versions are published on docs.microsoft.com

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
# IMPORTANT ISSUE (01/27/2020)
A new issue was discovered recently regarding the 'CDPSvc'. If that service is disabled, and
a new user logs on to the computer then opens 'System Settings' to view display settings,
'SystemSettings.exe' will crash and log an error to the event log with code "fatal app exit".
We removed the entry 'CDPSvc' from 'Win10_1909_ServicesDisable.txt' as a result.

# Low-impact ISSUE (04/20/2020)
Previously these scripts had a local policy setting at this location set to disabled:

**Local Computer Policy \ Computer Configuration \ Administrative Templates \ System \ Internet Communication Management \ Internet Communication settings**
```
Turn off Windows Network Connectivity Status Indicator active tests
```
With the active tests disabled, Office 365 is not able to contact it's licensing service, and therefore would not run any of the Office apps.  This setting has been changed back to "Not configured" in the included LGPO file.

# Low-impact ISSUE (04/22/2020)

In some virtual environments, such as Azure Windows Virtual Desktop, some of the application windows will have no border.  An example is Windows File Explorer.  You can replicate this by opening Wordpad and File Explorer, then move then around and note that you may not see a border where one app starts and the other ends.
One of the optimizations in the latest drop changes the Visual Effects settings (found in System Properties) to reduce animations and effects, while still maintaining a good user experience such as "smoothing screen fonts".
The other two optimizations: "show shadows under mouse pointer" and "Show shadows under windows" will enable a shadow effect around the windows like File Explorer, so that the border of the app is now visible.
These settings are written to the default user profile registry hive, so would apply only to users whose profile is created after these optimizations run, and on this computer.

# Low-impact ISSUE (04/29/2020)
Several of the built-in UWP apps, such as Skype, Phone, and Photos, will start processes and run in the background, even though the user has not started the app(s).  On a single machine this is near-zero impact, but on multi-session Windows, it can be a slightly larger impact issue.  There is a setting in the 'Settings' app, under 'Background apps' that allows you to control this behavior on a per-user basis.  However, there is currently no way to change this behavior as a global setting, other than to completely uninstall the app.

If you would like to keep one or more of these apps in your image, and still control the background behavior, you can edit the default user registry hive and set the following settings:

"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe /v Disabled /t REG_DWORD /d 1 /f
"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe /v DisabledByUser /t REG_DWORD /d 1 /f
"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c /v Disabled /t REG_DWORD /d 1 /f
"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c /v DisabledByUser /t REG_DWORD /d 1 /f
"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe /v Disabled /t REG_DWORD /d 1 /f
"HKCU\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe /v DisabledByUser /t REG_DWORD /d 1 /f

You could also set these settings with Group Policy Preferences, and should take effect after a log off and log back on.
