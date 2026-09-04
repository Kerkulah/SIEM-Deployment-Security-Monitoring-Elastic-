<h1> Wazuh Security Monitoring
</h1>



<h2>Lab Environment Overview</h2>
This lab demonstrates how attackers perform password based SSH attacks using tools like Hydra and how security teams can detect and respond to these attacks using a Security Information and Event Management (SIEM) system. The environment consists of three core components working together to simulate a complete attack and defend scenario.
<br />
<br />

<br /> Active Agents: <br />
<img src="https://imgur.com/U4Ry2nZ.jpg"  height="80%" width="80%">
<br /> Agent 001 (Kali) - Kali GNU/Linux 2025.1 - Attacker machine <br />
<br />Agent 004 (Ubuntu-Desktop) - Ubuntu 24.04.3 LTS - Target/Victim machine<br />
<br />Cluster: node01<br />
<br />Version: v4.14.2<br />


<br  />
<br />

<br />
<h2> Tech Stack</h2>

Proxmox VE : https://www.virtualbox.org/wiki/Downloads](https://www.proxmox.com/en/downloads
<br />
<br />
Kali Linux :                             https://www.kali.org/get-kali/#kali-installer-images
<br />
<br />
Ubuntu Desktop :                                https://ubuntu.com/tutorials/how-to-run-ubuntu-desktop-on-a-virtual-machine-using-virtualbox#1-overview

Wazuh :                                https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
<br />


Confirm SSH Log Collection
<h2> Brute Force Detection</h2>

<br />Confirm SSH Log Collection on both systems to READ(sudo cat /var/ossec/etc/ossec.conf) if not present WRITE(sudo nano /var/ossec/etc/ossec.conf) <br />
<img src="https://imgur.com/6vrJI20.jpg"  height="80%" width="80%">
<br />
<br />
<br />
What this demonstrates:
- Hydra was launched against the target's SSH service using a known username (Labuser) and the rockyou.txt wordlist, generating high volume authentication attempts (150–190 tries/min, 10 parallel tasks) to simulate a realistic credential stuffing/brute force attack.

- The Wazuh agent on the target actively tails /var/log/auth.log (confirmed in the ossec.conf excerpt), capturing each failed SSH authentication event in real time.

- These events feed into the Wazuh Manager, where the base rule (SID 5716, "sshd authentication failed") is correlated by custom rule 100010, which fires when 8+ failures occur from the same source IP within a 120 second window.

<br />

<img src="https://imgur.com/2WK8wkr.jpg"  height="80%" width="80%">

<br />

What this screenshot confirms:

- 168 hits in a 30 second window that's real, high volume failed login data actually landing in wazuh-alerts-*, not a config or ingestion problem.

- Every visible document shows rule.description: sshd: authentication failed with rule.id: 5760, data.srcip: 10.10.1.50 (my Kali attacker), data.dstuser: Labuser, data.srcport incrementing per attempt, and agent.name: Ubuntu-Desktop (agent.id: 005), this is the base SSH auth failure event exactly as expected, one per.

- The full log field even shows raw sshd output like Failed password for Labuser from 10.10.1.50 port 38024 ssh2  which is a solid forensic level detail

<br />

<img src="https://imgur.com/1wS8OmZ.jpg"  height="80%" width="80%">


<br />  


What this screenshot confirms:

- 168 hits in a 30 second window that's real, high volume failed login data actually landing in wazuh-alerts-*, not a config or ingestion problem.

- Every visible document shows rule.description: sshd: authentication failed with rule.id: 5760, data.srcip: 10.10.1.50 (my Kali attacker), data.dstuser: Labuser, data.srcport incrementing per attempt, and agent.name: Ubuntu-Desktop (agent.id: 005), this is the base SSH auth failure event exactly as expected, one per.

- The full log field even shows raw sshd output like Failed password for Labuser from 10.10.1.50 port 38024 ssh2  which is a solid forensic level detail

<br />

<img src="https://imgur.com/1wS8OmZ.jpg"  height="80%" width="80%">
<br />- />

<br />
<br />

<p align="center">
<br/>


Verify registration
