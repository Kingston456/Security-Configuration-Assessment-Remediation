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

<img width="959" height="599" alt="SCA 46%" src="https://github.com/user-attachments/assets/74ed569d-35f9-49d3-a633-4debea283813" />

### SECOND SCAN

🟢 **PASSED**- 85

🔴 **FAILED**- 66

⚪ **NOT APPLICABLE**- 29

🟣 **TOTAL SCORE**- 56%

<img width="959" height="599" alt="SCA score 56%" src="https://github.com/user-attachments/assets/34e4ce52-f31e-499f-8f0e-91881b6fe914" />

### 🔍Sample Findings

- **Finding 1- 19557: Ensure time synchronization is in use**

- **Status:** Remediated ✅

- **Issue:** No NTP or chrony service was installed, meaning the system had no mechanism for synchronized timekeeping.

- **Action Taken:** Installed NTP using apt-get on virtual machine.
<img width="608" height="500" alt="19557 NTP fail" src="https://github.com/user-attachments/assets/14607446-bb85-47cd-b270-44b96b63b466" />

<img width="959" height="599" alt="19557 NTP remediation" src="https://github.com/user-attachments/assets/056db44c-7b68-48a2-8036-11693666fc1a" />

<img width="607" height="500" alt="19557 NTP pass" src="https://github.com/user-attachments/assets/18c4e5fc-1598-441e-b312-d9a6ea08b12d" />


- **Finding 2- 19522 & 19523: Make sure AIDE is Installed & Ensure filesystem integrity is regularly checked**

- **Status:** Remediated ✅

- **Issue:** No File Integrity Monitoring tools was installed, meaning the system had no way of checking if critical files have been altered without authorization.

- **Action Taken:** Installed & initialized AIDE. I then Verified the check ran clean against the baseline using aide.wrapper --check. Scheduled automated daily integrity checks via root's crontab (0 5 * * * /usr/bin/aide.wrapper --config /etc/aide/aide.conf --check).

<img width="959" height="599" alt="19522 AIDE remediation" src="https://github.com/user-attachments/assets/7b8f2f8c-bc97-45a5-b54b-19bc7c5ec332" />

<img width="605" height="455" alt="19522 AIDE pass" src="https://github.com/user-attachments/assets/41d3af41-43e3-46b0-a4b8-6e19090c4d6d" />

<img width="611" height="502" alt="19523 AIDE pass" src="https://github.com/user-attachments/assets/ec9c907b-32fb-49a2-9349-c40ae71cfd18" />

- **Finding 3- 19579: Ensure telnet client is not installed**

- **Status:** Remediated ✅
  
- **Issue:** The system is running the Telnet protocol, which is an insecure and unencrypted protocol.

- **Action Taken:** Uninstalled Telnet using apt-get remove Telnet on Virtual Machine.

<img width="617" height="503" alt="19579 Telnet failed" src="https://github.com/user-attachments/assets/85ce8acf-c10d-4413-9a39-508ddb4c9c2a" />

<img width="959" height="599" alt="19579 Telnet uninstalled" src="https://github.com/user-attachments/assets/db568513-03b6-4241-9f2d-afc695a3c6dd" />

<img width="621" height="502" alt="19579 Telnet pass" src="https://github.com/user-attachments/assets/e6a3becd-448a-49d5-b35c-d4f77b813bb4" />


- **Finding 4- 19582 & 19583: Ensure ICMP redirect message sending is disabled & Ensure source routed packets are not accepted**

- **Status:** Remediated ✅

- **Issue:** The system has souce routing and ICMP redirect messages enable. If source routing is not disabled an attacker can write their own path inside a packet (using a spoofed source address) and send it toward a private address. If ICMP redirect is not disabled an attacker can send out malicious ICMP redirect messages to devices on a network, tricking them into rerouting their traffic through a path the attacker controls.

- **Action Taken:** Disabled source routing (net.ipv4.conf.all.accept_source_route = 0, net.ipv4.conf.default.accept_source_route = 0) and disabled ICMP redirect sending (net.ipv4.conf.all.send_redirects = 0) — by adding both parameters to /etc/sysctl.conf, applying them immediately via sysctl -w, and flushing the routing table with sysctl -w net.ipv4.route.flush=1.

  <img width="623" height="509" alt="19582   19583 fail" src="https://github.com/user-attachments/assets/9d50d3c7-4684-4d73-b9f7-cb92da97bc61" />

<img width="959" height="599" alt="19582   19583 remediation" src="https://github.com/user-attachments/assets/bd18d18b-4e17-4e6c-9551-909e57a02f51" />

<img width="610" height="479" alt="19582   19583 pass" src="https://github.com/user-attachments/assets/4f39771b-2dac-4858-bee9-da007521e7a4" />

- **Finding 5- Ensure suspicios packets are logged**

- **Status:** Remediated ✅

- **Issue:** The system does not have this feature enabled. Without (log_martians) active, the system would not be able to log any evidence of unroutable or spoofed source addresses. This would leave no evidence for an administrator to investigate if an attacker was trying to probe the system.

- **Action Taken:**










