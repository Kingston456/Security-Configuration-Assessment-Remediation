### 🛡️ Wazuh Security Configuration Assessment Lab — Vulnerability Remediation

### 📋 Project Overview

This Homelab demonstrates Security Configuration Assessment and remediation using CIS Benchmarks. Using wazuh and command-line remediation, I improved the SCA compliance score from 46% to 56%.

### Lab Environment:

### Component

- **SIEM** - Wazuh 4.x (Server IP: 172.16.0.10)
- **Target**- Metasploitable 3 (172.16.0.12)
- **Network**- Bridged Adapter + NAT
- **Command Terminal**- Windows 11/Metasploitable 3

### 🎯 Project 3 — Vulnerability Remediation

**Objective**- Harden Metasploitable 3's SCA compliance score by applying CLI-based remediations (via SSH from a Windows 11 host) guided by Wazuh and CIS benchmarks. 

**What i did**

**1. Deployed Wazuh SIEM** as a dedicated VM (172.16.0.10) within the homelab environment.

**2. Established an SSH** session to Metasploitable 3 (172.16.0.12) from Windows 11 Command-Line Terminal.

**3. Researched and applied** remediation steps for each SCA finding" 

**4. Verified remediation** by restarting the Wazuh agent and re-running the SCA scan to confirm failed checks updated to passing.

### 📊Scan Summary

### Target: Metasploitable 3 (172.16.0.12)

### FIRST SCAN

🟢 **PASSED**- 70

🔴 **FAILED**- 80

⚪ **NOT APPLICABLE**- 30

🟣 **TOTAL SCORE**- 46%

### SECOND SCAN

🟢 **PASSED**- 85

🔴 **FAILED**- 66

⚪ **NOT APPLICABLE**- 29

🟣 **TOTAL SCORE**- 56%

### 🔍Findings

- **Finding 1- (19582)
