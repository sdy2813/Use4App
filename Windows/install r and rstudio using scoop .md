# Install R and RStudio using Scoop

**Scoop** is a command-line package manager for Windows, which is designed to make it easier for users to install and manage software on their Windows systems. It allows users to easily install, uninstall, and update applications from the command line, which can save time and effort compared to using graphical installers or manual installation.

Scoop is similar to package managers found in Linux distributions like APT for Ubuntu, yum for Red Hat, or pacman for Arch Linux. It is designed to simplify the installation process for developers and power users, providing a convenient and fast way to set up and maintain their development environments. To use Scoop, you need to have PowerShell installed on your Windows system.

## install Scoop

### Default installation

 (install to C:\Users<user>\scoop), run in PowerShell:"

```powershell
iex (new-object net.webclient).downloadstring('https://get.scoop.sh')
#or
iwr -useb get.scoop.sh | iex

```

### Install to a specified directory

```powershell
$env:SCOOP='D:\Scoop'

[environment]::setEnvironmentVariable('SCOOP',$env:SCOOP,'User')

iex (new-object net.webclient).downloadstring('https://get.scoop.sh')

```

### Install to the global directory 

```powershell
# Install to the global directory with -g option (D:\Scoop\global), run as administrator
[environment]::setEnvironmentVariable('SCOOP_GLOBAL','D:\Scoop\global','Machine')
$env:SCOOP_GLOBAL='D:\Scoop\global'
```

### Verify if Scoop has been installed successfully

```powershell
scoop help
```

##  Usage of Scoop

### Basical uasge

```powershell
$ scoop help
Usage: scoop <command> [<args>]                                                                                                         Some useful commands are:

alias       Manage scoop aliases
bucket      Manage Scoop buckets
cache       Show or clear the download cache
cat         Show content of specified manifest.
checkup     Check for potential problems
cleanup     Cleanup apps by removing old versions
config      Get or set configuration values
create      Create a custom app manifest
depends     List dependencies for an app
export      Exports (an importable) list of installed apps
help        Show help for a command
hold        Hold an app to disable updates
home        Opens the app homepage
info        Display information about an app
install     Install apps
list        List installed apps
prefix      Returns the path to the specified app
reset       Reset an app to resolve conflicts
search      Search available apps
status      Show status and check for new app versions
unhold      Unhold an app to enable updates
uninstall   Uninstall an app
update      Update apps, or Scoop itself
virustotal  Look for app's hash on virustotal.com
which       Locate a shim/executable (similar to 'which' on Linux)
```

+ Shims Directory

After installing Scoop, a corresponding shim.exe for the installed program will be generated in the shims directory under the Scoop installation directory (not sure why it's not a shortcut).

After installing Scoop, the shims directory is automatically added to the user's Path.

+ Persist Directory

The persist directory contains the configuration files for installed software. This part will not be modified when updating software in the future.

If you want to run a command as an administrator without opening cmd (or PowerShell) as an administrator, simply add "sudo" before the command  in the command line. `scoop install sudo`

### Install Git and add a new repository

```powershell
scoop install git

#cAdd the officially maintained extras repository (which includes a large number of GUI programs).
scoop bucket add extras
# or
scoop bucket add extras https://kgithub.com/ScoopInstaller/Extras.git
#or
scoop bucket add extras https://gitee.com/scoop-bucket/extras.git

# Use Github Action to automatically merge updates from other Scoop repositories every day.
scoop bucket add apps https://github.com/kkzzhizhou/scoop-apps

# update scoop
scoop update
```

### Enhance search functionality

The search experience in Scoop is really bad, so it is recommended to install scoop-search to improve the Scoop search experience.

```powershell
scoop install scoop-search
```

## Install R and RStudio using Scoop


```powershell
# install R
scoop install r

# Sensible aliases, and an easy way to add your own aliases and remove ones you don't like
scoop install pshazz

# search rstudio from repository
scoop search rstudio

# install it
scoop install rstudio

# Then you can easily manage your R versions.
scoop update r
```

## other appliaction
```powershell
scoop install trafficmonitor
scoop search typora
scoop install vim
scoop install exa
scoop install yt-dlp
scoop install ffmpeg
scoop install sudo
scoop install mambaforge
scoop install fd
scoop install duf
scoop install grep
scoop install sed
scoop install gawk
scoop install scoop-search
scoop install zotero
scoop install scoop-completion
```
 
 
 
## Reference

1. [Scoop](https://scoop.sh/)
2. [scoop-apps](https://github.com/kkzzhizhou/scoop-apps)
3. [Scoop Extras](https://github.com/ScoopInstaller/Extras)
