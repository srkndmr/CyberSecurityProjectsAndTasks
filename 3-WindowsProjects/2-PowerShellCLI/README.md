# 🛠️ PowerShell Commands and Usage Guide

Welcome to the **PowerShell Commands and Usage Guide**! 🎉 This guide is designed to help you get started with PowerShell, a powerful command-line tool and scripting language for Windows. Whether you're managing files, automating tasks, or configuring systems, this guide covers essential commands with clear examples. 🚀  

💻 Happy scripting! 😊  

---


## Table of Contents
- [Get-Help](#get-help)
- [What is Get-Process?](#what-is-get-process)
- [Key Uses of Get-Command](#key-uses-of-get-command)
- [Navigation Commands](#navigation-commands)
- [File Operations](#file-operations)
- [Managing Permissions](#managing-permissions)
- [Package Management](#package-management)
- [Environment Variables](#environment-variables)

---

## Get-Help

### Basic Help Command
To get a brief description of the `Get-Process` command:
```powershell
Get-Help Get-Process
```

### Detailed Information
For more detailed information:
```powershell
Get-Help Get-Process -Detailed
```

### Examples
To see usage examples:
```powershell
Get-Help Get-Process -Examples
```

### Complete Information
To view all technical details:
```powershell
Get-Help Get-Process -Full
```

### Updating Help
If help files are outdated or missing:
```powershell
Update-Help
```

---

## What is Get-Process?

The `Get-Process` cmdlet lists all running processes on your system. Key uses include:
- Viewing currently running applications or services.
- Inspecting specific processes (e.g., memory usage, process ID).

### Examples
List all processes:
```powershell
Get-Process
```

Inspect a specific process (e.g., Notepad):
```powershell
Get-Process notepad
```

Access online help:
```powershell
Get-Help Get-Process -Online
```

---

## Key Uses of Get-Command

### List All Commands
```powershell
Get-Command
```

### Search for a Command
```powershell
Get-Command Get-Process
```

### Filter by Command Type
Cmdlets:
```powershell
Get-Command -CommandType Cmdlet
```

Aliases:
```powershell
Get-Command -CommandType Alias
```

### Detailed Command Information
```powershell
Get-Command Get-Process | Format-List
```

---

## Navigation Commands

### Print Current Location
```powershell
Get-Location
```

Shortcut:
```powershell
pwd
```

### List Directory Content
```powershell
Get-ChildItem
```

Shortcut:
```powershell
ls
```

### Move to Directories
Root:
```powershell
Set-Location C:\
```

Home:
```powershell
Set-Location ~
```

---

## File Operations

### Create, Edit, and Manage Files
Create a file:
```powershell
New-Item -Name story1.txt -ItemType File
```

Write to a file:
```powershell
echo "Hello World" > story1.txt
```

Read file content:
```powershell
Get-Content story1.txt
```

### Folder Management
Create a folder:
```powershell
New-Item -Name story -ItemType Directory
```

Move a file:
```powershell
Move-Item story1.txt story\
```

Delete a folder and its contents:
```powershell
Remove-Item -Recurse -Force story
```

---

## Managing Permissions

### Change File Owner
Retrieve the ACL:
```powershell
$acl = Get-Acl file1.txt
```

Set the owner:
```powershell
$acl.SetOwner([System.Security.Principal.NTAccount]"Administrator")
```

Apply the updated ACL:
```powershell
Set-Acl -Path file1.txt -AclObject $acl
```

---

## Package Management

### Manage Windows Updates
Install PSWindowsUpdate:
```powershell
Install-Module -Name PSWindowsUpdate -Force -Scope CurrentUser
```

Check for updates:
```powershell
Get-WindowsUpdate
```

Install updates:
```powershell
Install-WindowsUpdate -AcceptAll -AutoReboot
```

### Manage Applications with Chocolatey
Install Chocolatey:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

Install VLC:
```powershell
choco install vlc -y
```

---

## Environment Variables

### Working with Environment Variables
Print existing variable:
```powershell
echo $env:COMPUTERNAME
```

Create a new variable:
```powershell
$env:test = "hello powershell"
```

### Modify PATH
Add an executable to PATH:
```powershell
$env:PATH += ";C:\Path\To\Executable"
```

List Environment Variables:
```powershell
Get-ChildItem Env:
```

---

### System Specs Script Example
Collect and display system specs:
```powershell
$report = [PSCustomObject]@{
    ComputerName = $env:COMPUTERNAME
    UserName     = $env:USERNAME
    OSVersion    = (Get-ComputerInfo).WindowsVersion
    CPU          = (Get-CimInstance -ClassName Win32_Processor).Name
    RAM          = "{0:N2} GB" -f ((Get-CimInstance -ClassName Win32_PhysicalMemory | Measure-Object -Property Capacity -Sum).Sum / 1GB)
}
$report
```
