# Setting up VirtualBox

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
((new-object net.webclient).DownloadFile('https://chocolatey.org/install.ps1','install.ps1'))
.\install.ps1

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

- Other software

.Net Reflector

- Setup the cmder to open in powershell admin
- Update the cmder profile 
- Install vscode extensions

- Take the snapshot of the VM

- Clone the git repos
- Install the git ssh keys
- Add the dark theme