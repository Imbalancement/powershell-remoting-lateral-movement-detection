# PowerShell Remoting Detection Lab

## 🚨 Lateral Movement Detection with PowerShell Remoting + Wazuh

## 📌 Overview

This lab simulates lateral movement in an Active Directory environment using PowerShell Remoting from a Windows 10 workstation to a Windows Server domain controller. The activity was then detected using Windows Event Logs and Wazuh threat hunting queries.

The goal of this lab was to understand how PowerShell Remoting appears from a defender’s perspective and how a SOC analyst can identify suspicious remote command execution using endpoint telemetry.

## 🎯 Objectives

- Simulate remote command execution using PowerShell Remoting
- Generate Windows telemetry from a realistic lateral movement technique
- Detect PowerShell script block activity using Event ID `4104`
- Confirm visibility inside Wazuh
- Document the detection process in a clear SOC-style investigation format

## 🧠 Skills Demonstrated

- Active Directory lab administration
- PowerShell Remoting / WinRM
- Windows Event Log analysis
- PowerShell Script Block Logging
- Wazuh threat hunting
- Detection engineering
- MITRE ATT&CK mapping
- SOC-style evidence documentation

## 🏗️ Lab Environment

| System | Role |
|---|---|
| `dc01` | Windows Server Domain Controller |
| `win10-lab` | Domain-joined Windows 10 workstation |
| `Wazuh Server` | SIEM / log analysis platform |
| `LAB.LOCAL` | Active Directory domain |

## ⚔️ Simulated Technique

This lab focuses on PowerShell Remoting, which can be abused by attackers for lateral movement across Windows systems.

**MITRE ATT&CK Mapping:**

| Tactic | Technique |
|---|---|
| Lateral Movement | T1021.006 — Windows Remote Management |

## 🔎 Detection Focus

The main detection source for this lab was PowerShell Script Block Logging.

| Log Source | Event ID | Purpose |
|---|---:|---|
| Microsoft-Windows-PowerShell/Operational | `4104` | Captures PowerShell script block activity |
| Windows Security | `4624` | Successful logon activity |
| Windows Security | `4688` | Process creation activity |
| Wazuh | N/A | Centralized detection and threat hunting |

## 🧪 Lab Summary

PowerShell Remoting was enabled on both the Windows workstation and the domain controller. From `win10-lab`, a remote command was executed against `dc01` using `Invoke-Command`.

The command successfully returned the remote hostname, user context, and network configuration from the domain controller. This generated PowerShell telemetry on `dc01`, which was confirmed locally in Event Viewer and centrally inside Wazuh.

## ✅ Key Finding

The most valuable detection evidence was **Event ID 4104**, which captured the remote PowerShell script block:

```powershell
whoami; hostname; ipconfig
```
## 📸 Evidence Walkthrough

### Step 1 — PowerShell Remoting Enabled on the Target Host

PowerShell Remoting was enabled on the domain controller `dc01` to allow remote PowerShell connections over WinRM. This created the necessary conditions to simulate lateral movement from the Windows 10 workstation to the domain controller.

**Command Used:**

```powershell
Enable-PSRemoting -Force
```
<img width="1016" height="842" alt="Enable-PSRemoting" src="https://github.com/user-attachments/assets/941c7a1d-4897-4cf4-a60a-148db51df0a6" />

