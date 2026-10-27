# Day 1: Windows Event Log Analysis

## Objective
Analyze local Windows security logs to identify authentication attempts and process creation, mapping these to real-world SOC monitoring tasks.

## 1. Event ID 4625: Failed Logon
**What this is:** This event generates every time a user fails to log into the system.
**Why SOC monitors this:** A high volume of 4625 events in a short time indicates a Brute Force attack (MITRE ATT&CK T1110).

**My Findings:**
![Event ID 4625 Failed Logon](../screenshots/failed-logon.jpg)

*   **Account Name targeted:** atik
*   **Failure Reason:** Unknown user name or bad password.

## 2. Event ID 4624: Successful Logon
**What this is:** This event generates when a user successfully authenticates.
**Why SOC monitors this:** To track who is accessing the system and how.

**My Findings:**
![Event ID 4624 Successful Logon](../screenshots/success-logon.jpg)

*   **Logon Type:** 2 (Interactive / Physical Keyboard)
*   *(Analyst Note: Logon Type 2 means someone was physically at the keyboard. Logon Type 3 means they logged in over the network. If I see Type 3 from a strange IP address, it is a major security incident).*

## 3. Sysmon & Process Creation
**What this is:** Sysmon monitors and logs system activity to the Windows event log. It provides detailed information about process creation.
**Why SOC monitors this:** If malware gets on a computer, it has to run a process. Monitoring processes helps detect malicious commands (like PowerShell executing enumeration commands).

**My Findings:**
![Sysmon Process Creation](../screenshots/powershell-process.png)

*   **Process Executed:** `whoami`
*   **User Context:** `desktop-mp363ir\atik`
*   *(Analyst Note: An attacker often runs `whoami` immediately after gaining a shell to determine their privilege level. Detecting this command running from a suspicious parent process is a high-fidelity alert).*