# Building Home Cyber Lab with VirtualBox
## Introduction:
I’ve always wanted my own little digital playground where I could tinker and safely break things, hack things, and learn how to fix them again — without my neighbors calling the FBI.

This project involves building a virtual cybersecurity home lab using Oracle VirtualBox, directly on my PC in Dallas, TX.
### Objective:
Create a simple environment with:
* Windows 11 client
* Ubuntu Server
* pfSense firewall for traffic control
* Kali Linux as an attacker
* Windows Server 2022 for Active Directory

This setup will let me practice networking, system administration, and eventually security testing.

<br><br>

## Step 1: Planning the Lab
Before spinning up VMs like a caffeinated sysadmin, I drew out my plan:

![Network Diagram](images/network.png)

A simple diagram showing pfSense connected to both the “NAT (WAN/Internet)” and an “Internal Network: Cyber (LAN).”
The Windows 11, Ubuntu, and Windows Server 2022 VMs reside within the internal network labeled “Cyber”.

### Network Plan:

|**Device**	| **OS** | **IP Address** | **Role** |
| ----- | -- | ---------- | ---- |
| pfSense | FreeBSD | 10.0.0.1 | Firewall/Gateway |
| Windows 11 | Win 11 | 10.0.0.10 |	Client/Workstation|
| Windows Server | Window Server 2022 |	10.0.0.40 |	Active Directory |
| Ubuntu | Ubuntu 24.04 LTS | 10.0.0.20 | Internal Server |
| Kali | Kali Linux 2025.3 | 10.0.0.30 | Attacker |

<br><br>			

## Step 2: Setting Up VirtualBox
I downloaded **VirtualBox 7.2.4** and the guest ISOs.

**VM Specs (for my machine):**
* CPU: AMD Ryzen 9 9950X
* RAM: 64 GB DDR5-6000
* Storage: 4 TB NVMe (RAID-5, because I like my bits safe)

## Creating the VMs
Here are the screenshots of how each VM was set up before installing the OS.

### Windows 11:

Name: “Windows 11 Lab” | System: 4096 MB RAM, 2 Processors, 80 GB, and network set to “Internal Network: Cyber”

![Windows 11](images/windows11.png) 

### Ubuntu 24.04 LTS:

Name: “Ubuntu Lab” | System: 2048 MB, 2 Processors, 25 GB, and network set to “Internal Network:  Cyber”
 
![Ubuntu](images/ubuntu.png) 

### Kali Linux 2025.3:

Name: “Kali Linux Lab” | System: 4096 MB, 2 Processors, 25 GB, and network set to “Internal Network:  Cyber”

![Kali Linux](images/kali.png)  

### Windows Server 2022:

Name: “Windows Server 2022” | System: 4096 MB, 2 Processors, 50 GB, and network set to “Internal Network:  Cyber”

![Windows Server](images/windowserver.png)  

### Netgate pfSense:

Name: “pfSense-Firewall” | System: 2048 MB, 20 GB, and network set to “External: NAT” & “Internal Network:  Cyber”

![pfSense](images/pfSense.png)  


### Configuring Network:

Adapter 1: Host adapter (for Internet access)

![Network Configuration](images/networkconfig.png)  

<br><br>

## Step 3: Installing the OS
Installation on the systems is straightforward, similar to installing any other operating system. There are no special tricks or requirements unless you want to set up Windows without signing in; then, check this out. How to Install and Log in to Windows 11 Without a Microsoft Account

### <u>Windows 11:</u>
 
The Windows installer is showing “This might take a few minutes.”

![Windows 11](images/windowsinstaller.png) 	 

Windows 11 is fully installed. (Notice the window header on top and the system tray below)

![Windows 11](images/windowsfull.png) 

### <u>Ubuntu Lab:</u>
 
Ubuntu Lab is up and running after a full upgrade and update.

![Ubuntu](images/ubuntufull.png) 

### <u>Kali Linux Lab:</u>
 
This Kali Link VM will be used as an attacker in future projects.

 ![Kali Linux](images/kalifull.png) 

### <u>Windows Server 2022:</u>
 
Installed Windows Server 2022 Standard Evaluation.

 ![Windows Server 2022](images/serverinstall.png) 

I will set this up in a later project for AD attacks.

 ![Windows Server 2022](images/serverfull.png) 

### <u>pfSense-Firewall:</u>
 
Here is pfSense after installation. You can see WAN was set to 10.0.2.15/24 and LAN to 10.0.0.1/24

 ![pfSense](images/pfsensefull.png)

<br>

### One additional thing I would recommend is installing the VboxGuestAddition.iso so your screen can be resized. It helped me with an ultra-wide screen, and it was super easy to do. On the VirtualBox menu, go to Devices > Insert Guest Additions CD image.. From there, follow the onscreen instructions and reboot the VM. Once the system comes back online, you will be able to resize your screen, and the GUI will adapt

<br>

![Guest Addition](images/guestaddition.png)
 
<br><br>

##  Step 4: Network Testing
Now the fun part — making the machines talk.
1.	Checked IPs
* On Windows: ipconfig
* On Ubuntu: ip addr show
2.	Pinged between them:
* Pinging 10.0.0.20 with 32 bytes of data: Reply from 10.0.0.20 time<1ms. Everything was connected through pfSense — mission success!

![Bidirectional](images/bidirectional%20ping.png)

<br><br>

## Step 5: Experimenting
To prove everything worked, I tested a few things:
* From Windows → SSH into Ubuntu: ssh boston@10.0.0.20

![SSH](/images/ssh.png)

* From Ubuntu → Ping 8.8.8.8 to confirm Internet through pfSense

![Ping 8.8.8.8](images/ping.png)

<br><br>

## Step 6: Reflection

### This first lab taught me:
* VirtualBox networking modes are key to any virtual setup.
* pfSense is both awesome and intimidating at first glance.
* Documentation (and screenshots) are your best friend for recreating labs later.

### Next Steps:
* Permote windows server to a domain controler and join Windows 11 to the domain.
* Practice DHCP, DNS, and Group Policy
* Simulate attacks with Kali Linux in a safe subnet

