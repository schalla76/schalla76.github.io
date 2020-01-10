# Setting up VirtualBox

## Window 2016 Server

### Install Software

- Install software using chocolatey

```ps
((new-object net.webclient).DownloadFile('https://chocolatey.org/install.ps1','install.ps1'))
.\install.ps1

choco install git -y

choco install javaruntime -y

choco install jdk11 -y

choco install lein -y

choco install googlechrome -y

choco install vscode -y

choco install cmder -y

choco install nodejs -y

choco install winmerge -y

choco install firefox-dev --pre  -y

choco install windirstat -y

choco install 7zip -y

choco install adobereader -y

choco install microsoft-office-deployment -y

choco install visualstudio2019enterprise -y

choco install sql-server-2019 -y

choco install sql-server-management-studio -y

choco install citrix-workspace
```

Other software

Oracle Virtual Box Guest Additions

.Net Reflector
