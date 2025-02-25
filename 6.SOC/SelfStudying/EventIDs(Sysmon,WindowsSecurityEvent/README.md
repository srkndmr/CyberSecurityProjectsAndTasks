# 🔍 In Sysmon (System Monitor by Microsoft): Event ID 10 - Process Access Events

In **Sysmon**, **Event ID 10** corresponds to **Process Access events**. This event is logged when one process tries to access the memory or handle of another process—like in the case of **Mimikatz** trying to access the **LSASS** process.

---

## 🔐 What Does Event ID 10 Represent?

**Event ID 10** in Sysmon captures details whenever a process uses Windows API functions like:

- `OpenProcess()`
- `ReadProcessMemory()`
- `WriteProcessMemory()`
- `VirtualAllocEx()`
- `CreateRemoteThread()`

### 🚩 **Common Malicious Use Cases:**

These functions are commonly abused by attackers for:

- Credential dumping (e.g., from LSASS using Mimikatz)
- Code injection into legitimate processes
- Process hollowing or DLL injection

---

## 📊 Why Is This Critical for LSASS Monitoring?

When tools like **Mimikatz** attempt to dump credentials, they typically:

1. **Open LSASS.exe process** using `OpenProcess()` to gain access.
2. **Read LSASS memory** using `ReadProcessMemory()` to extract sensitive information like password hashes.
3. **Elevate privileges** or bypass security restrictions.

Since LSASS handles sensitive data (passwords, tokens, hashes), unauthorized memory access attempts should **never** occur unless initiated by legitimate security or diagnostic tools.

---

## 🛡️ Example of a Sysmon Event ID 10 Log (Process Access Detection):

| **Field**             | **Value**                                    |
|-----------------------|----------------------------------------------|
| **SourceImage**       | `C:\Users\THM-Threat\Downloads\mimikatz.exe` |
| **TargetImage**       | `C:\Windows\System32\lsass.exe`              |
| **GrantedAccess**     | `0x1010` (Read/Write permissions)             |
| **SourceProcessId**   | ID of Mimikatz process                        |
| **TargetProcessId**   | ID of LSASS process                           |
| **CallTrace**         | Function calls made to access LSASS memory    |

---

## ⚙️ When Should You Monitor Event ID 10?

Monitor **Event ID 10** especially when:

- The **target process** is `lsass.exe`.
- Access permissions like `0x1010`, `0x1410`, or `0x1F0FFF` are granted.
- Unusual processes (e.g., `cmd.exe`, `powershell.exe`, or any unsigned binaries) are trying to access sensitive processes.

### 🚩 **Common Tools Triggering Event ID 10 (Malicious & Legitimate):**

| **Malicious Tools**       | **Legitimate Tools**         |
|---------------------------|------------------------------|
| Mimikatz                  | Task Manager (read access)   |
| Cobalt Strike             | Windows Defender diagnostics |
| Metasploit payloads       | Antivirus scanning engines   |
| Custom malware (injections) | Sysinternals ProcDump      |

---

## 💡 Conclusion:

- Event ID **10** is a key event for detecting unauthorized memory access attempts (such as Mimikatz).
- Monitoring this event, especially against processes like **LSASS**, can help detect credential dumping attempts early.
- You can further enhance detection by correlating **Event ID 10** with process creation events (**Event ID 1**) and network activity events (**Event ID 3**).

---

# 🔍 📋 **Common Sysmon Event IDs (for Advanced Monitoring):**

| **Event ID** | **Event Type**                       | **Description**                                                           |
|-------------|--------------------------------------|---------------------------------------------------------------------------|
| **1**       | Process Creation                     | Logs every time a process is started (includes full command-line details). |
| **2**       | File Creation Time                   | Logs when a file is created or modified.                                  |
| **3**       | Network Connection                   | Captures outbound network connections initiated by a process.              |
| **4**       | Sysmon Service State Change          | Logs when the Sysmon service starts or stops.                             |
| **5**       | Process Termination                  | Logs when a process terminates.                                           |
| **6**       | Driver Loaded                        | Logs when a driver (.sys file) is loaded onto the system.                  |
| **7**       | Image Loaded                         | Logs when a module (DLL or executable) is loaded into a process.           |
| **8**       | CreateRemoteThread                   | Logs when a process creates a thread in another process (common in injections). |
| **9**       | Raw Access Read                      | Detects raw read access to disk, useful for identifying tools bypassing file system APIs. |
| **10**      | Process Access                       | Logs when a process attempts to access the memory of another process (critical for LSASS monitoring). |
| **11**      | File Creation                        | Logs file creation operations (including temporary files).                 |
| **12**      | Registry Object Added or Deleted     | Logs changes to the Windows Registry (useful for persistence detection).   |
| **13**      | Registry Value Set                   | Logs when a registry value is modified.                                   |
| **14**      | Registry Key or Value Renamed        | Logs when a registry key or value is renamed.                             |
| **15**      | File Stream Created                  | Detects alternate data streams creation (used by attackers for hiding data).|
| **22**      | DNS Query                            | Logs DNS queries made by a process (useful for detecting C2 communications).|

---

# 🔐 🛡️ **Common Windows Security Event Log IDs (Native Windows Logs):**

| **Event ID** | **Event Type**                    | **Description**                                                              |
|-------------|-----------------------------------|------------------------------------------------------------------------------|
| **4624**    | Successful Logon                  | Logs every successful login attempt (includes logon type & user details).     |
| **4625**    | Failed Logon                      | Logs failed login attempts (useful for brute force detection).                |
| **4634**    | Logoff Event                      | Indicates when a user logs off.                                               |
| **4648**    | Logon Using Explicit Credentials  | Detects attempts to log in using specific credentials.                        |
| **4672**    | Special Privileges Assigned       | Logs when special admin privileges are assigned to a user.                     |
| **4688**    | Process Creation                  | Same as Sysmon ID 1 but native to Windows logs (basic details).               |
| **4689**    | Process Termination               | Same as Sysmon ID 5 (process termination).                                   |
| **4697**    | New Service Installed             | Logs when a new Windows service is installed (can indicate persistence setup). |
| **4720**    | User Account Created              | Logs when a new user account is created (possible sign of attacker activity). |
| **4722**    | User Account Enabled              | Indicates if a disabled account was enabled again.                            |
| **4723**    | Password Change Attempt           | Logs when a user attempts to change their password.                           |
| **4725**    | User Account Disabled             | Logs when a user account is disabled.                                         |
| **4771**    | Kerberos Pre-Authentication Failed| Logs failed Kerberos authentication attempts.                                |
| **5140**    | Shared Object Accessed            | Logs access to shared network resources (SMB access monitoring).               |

---

# 🔎 💡 **Real-World Use Cases for Event IDs:**

### 🔑 **Detect Credential Dumping (Mimikatz) → Sysmon Event ID 10:**
- Alert on processes accessing `lsass.exe` with high privileges.

### ⚠️ **Suspicious Process Creation → Sysmon Event ID 1:**
- Detect unexpected processes like `powershell.exe` running unusual commands:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" | Where-Object { $_.Id -eq 1 -and $_.Message -match "Invoke-Expression" }
```

### 🚨 **Brute Force Attack Detection → Security Event ID 4625:**
- Monitor multiple failed logon attempts from the same IP or account.

### 🔗 **Lateral Movement Detection → Sysmon Event ID 3:**
- Alert on network connections to high-value systems (e.g., Domain Controllers).

### 🕵️ **Persistence Detection via Registry Changes → Sysmon Event ID 13:**
- Detect attackers modifying registry keys for startup persistence.

---

# 🛡️ **Why Event IDs Matter for SOC Analysts:**

- 🎯 **Precision:** Quickly pinpoint specific activities (process creation, memory access, logins).
- 🔍 **Threat Detection:** Helps correlate malicious patterns across different logs.
- 📊 **SIEM Integration:** You can create custom alerts based on specific Event IDs (e.g., Splunk, ELK, Sentinel).

