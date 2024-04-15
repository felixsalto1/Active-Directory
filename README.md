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
 <img src="https://i.imgur.com/NRgx1ue.png" >
 
- Open Oracle VirtualBox and click "New."
- Name the first machine as "DC" for Domain Controller and select the Server 2019 ISO image. Check "skip unattended installation" as we will complete this later once the OS is installed on our virtual machine.
- Assign at least 2 GB of RAM (2048 MB), Set between 1-4 CPU depending on the cores of your processor. If you are unsure leave it at 1. My processer has 8-cores and so I have this set at 4. Lastly, create a new virtual hard disk of at least 20 GB.

<img src="https://i.imgur.com/8HnUOZn.png" >

- Onced finished, go to the Settiing and under Network tab on the left window pane, Enable Network Adapter 2 attached to the Internal Network. 

  *Remeber we are creating our Domain Controller right now but we want to have 2 NICs. One dedicated for the internet that is going to be running NAT and we will have the second one that is dedicated for the internal VMWare network.
 
- Now its time to boot up our Domain Controller machine and set up the Server 19 operating system.

### Installing the Operating Systems and Initial Setup

Windows Server 2019 requires additional attention during setup to ensure proper administrative access. Here are the specifics:

- **Install Windows Server 2019:** Follow the on-screen instructions until you reach the account setup stage.
  
  <img src="https://i.imgur.com/MlpU7Os.png" >

- Make sure to select "Standard Evaluation (Desktop Expierence)" on the windows setup page. This option installs the full Windows GUI.

  <img src="https://i.imgur.com/saWUiJx.png" >

- Also make sure to select "Custom" install select Drive with Unallocated space (the 20GB we enabled earlier)
  
- **Create an Admin Account:** As part of the installation, you'll be prompted to create an administrator account. Here, I provided a username and a robust and unique password for enhanced security. This account will have elevated permissions, so it's crucial to protect it adequately.

 <img src="https://i.imgur.com/8fpjadL.png" >

 <img src="https://i.imgur.com/2fQzu22.png" >

### Laying Down the Operating Systems and Configuring NICs

- Now we have successfully installed our first vitual machine and Domain Controller with the Server 2019 operating systems. 
- In addition, for our Domain Controller, we configured the dual Network Interface Cards (NICs) for effective network management. As explained earlier, here is how we set them up
- NIC for Open Internet: The first NIC is configured to use Network Address Translation (NAT), enabling our Domain Controller to connect to the open Internet.
- NIC for Internal Network: The second NIC is allocated exclusively to our internal virtual network, ensuring isolated communication within our simulated environment.
- This dual NIC setup equips the Domain Controller with the network versatility needed for real-world applications, aligning perfectly with our lab's objectives.
- Doing so has established a secure yet flexible network topology indispensable for our Active Directory Lab.
- We will also configure this set up on our DC virtual machine by clicking on the Network icon on the toolbar. Then select "Network & Internet settings"
  <p align="left">  <img src="https://i.imgur.com/yFlztPG.png" >
 
- We will then be brought to a new window, look for the option to select "Change adapter options." You may see two Ethernet networks double click one and a small pop up window will show. Select "Details". Under this window you will see "Autoconfiguration IPv4.." with an IP address starting with 169, this is the internal network and we will rename it to "Internal"
- The second Ethernet option should show an IP address starting with 10, this we will name "Internet". This step is neccessary in order to set up the router in the later steps.
   <img src="https://i.imgur.com/dTA476a.png" >


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
