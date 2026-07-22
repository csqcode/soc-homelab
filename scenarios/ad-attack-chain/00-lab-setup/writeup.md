## Domain & Host Inventory
- Domain: LAB.LOCAL
- DC: Windows 2022 Server
- Workstation: Windows 11
- Attacker: Kali Linux
- Subnet: 10.10.10.0/24
- Key Accounts and their roles
  - j.admin (Domain Admin)
  - svc_sql (domain user, SPN, local admin on Windows 11)
  - jcyber (Nonprivileged user on Windows 11)

## Detection Stack Baseline
- Suricata on Porxmox host, logging to eve.json
- Splunk Free (500MB/day limit) with cron scheduling used for alerts
- Sysmon + Universal Forwarder used on Windows 11 and DC. SwiftOnSecuirty baseline config
  - Key IDs collected: 1, 3, 4, 10, 11
- Windows Event Logs: Security, System, Application
- GPO Audit subcategories enabled
  - Key IDs collected: 4768, 4769, 4624, 4625, 4720, 4738, 4732, 4688, 4719, 4103, 4104, 4698, 4697
 
## Deliberate Control Regressions
| Control | Change | GPO Location | Explanation |
| --- | --- | --- | --- |
| SMB Signing | Disabled | Computer Config --> Security Options | |
| RunAsPPL | 0 | Registry / Local Security Policy | |
| Credential Guard | Off | Computer Config --> Admin Templates | |
| WDigest | Enabled | Registry | |
| RC4 Kerberos | Permitted | Security Options --> Network Security: Configure Encryption Types | |
| Defender | Disabled | Admin Templates --> Windows Components --> Microsoft Defender Antivirus | |
