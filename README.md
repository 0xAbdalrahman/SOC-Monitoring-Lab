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

#### Install Kali Linux
- Download the VirtualBox image from [kali.org](https://www.kali.org/get-kali/#kali-virtual-machines)
- In VirtualBox, create a new VM based on your computer resources.
<br></br>

## Part 2 — Network Configration

#### 2.1 — Create NAT network and assign to VMs
- In VirtualBox: Tools → Network → NAT Networks → Create. Example IPv4 prefix: 192.168.10.0/24. Give the network a name and Apply.
- For each VM: Settings → Network → Attached to: NAT Network
- select SOC-NAT. Install Windows only (advanced), and complete setup.

#### 2.2 — Set static IP on Splunk (Ubuntu)
Open a terminal on the Ubuntu VM and edit netplan:
`sudo nano /etc/netplan/00-installer-config.yaml` replace or update the file to:
```
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [192.168.10.10/24]
      nameservers:
        addresses: [8.8.8.8]
      routes:
        - to: default
          via: 192.168.10.1
  version: 2
```
Apply the configuration and verify `sudo netplan apply`

#### 2.3 — Install Splunk on Ubuntu
- Download Splunk Enterprise .deb from splunk.com (transfer via shared folder or SCP).
- Install VirtualBox guest tools for shared folders `sudo apt-get install virtualbox-guest-utils`
- Mount shared folder (if used)
```
sudo mkdir /mnt/share
sudo mount -t vboxsf -o uid=1000,gid=1000 <shared-folder-name> /mnt/share
ls -la /mnt/share
```
- Install Splunk:
```
sudo dpkg -i /mnt/share/splunk-<version>.deb
sudo /opt/splunk/bin/splunk start --accept-license
sudo /opt/splunk/bin/splunk enable boot-start -user splunk
```
- In Splunk Web: Settings → Indexes → New Index → name: endpoint.
- Settings → Forwarding & receiving → Configure receiving → New Receiving Port → set port 9997.

#### 2.4 Configure Windows target 

- Rename PC (optional): Settings → About → Rename this PC → TARGET-PC → Restart.
- Set static IPv4: Network icon → Open Network & Internet Settings → Change adapter options → Right-click adapter → Properties → IPv4 → Use the following IP address
  - IP: 192.168.10.100
  - Subnet mask: 255.255.255.0
  - Default gateway: 192.168.10.1
  - Preferred DNS: 8.8.8.8
- Verify `ipconfig shows 192.168.10.100`
- From a Windows browser, visit `http://192.168.10.10:8000` to confirm Splunk is reachable.
- Install the sysmon app from Splunk apps installation.

#### 2.5 Configure Windows Server
- Rename the server: ADDC01 → Restart.
- Configure the domain `splunklab.local`:
    - Open Server Manager → Add Roles and Features.
    - Install Active Directory Domain Services (AD DS).
    - Promote server to Domain Controller → Select Add a new forest → Enter domain: `splunklab.local`.
    - Restart and log in as SPLUNKLAB\Administrator.
    - Verify in Active Directory Users and Computers (ADUC) that the domain `splunklab.local` exists.
- Set static IPv4 on the server’s adapter:
  - IP: 192.168.10.7
  - Subnet mask: 255.255.255.0
  - Default gateway: 192.168.10.1
  - Preferred DNS: 8.8.8.8
- Install Splunk Universal Forwarder on the server and point to 192.168.10.10:9997.
- Verify logs in Splunk: index=endpoint — you should see TARGET-PC and ADDC01 as hosts.

#### 2.6 Install Splunk Universal Forwarder and Sysmon
- Download Splunk Universal Forwarder MSI from splunk.com and install.
- Download Sysmon (Sysinternals) and a recommended config sysmonconfig.xml from sysmon-modular.
- In PowerShell (as Administrator), install Sysmon: `.\Sysmon64.exe -i .\sysmonconfig.xml`
- Configure inputs on the forwarder: create `C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf` with: 
```
[WinEventLog://Application]
index = endpoint
disabled = false

[WinEventLog://Security]
index = endpoint
disabled = false

[WinEventLog://System]
index = endpoint
disabled = false

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
index = endpoint
disabled = false
renderXml = true
source = XmlWinEventLog:Microsoft-Windows-Sysmon/Operational
```
- Restart the SplunkForwarder service (Services → SplunkForwarder → Restart).
- In Splunk Web, search index=endpoint to confirm logs arrive.
- Do these steps on Active Directory and the Windows 10 machines.

#### In the end 
- All VMs are attached to a NAT network (192.168.10.0/24).
- Ubuntu/Splunk is 192.168.10.10 and accepts forwarded data on port 9997.
- Windows target is 192.168.10.100; Windows Server domain controller is 192.168.10.7.
- Splunk receives Application, Security, System, and Sysmon event logs from both Windows hosts — verify with index=endpoint in Splunk Web.

