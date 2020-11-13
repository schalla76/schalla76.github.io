---
layout: post
title: "Setting up VirtualBox"
date: 2020-01-09
categories: Virtual Machine
---

## Window 2016 Server

### Install Software

- Install Win2016 OS
- Rename the computer name
- Oracle Virtual Box Guest Additions

- Add below listed Windows components

[Source link](https://hexagonppm.fluidtopics.net/reader/sdCFN~hAeetAIBD8uL2j_A/oZwZa8Isxm34dLI5EqZF_A)

- The Web Server (IIS) role must be installed, including:

  - Security > Basic Authentication
  - Application Development
    - .NET Extensibility 4.6
    - ASP.NET 4.6

- The .NET Framework 4.6 feature must be installed, including:

  - WCF Services
    - HTTP Activation
    - Named Pipes Activation
    - TCP Activation

- The Windows Process Activation Service feature must be installed, including:

  - Process Model
  - Configuration APIs

- The Distributed Transactions service must be installed and enabled (for support of 32-bit COM+ application installation).
- Windows Server 2016 is supported with UAC enabled and set to Level 3 (Default).

- Microsoft .NET Framework 4.6
- Microsoft Visual C++ 2017 64-bit Redistributable Package (for file service compression functionality)

- Install software using chocolatey

```ps
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

choco install googlechrome -y
choco install firefox-dev --pre  -y
choco install git -y
choco install javaruntime -y
# choco install jdk11 -y
choco install lein -y
choco install vscode -y
choco install cmder -y
choco install nodejs -y
choco install winmerge -y
choco install windirstat -y
choco install 7zip -y
choco install adobereader -y
choco install microsoft-office-deployment -y
choco install visualstudio2019enterprise -y
choco install sql-server-2019 -y
choco install sql-server-management-studio -y
choco install citrix-workspace -y
```

- Visual Studio and Winmerge configuration

  - Go to Tools>Options>Source Control>Visual Studio Team Foundation Server>Configure User Tools...

Compare: /e /u /wl /wr /dl %6 /dr %7 %1 %2
Merge: /e /u /wl /dl %6 /dr %7 %1 %2 %4

| Option | Description                                     |
| ------ | ----------------------------------------------- |
| /e     | Esc to close                                    |
| /u     | Avoids adding to Most Recently used menu        |
| /wl    | Opens left side as readonly                     |
| wr     | Opens right side as readonly                    |
| dl     | Provides the description on the left title bar  |
| dr     | Provides the description on the right title bar |
| %1     | Original file                                   |
| %2     | Modified file                                   |
| %3     | Base file                                       |
| %4     | Merged file                                     |
| %5     | Diff command line options                       |
| %6     | Original file label                             |
| %7     | Modified file label                             |
| %8     | Base file label                                 |
| %9     | Merged file label                               |

- Install VS Extension

  - Install the extension .Net Reflector
  - Install the extension [Roslynator](https://github.com/JosefPihrt/Roslynator)
  - Install the extension [ReAttach](https://marketplace.visualstudio.com/items?itemName=ErlandR.ReAttach)
  - Install the extension [Productivity Power Tools](https://marketplace.visualstudio.com/items?itemName=VisualStudioPlatformTeam.ProductivityPowerPack2017)
  - Install the extension [Code Converter](https://marketplace.visualstudio.com/items?itemName=SharpDevelopTeam.CodeConverter)
  - Install the extension [Microsoft Code Analysis 2019](https://marketplace.visualstudio.com/items?itemName=VisualStudioPlatformTeam.MicrosoftCodeAnalysis2019)
  - Install the dark theme for high contrast using below links
    - Run regedit
    - Select HKEY_USERS in the file tree on the left-hand side of the window.
    - Load VS independent registry (hive) into HKEY_USERS.
    - Navigate to HKEY_USERS > VS > Software > Microsoft > VisualStudio > 15.0_XXXXXXXX_Config > Themes.

    The dark theme has a GUID of 1ded0138-47ce-435e-84ef-9ec1f439b749, while the high contrast theme has a GUID of a5c004b4-2d4b-494e-bf01-45fc492522c7. Copy the GUID and delete the registry of the high contrast theme (again, starting with a5c...) (you can back it up by right-clicking it and clicking Export if you wish).

    Paste the GUID of the high contrast theme into the folder name (in the file tree) of the dark theme. Then click the dark theme folder and change "Dark" in the first entry to "High Contrast" and "Dark" in the second entry to "HighContrast." Keep the GUID for the dark theme in there.

    Unload the hive. If you don't do this VS won't open.

    Select VS back up in HKEY_USERS and click File > Unload Hive...
    - [Reddit link](https://www.reddit.com/r/VisualStudio/comments/7tbp6h/visual_studio_2017_high_contrast_theme_dark/)
    - [Screenshots](https://imgur.com/a/TRcrE)

- Other software

  - Setup the cmder to open in powershell admin
  - Update the cmder profile
  - Install vscode extensions

- Take the snapshot of the VM

- Clone the git repos
- Install the git ssh keys
- Add the dark theme
