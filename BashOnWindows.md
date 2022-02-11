# Bash on Windows

There are a couple of options for running bash on Windows:
- Git Bash
- Ubuntu Bash

## Install Microsoft Terminal

This can be installed either through the Microsoft Store or via chocolatey
`> choco install microsoft-windows-terminal`


## Git Bash

Turn on Windows Subsystem for Linux

Go to Control Panel > Programs > Turn Windows features on or off  
And ensure Windows Subsystem for Linux is turned on.

Git Bash can be added to Windows Terminal by going to Settings > Add a new profile  
Then click \+ New empty profile
- Name: Git Bash
- Command line: %ProgramFiles%\Git\git-bash.exe
- Icon: %ProgramFiles%\Git\mingw64\share\git\git-for-windows.ico
- Tab Title: Git Bash


## Linux Bash

Install LTS version of Ubuntu from the Microsoft Store (or your preferred distro)
Open the distro

Powershell `wpl` or `bash`

### Keeping Ubuntu up-to-date
