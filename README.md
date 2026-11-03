# Home SOC Lab — Attack Simulation & Detection Analysis
 
A self-built Security Operations Center (SOC) lab simulating real attacker behavior against a vulnerable target, with detection analyzed through log forwarding and Splunk. Built as a hands-on substitute for professional SOC experience, focused on understanding *where* logging pipelines provide visibility — and where they don't.
 
---
## Architecture
 
| Component | Details |
|---|---|
| **Attacker / SIEM host** | Kali Linux (`192.168.127.128`) — runs attack tooling (Nmap, Hydra) and Splunk Enterprise |
| **Target** | Metasploitable2 (`192.168.127.130`) — intentionally vulnerable Linux VM |
| **Network** | VMware Host-only subnet, isolated from external traffic |
| **Log transport** | `sysklogd` on Metasploitable2 forwarding over UDP 514 to Splunk |
| **Web app target** | DVWA (Damn Vulnerable Web Application) running on Apache, hosted on Metasploitable2 |
 
![Architecture diagram / network overview](screenshots/architecture.png)
 
### Key infrastructure note
Metasploitable2 uses **`sysklogd`**, not `rsyslog` — an important distinction, since `rsyslog.conf` is never read on this system. All forwarding configuration lives in `/etc/syslog.conf`.
 
```
# /etc/syslog.conf — forwarding rule added
*.*     @192.168.127.129
```
 
![syslog.conf forwarding rule](screenshots/syslog-conf.png)
 
---
