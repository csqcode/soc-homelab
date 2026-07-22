## 01 - LLMNR Poisoning

**MITRE ATT&CK:** S0174 - Responder\
**Status:** Complete

---

## Objective

Responder is used to spoof a name resolution source, providing the NTLM hashes that allow for intial access to a system. In a real scenario, an attacker would use this to gain footing in an Active Directory.

---

## Environment / Prerequisites
- **Attacker:** Kali
- **Target:** Windows 11 Workstation
- **Starting Access:** None
- **Lab Weakening Required:** SMB Signing disabled

---

## Attack Execution
1. On Kali, start responder with poisoning
   - ```sudo responder -I [interface] -w -v```
2. On Windows 11, go to file explorer and search for a hostname that doesn't exist
   - ex. \\\NEEDFILES\
3. Enter credentials into Windows Security window
4. Look at Kali for username and hash
5. Use john or hashcat to crack
   - ```
        john hash.txt
        john --show hash.txt
      ```
---

## Explanation
- This technique is a type of Man-in-the-Middle attack. When you search for the nonexistent host, Windows checks various sources. It initially searches local files and DNS servers, but will fall back to LLMNR/NBT-NS if nothing is produced. Since we deliberaly searched for a false name, this fallback happens.
- Responder actively listens for LLMNR/NBT-NS on the specified interface. When it receives the request, it identifies itself as the host and establishes a connection with the victim. It then emulates an authentication service to prompt the victim to enter their credentials for harvesting.

---

## Detection Engineering

**Telemetry source:** Suricata

LLMNR Splunk: 
```spl
index=suricata alert.signature="LLMNR Response Packet Detected, Possible LLMNR Poisoning"
```
LLMNR Suricata:
```
alert udp any 5355 -> any any (msg:"LLMNR Response Packet Detected, Possible LLMNR Poisoning"; flow:stateless; content:"|80 00 00 01 00 01|"; sid:1000000; rev:1;)
```

NBT-NS Splunk:
```spl
index=suricata alert.signature="NBT-NS Response Packet Detected, Possible NBT-NS Poisoning"
```
NBT-NS Suricata:
```
alert udp any 137 -> any 137 (msg:"NBT-NS Response Packet Detected, Possible NBT-NS Poisoning"; flow:stateless; content:"|85 00 00 00 00 01|"; sid:1000001; rev:1;)
```

**Rationale:** The Suricata alerts check for specific byte sequences found in LLMNR/NBT-NS responses (These were taken from actual poisoned packets). It also only checks traffic that originates from the protocols' respective ports, further reducing the likelihood of a false positive.

---

## Investigation Walkthrough
- If I received this alert in a workspace, I would likely first check any relevent dashboard panels to gain a better understanding of the alert. Moving on to the actual alert, I would see if the packet was allowed/denied, the severity, as well as the signature. I would then look at the Destination IP, Interface, and Source IP to get a better idea of where the packet was sent and where it came from. I might look into the Destination and see if it was a random user, or someone with administrator privileges. I could then research the Source, their role, and if Suricata generated any matching alerts about a running Kali Linux instance (This was detected so much I had to suppress the alert).

---

## Detection Gaps / Limitations
- This methodology does not detect any traffic from the perspective of the Windows 11 system
- There is a likelihood of false positives as this is a legitimate process that Windows calls upon
- Attacker could evade by using a stricter form of poisoning (Responder uses bursts of LLMNR, NBT-NS, and mDNS)

---

## Real-World Mitigation
- Mandatory cyber training encourages employees not to freely give out credentials, which would hinder the primary goal of responder.
- Enable SMB Signing to fully prevent poisoning

---

## Takeaways
- I was shocked by how both easy and hard it was to get credentials. While Kali has to do virtually nothing, it needs to hope that somebody slips up and types their information into a random security window.
- I feel as though this step shows the importance of individual training. While security systems can be in place to drop these packets, any failure means that employees must be smart enough not to fall for this trap. An employee with insufficient training may accidentally give out their credentials thinking they've signed into a legitimate service.
- If I were to do this again, I would like to explore more of Responder's complexities. I would like to see more advanced ways of poisoning. I would also like to strengthen my alerts to reduce the likelihood of false positives.
