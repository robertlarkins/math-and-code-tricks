# Bash on Windows

There are a couple of options for running bash on Windows:
- Git Bash
- Ubuntu Bash

## Install Microsoft Terminal

This can be installed either through the Microsoft Store or via chocolatey
`> choco install microsoft-windows-terminal`


## Git Bash

Git Bash emulates a bash terminal on Windows to provides access to unix commands and the Git CLI.
It comes included as part of the [Git for Windows](https://gitforwindows.org/) install.

Git Bash can be added to Windows Terminal by going to Settings > Add a new profile  
Then click \+ New empty profile
- Name: Git Bash
- Command line: %ProgramFiles%\Git\git-bash.exe
- Icon: %ProgramFiles%\Git\mingw64\share\git\git-for-windows.ico
- Tab Title: Git Bash


## Linux Bash

Turn on Windows Subsystem for Linux (WSL)

Go to Control Panel > Programs > Turn Windows features on or off  
And ensure Windows Subsystem for Linux is turned on.

Install the latest Ubuntu LTS (long term support) version from the Microsoft Store (or your preferred distro, but these instructions will use Ubuntu).

There are different ways of opening the Ubuntu shell terminal:
- From Microsoft Store
- Ubuntu shortcut in the Start menu
- `ubuntu2004` (the 20.04 distro version) from powershell or cmd
- `wsl -r <distro-name>` from powershell, eg: `wsl -r Ubuntu-20.04`
- From Windows Terminal as a new tab - access is automatically added once Ubuntu is installed


### Upgrade from WSL 1 to WSL 2

Get the list of installed Linux distributions by running `wsl -l -v` in PowerShell, this also lists the wsl version used for each.

To upgrade version to WSL 2 run: `wsl --set-version <distro-name> 2`.  
So if the distro name is `Ubuntu-20.04` then this will be `wsl --set-version Ubuntu-20.04 2`

See:
- https://docs.microsoft.com/en-us/windows/wsl/install


### Bash Commands

`wslfetch` provides details about WSL, this is built into the Ubuntu WSL distro.
Other distros need the WSLU package to be installed.


### Docker Desktop Integration

The Linux distro can be Ubuntu can be integrated with Docker Desktop to allow docker to be run from the distro using Docker Desktop. This is done by going

> Settings > Resources > WSL Integration

and enabling the desired distro, then _Apply & Restart_.

See:
- https://docs.docker.com/desktop/windows/wsl/


### Running GIT

Git appears to be automatically installed (at least with Ubuntu).

There are different options for where git repos can be stored, but for convenience just put it in a folder (eg: `~/dev`) in your home directory.

Doing a `git clone` will (if necessary) request your username and password to access this repo.


## Running in VSCode

Files and Folders in a WSL Distro can be opened in VSCode in Windows.

- Firstly install the *Remote - WSL* extension in VSCode.
- From the Distro terminal navigate to the desired directory and enter `code .` to open the folder in VSCode.
To open a file enter `code <file-name>`.

If the VSCode session is connected to the Linux distro it will show the Distro name in the bottom left.

The VSCode terminal will open to the distro terminal.

See:
- https://docs.docker.com/desktop/windows/wsl/#develop-with-docker-and-wsl-2


### Keeping Ubuntu up-to-date

Ubuntu can be kept up-to-date in the terminal by doing the following:
- `sudo apt update` to check for package updates
- `apt list --upgradable` to list all packages that can be upgraded 
- `sudo apt upgrade` to upgrade all the packages
