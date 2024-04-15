# Basic Home Lab Running Active Directory (Oracle VirtualBox) 

<br/>
<img src="https://i.imgur.com/HmATYuu.png" >
<br />

 
<h2>Description</h2>
The project consists of a simple Active Directory home lab environment using Oracle Virtual Box. Server 2019 ISO and Windows 10 ISO are used to virtually run two separate machines using Oracle Virtual Box. Our first virtual machine will be installed using Server 2019 and will act as our domain controller, housing our Active Directory. This machine will be equipped with two network adapters: one to connect to the outside internet and the other to connect to the Virtual Box private network, with an assigned IP address. A PowerShell script is utilized to create over 1000 users in the private Virtual Box. This lab simulates a corporate office setup, where there is a main computer acting as the domain controller. Employees can then use their own computers to connect to the domain controller using their username and password, and access the internet through the private IP address and gateway.
<br />

## Objectives

<br />
<img src="https://i.imgur.com/8kW79OB.png" >
<br />

In this lab, we will:

- Set up an Active Directory Domain Service starting from the ground up.
- Create a dedicated Domain Administrator account, to uphold security and oversight.
- Deploy a DHCP server for dynamic allocation of IP addresses.
- Illustrate the formation of an Organizational Unit for enhanced user administration and management
- Explore the complexities of Routing and Remote Access to replicate a corporate intranet.
- Configure Network Interface Card (NIC) settings to guarantee both local and internet connectivity.
- Demonstrate streamlined user management by utilizing PowerShell to collectively add over 1000 users.
- Engage with our client machine, replicating a login procedure with one of the recently added users.

## Technologies Used:

- Oracle VirtualBox
- ISO Files for Windows Server 2019 and Windows 10
- Powershell

## Installation

### Create Virtual Machines in VirtualBox

- Open Oracle VirtualBox and click "New."
- Name the first machine as "DC" for Domain Controller and select the Server 2019 ISO image. Check "skip unattended installation" as we will complete this later once the OS is installed on our virtual machine.
- Assign at least 2 GB of RAM (2048 MB), Set between 1-4 CPU depending on the cores of your processor. If you are unsure leave it at 1. My processer has 8-cores and so I have this set at 4. Lastly, create a new virtual hard disk of at least 20 GB.
- Onced finished, go to the Settiing and under Network tab on the left window pane, Enable Network Adapter 2 attached to the Internal Network. 

  *Remeber we are creating our Domain Controller right now but we want to have 2 NICs. One dedicated for the internet that is going to be running NAT and we will have the second one that is dedicated for the internal VMWare network.
 
- Now its time to boot up our DC machine and set up the Server 19 ISO.

### Installing the Operating Systems and Initial Setup

I initiated the installations by attaching the ISO files to their respective VMs. Windows Server 2019 required additional attention during setup to ensure proper administrative access. Here are the specifics:

- **Install Windows Server 2019:** Follow the on-screen instructions until you reach the account setup stage.
  
  ![IMG_1735](https://github.com/AmiliaSalva/ActiveDirectoryLab/assets/132176058/9ddc81f9-b45e-4b8c-ba31-9aa044cf92ba)
  
- **Create an Admin Account:** As part of the installation, you'll be prompted to create an administrator account. Here, I provided a username and a robust and unique password for enhanced security. This account will have elevated permissions, so it's crucial to protect it adequately.

  ![IMG_1737](https://github.com/AmiliaSalva/ActiveDirectoryLab/assets/132176058/67547c2b-797c-4ca2-b4d1-b1a71897ad13)

  ![IMG_1739](https://github.com/AmiliaSalva/ActiveDirectoryLab/assets/132176058/4d16f5ae-3b6c-4ca8-be5a-ff8476ba66b4)

### Laying Down the Operating Systems and Configuring NICs

I initiated the installation process after attaching the ISO files to their respective VMs. The on-screen instructions are generally straightforward, leading to a successful installation of both operating systems.

In addition, for our Domain Controller, it's imperative to configure dual Network Interface Cards (NICs) for effective network management. Here's how I set them up

- **NIC for Open Internet:** The first NIC is configured to use Network Address Translation (NAT), enabling our Domain Controller to connect to the open Internet.
  
- **NIC for Internal Network:** The second NIC is allocated exclusively to our internal virtual network, ensuring isolated communication within our simulated environment.

This dual NIC setup equips the Domain Controller with the network versatility needed for real-world applications, aligning perfectly with our lab's objectives. 

Doing so has established a secure yet flexible network topology indispensable for our Active Directory Lab.


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
