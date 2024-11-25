# PowerShell Commands and Usage Guide

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
Detailed Information
For more detailed information:


Get-Help Get-Process -Detailed
Examples
To see usage examples:


Get-Help Get-Process -Examples
Complete Information
To view all technical details:


Get-Help Get-Process -Full
Updating Help
If help files are outdated or missing:


Update-Help
What is Get-Process?
The Get-Process cmdlet lists all running processes on your system. Key uses include:

Viewing currently running applications or services.
Inspecting specific processes (e.g., memory usage, process ID).
Examples:

List all processes:

Get-Process
Inspect a specific process (e.g., Notepad):

Get-Process notepad
Access Online Help
Use Get-Help -Online to open documentation in a browser:


Get-Help Get-Process -Online
Key Uses of Get-Command
List All Commands

Get-Command
Search for a Command

Get-Command Get-Process
Filter by Command Type
Cmdlets:

Get-Command -CommandType Cmdlet
Aliases:

Get-Command -CommandType Alias
Detailed Command Information

Get-Command Get-Process | Format-List
Navigation Commands
Print Current Location

Get-Location
Shortcut:

pwd
List Directory Content

Get-ChildItem
Shortcut:


ls
Move to Directories
Root:

Set-Location C:\
Home:

Set-Location ~
File Operations
Create, Edit, and Manage Files
Create a file:

New-Item -Name story1.txt -ItemType File
Write to a file:

echo "Hello World" > story1.txt
Read file content:

Get-Content story1.txt
Folder Management
Create a folder:

New-Item -Name story -ItemType Directory
Move a file:

Move-Item story1.txt story\
Delete a folder and its contents:

Remove-Item -Recurse -Force story
Managing Permissions
Change File Owner
Retrieve the ACL:

$acl = Get-Acl file1.txt
Set the owner:

$acl.SetOwner([System.Security.Principal.NTAccount]"Administrator")
Set-Acl -Path file1.txt -AclObject $acl
Package Management
Manage Windows Updates
Install PSWindowsUpdate:

Install-Module -Name PSWindowsUpdate -Force -Scope CurrentUser
Check for updates:

Get-WindowsUpdate
Install updates:

Install-WindowsUpdate -AcceptAll -AutoReboot
Manage Applications with Chocolatey
Install Chocolatey:

Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
Install VLC:

choco install vlc -y
Environment Variables
Working with Environment Variables
Print existing variable:

echo $env:COMPUTERNAME
Create a new variable:

$env:test = "hello powershell"
Modify PATH
Add an executable to PATH:

$env:PATH += ";C:\Path\To\Executable"
List Environment Variables

Get-ChildItem Env:
System Specs Script Example
Collect and display system specs:

$report = [PSCustomObject]@{
    ComputerName = $env:COMPUTERNAME
    UserName     = $env:USERNAME
    OSVersion    = (Get-ComputerInfo).WindowsVersion
    CPU          = (Get-CimInstance -ClassName Win32_Processor).Name
    RAM          = "{0:N2} GB" -f ((Get-CimInstance -ClassName Win32_PhysicalMemory | Measure-Object -Property Capacity -Sum).Sum / 1GB)
}
$report
