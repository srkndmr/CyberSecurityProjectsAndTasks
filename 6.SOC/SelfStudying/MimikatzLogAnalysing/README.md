# 🚩 Analysis of the Event Log (Mimikatz LSASS Access Attempt)

![Screenshot 2025-02-25 at 15 46 06](https://github.com/user-attachments/assets/29497640-aa16-4e38-97f0-1fb1ec82cd41)

This event log clearly shows a typical Mimikatz attack attempt on the LSASS process. Here's a breakdown of the key details from your screenshot:

## 🔍 Key Indicators of Compromise (IOC):

| **Field**           | **Value**                                                                |
|---------------------|--------------------------------------------------------------------------|
| **SourceImage**     | `C:\Users\THM-Threat\Downloads\mimikatz.exe`                         |
| **TargetImage**     | `C:\Windows\system32\lsass.exe`                                       |
| **GrantedAccess**   | `0x1010` (Indicates Read/Write access → suspicious for LSASS access)     |
| **SourceProcessId** | `3604`                                                                   |
| **TargetProcessId** | `744`                                                                    |
| **CallTrace**       | Shows Mimikatz calling Windows API functions to interact with LSASS memory |

---

## 🚨 What Makes This Suspicious?

### Direct Access to LSASS:
- `mimikatz.exe` is trying to interact with `lsass.exe`, a highly sensitive process.

### Access Rights (0x1010):
- This hexadecimal value corresponds to permissions used for reading and writing process memory, which is a clear sign of credential dumping behavior.

### Call Trace Analysis:
- The `CallTrace` section reveals function calls to sensitive system libraries like:
  - `ntdll.dll`
  - `KERNELBASE.dll`
  - `KERNEL32.DLL`
- These libraries are often used for low-level memory operations and process manipulation.

---

## 🛡️ How to Detect Similar Behavior in Event Viewer (Using XML Filter)

To hunt for similar LSASS access attempts via Event Viewer, use the following **XML Filter**:

```xml
<QueryList>
  <Query Id="0" Path="Microsoft-Windows-Sysmon/Operational">
    <Select Path="Microsoft-Windows-Sysmon/Operational">
      *[System[(EventID=10)]]
      and
      *[EventData[Data[@Name='TargetImage'] and (contains(., 'lsass.exe'))]]
      and
      *[EventData[Data[@Name='GrantedAccess'] and (contains(., '0x1010') or contains(., '0x1410') or contains(., '0x1F0FFF'))]]
    </Select>
  </Query>
</QueryList>
```

### 🔍 Explanation of the Filter:
- **Event ID 10** → Sysmon logs process access events.
- **TargetImage** → Filters for attempts to access `lsass.exe`.
- **GrantedAccess** → Looks for suspicious access permissions commonly used in credential dumping.

---

## ⚙️ Immediate Response Actions:

### Kill the Suspicious Process:

```powershell
Stop-Process -Id 3604 -Force
```

### Isolate the Affected Machine:
- Disconnect the machine from the network to prevent lateral movement.

### Collect Evidence:

- Export Sysmon logs for further analysis.
- Create a memory dump for forensic analysis using:

```powershell
Get-Process -Name lsass | ForEach-Object { .\procdump.exe -ma $_.Id C:\Dumps\lsass_dump.dmp }
```
*(Only run this in an isolated forensic environment.)*

### Check for Persistence:
- Review scheduled tasks, startup folders, and the registry for malicious persistence mechanisms.

---

## 📊 Creating Alerts in SIEM (Example - Splunk Query):

```spl
index=sysmon EventID=10 TargetImage="*lsass.exe*" (GrantedAccess="0x1010" OR GrantedAccess="0x1F0FFF")
```

This Splunk query will alert whenever unauthorized LSASS access attempts are detected.

