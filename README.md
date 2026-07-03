# soc-homelab
This homelab aims to emulate a Security Operations Center using the various tools an analyst would use in day-to-day life. 

## Hardware
- Dell Latitude 3310 (Isolator)
  - intel i5 8365u, 16gb ram, 128gb ssd
- Dell Precision Tower 3430 SSF (Main)
  - intel i7 8700, 64gb ram, 1tb NVME + 256gb SATA
 
## Tools
- pfSense - An open source, free firewall/router. Stationed on the laptop, it allows for me to place my tower on an isolated LAN. In combination with extensive rules, I can effectively isolate vulnerable VMs to ensure any malware or attacks do not reach my home network. 
- Proxmox - Open-source hypervisor that allows for virtualization. Used on both my laptop and tower. Hosts the VMs that I use for security and testing.
- Splunk - Enterprise-grade Security Information and Event Management (SIEM) used for indexing and searching logs. Stationed on the tower, it receives data from vulnerable VMs via the Splunk Universal Forwarder. Dashboards are created to visualize the most important and relevent data. Searching is used to further investigate specific events. Alerts are configured to trigger with malicious attacks.
- TailScale - Peer-To-Peer VPN that allows for secure, remote access. Installed on devices on my home network and lab network. Subnet Routing allows for home network devices to access LAN without port forwarding.
- Kali Linux - Open-source operating system with countless pentesting tools. Used in a VM to start attacks on other vulnerable VMs
- Metasploitable - Intentionally vulnerable VM that can be exploited via the Metasploit tools in Kali
- Windows 2022 Server - Enterprise-grade server meant to uphold the IT systems of entire corporations. In my lab, I have it in a VM with Active Directory enabled. This allows for a more realistic target to be exploited and protected
- Windows 11 - Modern operating system that is used by countless computers around the world. I use it as another target VM. With Active Directory enabled on the 2022 server, i can add a Windows 11 user under that domain. When combined with a traditional Windows user, this allows for a wide range of exploits to be conducted
- Suricata - Open-source Intrusion Detection System/Intrusion Prevention System (IDS/IPS). Installed on the tower proxmox node itself, it monitors my lab's network for any suspicious activity and forwards the logs to Splunk. Primarily set as an IDS, but will be moved into IPS to show attack repsonse.
- Sysmon - Free Microsoft tool used to log system activity at greater lengths than normally provided. Installed on my Windows Target VMS to have a better understanding of how attacks look as they happen.
