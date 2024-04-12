# Active-Directory
Basic Home Lab Running Active Directory (Oracle VirtualBox) 


<h2>Description</h2>
The project consists of a simple Active Directory home lab environment using Oracle Virtual Box. Server 2019 ISO and Windows 10 ISO are used to virtually run two separate machines using Oracle Virtual Box. Our first virtual machine will be installed using Server 2019 and will act as our domain controller, housing our Active Directory. This machine will be equipped with two network adapters: one to connect to the outside internet and the other to connect to the Virtual Box private network, with an assigned IP address. A PowerShell script is utilized to create over 1000 users in the private Virtual Box. This lab simulates a corporate office setup, where there is a main computer acting as the domain controller. Employees can then use their own computers to connect to the domain controller using their username and password, and access the internet through the private IP address and gateway.
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Oracle VirtualBox</b>


<h2>Environments Used </h2>

- <b>Windows 10 ISO</b> (21H2)
- <b>Server 2019 ISO</b>


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
