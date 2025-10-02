# AD-Security-Lab


## Objective
I will build a controlled virtual environment for hands-on cybersecurity learning. This lab will include multiple virtual machines (Windows 10, Kali Linux, Windows Server, and Ubuntu Server) configured on VirtualBox with proper networking. I will install and configure Splunk on Ubuntu Server and deploy Splunk Universal Forwarders with Sysmon on Windows endpoints to collect security telemetry.

I will set up Active Directory on Windows Server to manage a domain environment and enable automated tasks using PowerShell. From the offensive side, I will run basic brute-force tests with Crowbar from Kali against Windows targets.

On the defensive side, I will monitor and analyze logs in Splunk, where I will build dashboards to visualize authentication activity, configure alerts to detect brute-force attempts and suspicious logons, and generate scheduled reports to summarize activity and incidents.

By the end of this project, I will have an end-to-end lab that simulates both attack and defense, integrates reporting and alerting through Splunk, and strengthens my skills in virtualization, networking, endpoint monitoring, Active Directory, incident detection, and security automation.
<br></br>


## Skills Learned

##### Active Directory & Windows Security:
- Configured and managed AD domains and user accounts.
##### Splunk SIEM
- Installed, configured, and used Splunk for log collection and searching.
##### Log Analysis
- Analyzed and interpreted Windows and network logs to detect suspicious activity.
##### Attack Understanding
- Learned attack techniques (brute force and Pass-the-Hash) and how to identify their indicators in logs.
##### Sigma Rule Writing
- Created Sigma rules for generic, SIEM-agnostic detection of attacks.
##### Cybersecurity Problem-Solving 
- Developed critical thinking and hands-on problem-solving skills in security monitoring and investigation.
<br></br>

## Tools Used

##### Windows Server & Windows 10: 
- Active Directory Domain Controller and client machine.
##### Ubuntu Server
- Hosting Splunk for log collection and analysis.
##### Splunk
- SIEM platform for log ingestion and searching.
##### Kali Linux
- Attacker machine for simulating attacks.
##### Sysmon
- Collecting and analyzing Windows security logs.
##### Sigma
- Generic detection rule framework for defining attack logic.
##### Crowbar
- Brute force tool for RDP attacks.
##### Atomic Red Team
- Open-source library for testing MITRE ATT&CK techniques in a controlled environment.
<br></br>
