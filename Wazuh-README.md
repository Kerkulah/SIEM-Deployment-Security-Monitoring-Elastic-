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
<br />
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
<h2> Brute Force Detection</h2>
<br />
Using Hydra Simulate SSH brute force attack and detect it with Wazuh.
<br />
<br />
<br /> -Install HYDRA on my Kali system <br />
<br />-Created a test user with weak password />
<br />- Created  a small password list<br
                       
<br />
<img src="https://imgur.com/iNvGZFt.jpg"  height="80%" width="80%">
<img src="https://imgur.com/E4oSiiq.jpg"  height="80%" width="80%">
<img src="https://imgur.com/K7pRmGW.jpg"  height="80%" width="80%">
<img src="https://imgur.com/9OeQghj.jpg"  height="80%" width="80%">
<br />
<br />

<br />On Wazuh Dashboard, filter for Multiple authentication failures and check for Possible SSH brute force attack. <br />
<br />
<br />
- Multiple authentication failures (rule.id:5710)
<br />
<img src="https://imgur.com/AItOkyC.jpg"  height="80%" width="80%">

<img src="https://imgur.com/JjpuPrc.jpg"  height="80%" width="80%">
<br />
<br />
- Possible SSH brute force attack (rule.id:5712)
<br />
<br />
<img src="https://imgur.com/WX8VHs6.jpg"  height="80%" width="80%">
<img src="https://imgur.com/iPoux4P.jpg"  height="80%" width="80%">
<br />
<br />
<br /> - Verifying SSH Brute Force Attack on Ubuntu System<br />
<br />
<img src="https://imgur.com/EWG1Y9b.jpg"  height="80%" width="80%">
<img src="https://imgur.com/KHK0uz9.jpg"  height="80%" width="80%">

<br />
<br />

<p align="center">
<br/>


