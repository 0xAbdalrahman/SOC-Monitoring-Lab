# Splunk Lab Environment


## Description
I built a SOC Monitoring Lab, a small virtual SOC on VirtualBox. I deployed Ubuntu Server (Splunk), Windows Server with a domain controller, Windows 10 as a target machine, and Kali Linux for attacking purposes.

I configured Splunk and Sysmon, forwarded Windows event logs to the indexer, executed controlled attacks from Kali, and created Splunk dashboards, real-time alerts, and scheduled reports to detect, monitor, and investigate suspicious activity.
<br></br>

## Objective
- The objective of this lab is to provide a hands-on environment for practicing cybersecurity. Using VirtualBox, it runs Windows 10, Kali Linux, Windows Server, and Ubuntu Server to cover skills like network setup, endpoint monitoring, and security tool deployment with Splunk and Sysmon. 

- The lab includes offensive testing with Crowbar and Active Directory integration. On the defensive side, it focuses on installing and configuring Splunk, forwarding logs, creating dashboards, building reports, and setting up real-time alerts. 
<br></br>

## Skills Learned

#### Active Directory & Windows Security
- Configure and manage AD domains, organizational units (OUs), and user accounts.
#### Splunk SIEM
- Install, configure, and use Splunk for log collection, indexing, and searching.
#### Network Administration
- Configure IP addresses and troubleshoot connectivity issues (ping, DNS, RDP).
#### Sigma Rules
- Write Sigma rules for SIEM-agnostic detection of malicious patterns.
#### Log Analysis
- Interpret Windows and network logs to detect suspicious activity and incidents.
#### Problem-Solving
- Develop hands-on troubleshooting and investigative skills for security monitoring and incident response.
<br></br>

## Tools Used

#### Windows Server & Windows 10
- Active Directory Domain Controller and client machine.
#### Ubuntu Server
- Hosting Splunk for log collection and analysis.
#### Splunk
- SIEM platform for log ingestion and searching.
#### Kali Linux
- Attacker machine for simulating attacks.
#### Sysmon
- Collecting and analyzing Windows security logs.
#### Sigma
- Generic detection rule framework for defining attack logic.
<br></br>

## Part 1 – VM Installation

#### 1. Install Oracle VM VirtualBox
- Go to [virtualbox.org](https://www.virtualbox.org/)
- Download the latest version for your OS and complete the installation.

#### 2. Install Windows Server 2022
- Download the ISO from https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022
- In VirtualBox, create a new VM based on your computer resources.

#### 3. Install Ubuntu Server (Splunk Host)
- Download Ubuntu Server 22.04 LTS from https://ubuntu.com/server
- In VirtualBox, create a new VM based on your computer resources.

#### 4. Install Windows 10
- Download the Windows 10 ISO https://www.microsoft.com/en-ca/software-download/windows10
- In VirtualBox, create a new VM based on your computer resources.

#### 5. Install Kali Linux
- Download the VirtualBox image from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines)
- In VirtualBox, create a new VM based on your computer resources.
<br></br>


