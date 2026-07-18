# soc-homelab
This homelab aims to emulate a Security Operations Center using realistic attack chains and building detections across Splunk and Suricata. 

## Hardware
- Dell Latitude 3310 (Isolator)
  - intel i5 8365u, 16gb ram, 128gb ssd
- Dell Precision Tower 3430 SSF (Main)
  - intel i7 8700, 64gb ram, 1tb NVME + 256gb SATA
 
## Tools
| Category | Tools |
|---|---|
| Firewall/Router | pfSense |
| Hypervisor | Proxmox |
| SIEM | Splunk |
| VPN | TailScale |
| IDS/IPS | Suricata |
| Attack Tools | Kali Linux |
| Domain | Windows Server 2022 AD |
| Target | Windows 11 Workstation |

## Design Philosophy
This lab was designed with a primary focus on Splunk analysis. Targets are weakened to provide an easier surface of attack. Rather than focusing on simulating as many attacks as possible, this lab simulates a few detailed attack chains in favor of in-depth analysis. 

## Scenarios
| Scenario | Techniques Covered | Detection Sources | Status | 
| --- | --- | --- | --- |
| [AD Attack Chain](
