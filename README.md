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
   <img src="https://i.imgur.com/CkHrYKI.png" >

## Integrating Active Directory Domain Services

- Upon booting up Windows Server, I ventured into the Server Manager, and from there, I meticulously added the "Active Directory Domain Services" role.

 <img src="https://i.imgur.com/vL9FnAl.png" >


## Crafting a Domain Admin Account

- Open Active Directory Users and Computers.
- Right-click and select New > User.
- Provide details for your new Domain Admin account and set a strong password.

## Set Up DHCP Server

- Back in the Server Manager, I incorporated the "DHCP Server" role, ensuring our client machine would receive an IP address dynamically.

## Structuring the Organizational Unit

- Open Active Directory Users and Computers.
- Right-click on your domain, go to New > Organizational Unit, and create an OU.

## Configure Routing and Remote Access

- Go to Server Manager and add the "Routing" role.
- Configure Routing and Remote Access to set up an internal network.

## Configure NIC for Internet Access

- Open Network and Sharing Center and go to "Change adapter settings."
- Configure the NIC to connect to the outside internet.

## Add Users via PowerShell

In an organizational setup, it's not uncommon to have hundreds, if not thousands, of users. Manually entering each would be an inefficient use of resources. That's why I've incorporated a PowerShell script for batch user creation. Let's dissect the script ```BulkAddUserScript.ps1``` to understand its structure and function:

### Define Variables:

    # ----- Edit these Variables for your own Use Case ----- #
    $PASSWORD_FOR_USERS   = "Password1"
    $USER_FIRST_LAST_LIST = Get-Content .\names.txt
    # ------------------------------------------------------ #

Here, two variables are being defined:

- ```$PASSWORD_FOR_USERS = "Password1":``` This variable sets a default password for all users. This script is set as "Password1." While setting a universal password isn't the best practice for production, it's suitable for this lab.
- ```$USER_FIRST_LAST_LIST = Get-Content .\names.txt:``` This line reads a file ```names.txt``` that contains a list of first and last names. Each name in this file is presumably formatted as "FirstName LastName", one pair per line.

### Convert Plain Text Password:

      $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

- For security reasons, PowerShell commands related to user management often require passwords to be in the form of a secure string rather than plain text. Here, we're converting our plain text password from the ```$PASSWORD_FOR_USERS``` variable into a secure string and storing it in the ```$password``` variable.

### Create Organizational Unit (OU):

    New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

- Here, an Organizational Unit (OU) named ```_USERS``` is being created. The ```-ProtectedFromAccidentalDeletion $false``` parameter ensures that the OU is not protected from accidental deletions. This is handy for a lab environment, but in a production scenario, you should consider protecting it.

### Loop Through Names and Create Users:

    foreach ($n in $USER_FIRST_LAST_LIST) {
    ...
    }

- For each name in our list ```($USER_FIRST_LAST_LIST)```, the following actions are performed.

### Extract the first and last names:

    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()

- The ```.Split(" ")``` method splits the full name into a first and last name based on the space between them. The ```.ToLower()``` method then converts these names to lowercase.

### Construct the username:
    
    $username = "$($first.Substring(0,1))$($last)".ToLower()
- This line constructs a username by taking the first letter of the first name and appending the full last name, all in lowercase. For example, for the name ```"John Doe"```, the username would be ```"jdoe"```.

### Display progress in the console:

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
- This line displays a message in the console to show which user is being created.

 <img src="https://i.imgur.com/sZ5REIT.png" >


### Create the new user:


    New-AdUser ...

- The ```New-AdUser``` cmdlet creates a new user in Active Directory. The script specifies various parameters such as the given name, surname, display name, etc., and places the user in the previously created ```_USERS OU```. The ```-PasswordNeverExpires $true``` parameter ensures that the user's password does not expire, which is fine for a lab environment. Still, you generally want to set an expiration in a live production environment.


### Why are we doing this?

The main goal of this script is to demonstrate automation capabilities in Active Directory management, especially when dealing with a large number of users. Rather than manually entering each user, which can be a time-consuming and error-prone process, this script streamlines and automates the task, ensuring consistency and efficiency. It showcases the power of PowerShell scripting in managing Windows environments and offers a practical example of its application in real-world scenarios.

### Client Interaction and User Experience

With our Windows 10 VM ready, I configured it to connect to our server's environment. Subsequently, I logged off and back in, simulating the experience of one of our batch-created users.

 <img src="https://i.imgur.com/CDD38VC.png" >


## Final Thoughts and Conclusions

### Achieved Objectives

Throughout this lab, we traversed the essentials of setting up and configuring a Windows-based Active Directory environment. The walkthrough guided us through everything from initial setup and domain configurations to intricate networking adjustments and automation. The PowerShell script's employment for bulk user creation stands as a testament to how scalable and automatable an Active Directory environment can be.

### Learning Outcomes

The lab offered an invaluable hands-on experience, helping to demystify the often complex domain of Windows networking and Active Directory services. It provided insights into foundational aspects like DHCP setup, user management, Organizational Unit (OU) creation, and networking nuances. Perhaps most notably, it demonstrated how automation could significantly enhance administrative efficiency, a key takeaway for any IT professional.

### Real-World Applicability

The skills and knowledge gained from this lab extend far beyond its experimental confines. These competencies are highly transferable to real-world scenarios, where Active Directory is a cornerstone for organizational IT infrastructures. The automation aspects, particularly, equip you with the capability to manage large-scale environments effectively.

### Future Exploration

Although this lab covers significant ground, it's worth noting that Active Directory and Windows networking encompass much more. Advanced topics like Group Policies, advanced security measures, multi-site configurations, and integration with cloud services are some areas where you can extend your learning journey.

### Closing Remarks

In conclusion, this lab is a stepping stone into Windows-based networking and Active Directory. It equips you with the technical know-how and instills a strategic understanding of these services' roles within larger IT ecosystems.

The complexity and richness of Active Directory can be intimidating, but as this lab has demonstrated, a structured and hands-on approach can go a long way in gaining mastery over it. For those who aim to either enter the IT field or enhance their existing skill set, understanding Active Directory is almost a rite of passage, and this lab seeks to make that transition as seamless as possible.



<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
