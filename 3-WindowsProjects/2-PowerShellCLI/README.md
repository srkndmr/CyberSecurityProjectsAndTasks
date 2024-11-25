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

powershell
Copy code

Get-Help Get-Process -Detailed
Examples
To see usage examples:

powershell
Copy code
Get-Help Get-Process -Examples
Complete Information
To view all technical details:

powershell
Copy code
Get-Help Get-Process -Full
Updating Help
If help files are outdated or missing:

powershell
Copy code
Update-Help
What is Get-Process?
The Get-Process cmdlet lists all running processes on your system. Key uses include:

Viewing currently running applications or services.
Inspecting specific processes (e.g., memory usage, process ID).
Examples:

List all processes:

powershell
Copy code
Get-Process
Inspect a specific process (e.g., Notepad):

powershell
Copy code
Get-Process notepad
Access Online Help
Use Get-Help -Online to open documentation in a browser:

powershell
Copy code
Get-Help Get-Process -Online
Key Uses of Get-Command
List All Commands

powershell
Copy code
Get-Command
Search for a Command

powershell
Copy code
Get-Command Get-Process
Filter by Command Type
Cmdlets:

powershell
Copy code
Get-Command -CommandType Cmdlet
Aliases:

powershell
Copy code
Get-Command -CommandType Alias
Detailed Command Information

powershell
Copy code
Get-Command Get-Process | Format-List
Navigation Commands
Print Current Location

powershell
Copy code
Get-Location
Shortcut:

powershell
Copy code
pwd
List Directory Content

powershell
Copy code
Get-ChildItem
Shortcut:

powershell
Copy code
ls
Move to Directories
Root:

powershell
Copy code
Set-Location C:\
Home:

powershell
Copy code
Set-Location ~
File Operations
Create, Edit, and Manage Files
Create a file:

powershell
Copy code
New-Item -Name story1.txt -ItemType File
Write to a file:

powershell
Copy code
echo "Hello World" > story1.txt
Read file content:

powershell
Copy code
Get-Content story1.txt
Folder Management
Create a folder:

powershell
Copy code
New-Item -Name story -ItemType Directory
Move a file:

powershell
Copy code
Move-Item story1.txt story\
Delete a folder and its contents:

powershell
Copy code
Remove-Item -Recurse -Force story
Managing Permissions
Change File Owner
Retrieve the ACL:

powershell
Copy code
$acl = Get-Acl file1.txt
Set the owner:

powershell
Copy code
$acl.SetOwner([System.Security.Principal.NTAccount]"Administrator")
Set-Acl -Path file1.txt -AclObject $acl
Package Management
Manage Windows Updates
Install PSWindowsUpdate:

powershell
Copy code
Install-Module -Name PSWindowsUpdate -Force -Scope CurrentUser
Check for updates:

powershell
Copy code
Get-WindowsUpdate
Install updates:

powershell
Copy code
Install-WindowsUpdate -AcceptAll -AutoReboot
Manage Applications with Chocolatey
Install Chocolatey:

powershell
Copy code
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
Install VLC:

powershell
Copy code
choco install vlc -y
Environment Variables
Working with Environment Variables
Print existing variable:

powershell
Copy code
echo $env:COMPUTERNAME
Create a new variable:

powershell
Copy code
$env:test = "hello powershell"
Modify PATH
Add an executable to PATH:

powershell
Copy code
$env:PATH += ";C:\Path\To\Executable"
List Environment Variables

powershell
Copy code
Get-ChildItem Env:
System Specs Script Example
Collect and display system specs:

powershell
Copy code
$report = [PSCustomObject]@{
    ComputerName = $env:COMPUTERNAME
    UserName     = $env:USERNAME
    OSVersion    = (Get-ComputerInfo).WindowsVersion
    CPU          = (Get-CimInstance -ClassName Win32_Processor).Name
    RAM          = "{0:N2} GB" -f ((Get-CimInstance -ClassName Win32_PhysicalMemory | Measure-Object -Property Capacity -Sum).Sum / 1GB)
}
$report
