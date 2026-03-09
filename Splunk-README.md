<h1> Splunk Detection Engineering & SIEM Investigation Project</h1>



<h2>Description</h2>
Hands on detection engineering project focused on understanding Splunk from the ground up, including its architecture, data ingestion pipeline, and field extraction process. Used real world datasets to investigate security incidents, analyze adversary behaviors, and operationalize Splunk as a SIEM for threat detection and response. Developed high fidelity SPL detections both TTP driven and analytics driven to surface anomalies, abnormal patterns, and attacker techniques aligned with MITRE ATT&CK. Gained practical experience navigating fields, validating log sources, tuning queries, and creating precise, actionable detections. 

<h2> Quick walk through::</h2>
<br />
Splunk architecture & core components
<br />
SPL (Search Processing Language) detection creation
<br />
Incident investigation using real world data
<br />
TTP driven and anomaly based detection engineering
<br />
Analyzing ingested fields/log structures
<br />






<p align="center">
Intrusion Detection With Splunk  <br/>

Using self generated and dummy data, I retrieved all accessible events in Splunk by setting the time picker to All Time and running the query:
index="main" earliest=0
This query returns all events available within the specified index

<br />
<br />
<img src="https://imgur.com/prpIvlg.jpg"  height="80%" width="80%">
<br />
Exploring the Sysmon data begins with understanding the environment. Since we are approaching it as an unknown environment, the first step is to list all available sourcetypes using (index="main" | stats count by sourcetype) to identify what data sources are present. Query Sysmon sourcetype and take a look at the incoming data using (index="main" sourcetype="WinEventLog:Sysmon") <br/>
<img src="https://imgur.com/RVTGMiG.jpg"  height="80%" width="80%">
<br />
<br />

<p align="center">
Visit https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon to understand Sysmon event descriptions  <br/>

<br />
<br />
<img src="https://imgur.com/OWEfXXZ.jpg"  height="80%" width="80%">
<br />
<br />

Using these EventCodes, we can begin performing initial queries. Since unusual parent child process relationships often indicate suspicious activity, we’ll examine all parent child process trees with the following query. (index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 | stats count by ParentImage, Image)  <br/>
<img src="https://imgur.com/V07fC9L.jpg"  height="80%" width="80%">
<br />
<br />
  
<br />
The query returns 5,427 events far too many to review manually. At this stage, we can either filter out benign activity or focus on child processes commonly associated with suspicious behavior, such as cmd.exe and powershell.exe. We'll start by targeting those two by using the following query  index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 (Image="*cmd.exe" OR Image="*powershell.exe") | stats count by ParentImage, Image 
<br/>
<img src="https://imgur.com/yJlCwfn.jpg"  height="80%" width="80%">
<br />


The notepad.exe to powershell.exe process chain stands out immediately. This indicates that Notepad launched a child PowerShell process, which is highly unusual and often associated with suspicious or malicious activity. delve deeper by focusing solely on these events. index="main" sourcetype="WinEventLog:Sysmon" EventCode=1 (Image="*cmd.exe" OR Image="*powershell.exe") ParentImage="C:\\Windows\\System32\\notepad.exe" 
<br />
<br />
The ParentCommandLine shows Notepad running with no arguments, yet it spawns a PowerShell process whose CommandLine appears to download a file from the server at 10.0.0.229. This behavior is highly suspicious and indicative of potential malicious activity.
<br />
<img src="https://imgur.com/6Jo5rM8.jpg"  height="80%" width="80%">
<br />

<br />
<br />
After farther analysis, based on the data and the host parameter, we can conclude that this IP belongs to the host named waldo virtual machine on its ens160 interface. The IP seems to be doing some generic stuff. It’s becoming increasingly clear that the notepad.exe powershell.exe activity is not only malicious, but the Linux system involved also appears to be compromised. The host seems to be acting as a staging point for transmitting additional utilities, suggesting a broader infection.

<img src="https://imgur.com/bXX1Uxk.jpg"  height="80%" width="80%">
<br />
<br />
<img src="https://imgur.com/54emRht.jpg"  height="80%" width="80%">
<br />
<br />

<br />
<br />
<br />

Two hosts were impacted by this Linux pivot. Notably, evidence shows that a DCSync PowerShell script was executed on the second host, strongly indicating a likely DCSync attack

<br/>
<img src="https://imgur.com/dAw3np9.jpg"  height="80%" width="80%">
<br />
<img src="https://imgur.com/7iyeNkK.jpg"  height="80%" width="80%">
<br />
<br />



