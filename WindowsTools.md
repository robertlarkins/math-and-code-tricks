# Windows Tools

## Programs

### Package Managers
 - [Chocolatey](https://chocolatey.org/)
   a console based package manager for Windows (it also has a GUI if desired)  
   Most of the following tools can be installed via Chocolatey

#### Chocolatey

Install: https://chocolatey.org/install

Chocolatey programs:

```powershell
choco install vscode --params "/NoDesktopIcon /NoQuicklaunchIcon"
choco install ditto
choco install 7zip
choco install linqpad
choco install microsoft-windows-terminal
choco install docker-desktop
choco install sourcetree
choco install everything --params "/start-menu-shortcuts /run-on-system-startup"
choco install paint.net
choco install vlc
choco install beyondcompare
```

- List of programs installed by chocolatey: `choco list --localonly` 
- Upgrade all programs: `choco upgrade all`

### Disk Size
 - [WizTree](https://antibody-software.com/web/software/software/wiztree-finds-the-files-and-folders-using-the-most-disk-space-on-your-hard-drive/) - recommended
 - WinDirStat
 - TreeSize

### Version Control
 - [SourceTree](https://www.sourcetreeapp.com/) (GIT)

### File Searching
 - [Everything](https://www.voidtools.com/)

### Hardware Monitoring
 - [Open Hardware Monitor](https://openhardwaremonitor.org/)  
   A no install hardware monitoring software, useful for checking CPU temperature.

### Media Players
 - [VLC Media Player](https://www.videolan.org/)
 - [FastStone Image Viewer](https://www.faststone.org/FSViewerDetail.htm)

### Editors
 - [Paint.Net](https://www.getpaint.net/)

### IP Scanning
 - [Angry IP Sanner](https://angryip.org)

### Compression
 - [7zip](https://www.7-zip.org/)

### Web Debugging


### PowerShell Plugins

- [posh-git](https://github.com/dahlbyk/posh-git)
  posh-git is a PowerShell module that integrates Git and PowerShell by providing Git status summary information that can be displayed in the PowerShell promp.


#### Debugging Proxy Server
 - [Fiddler](https://www.telerik.com/fiddler/fiddler-classic)
 - [Charles](https://www.charlesproxy.com)

#### API Client
 - [PostMan](https://www.postman.com)
 - [Insomnia](https://insomnia.rest)
