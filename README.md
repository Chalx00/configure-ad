<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources in Azure
- Install Active Directory
- Create an Admin and User Account
- Join a Client to the Domain
- Run Powershell script to create Users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/r5mzLLS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 1: In your Azure portal, go to vitrual machines and create a VM that will serve as the domain controller (Windows server 2022,size: standard ES_v3_v2cpu). Next, create a client to go along with the domain controller and ensure they both utilize the same resource group and Vnet. Then change the domain controller's private IP address from dynamic to static (Go back to DC VM >Networking (settings) >network Interface >IP configurations >hit dynamic >change to Static >Save)
</p>
<br />

<p>
<img src="https://i.imgur.com/AZMDXHA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2: once resources are set up,download microsoft remote desktop to access tge virtual machines created in azure. Copy the domain controller's public IP address into remote desktop, and start the VM ( you'll be prompted to sign in, use the credentials created while setting up the VM resources in step 1). Once in VM environment, the server manager opens up (Note: this is because you created this VM as a server in step 1) click "add roles and features" > 'next' through until you get to the page where you select features then select "active directory domain services" > click install. After installing, click the yellow warning flag on the top right corner of the server manager and select "Promote this server to a domain controller" (This action makes the VM a domain controller) > click new forest > name the domain > install. After this step, the VM will restart, log back in using new domain name after the restart process.
</p>
<br />

<p>
<img src="https://i.imgur.com/BqZXMZV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3: to create an admin and user account in AD, go to active directory users and computers > under the domain name we created in the previous step, create new OU (Organizational Units) by right clicking and selecting new > Organizational unit > Name unit. To create a new user, right click > New > User > Create an Admin User & make note of the login name. Make the user Admin: Right-click user > Properties > Member of > Add > enter “Domain” > check names > Domain Admins > ok > Apply >Ok. Click on new OU Employees > New > User > Create a User & make note of the login name. At CMD prompt > logoff, Logs off Log back in as the Administrator and confirm with CMD Prompt, whoami.
</p>
<br />

<p>
<img src="https://i.imgur.com/vPcozbB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: To join a client to a domain, you must use the Domain Controller’s DNS server instead of a regular DNS server. To join a domain, in Azure, get the Private IP address of the Domain Controller (Networking tab > NIC Private IP > Copy), Go to Client(the client you intend to join to the domain controller) RDP, Networking > click on network Interface > DNS Servers under settings > Change DNS Servers to custom > enter the NIC (Private IP copied from domain controller). Restart Client-1, copy Client-1 Public IP & re-launch RDP. Go to Client-1 system settings (right click Start menu >System >Settings >Rename this PC Advanced >In Computer name Tab, hit Change >Member of >Domain >enter domain name > enter credentials > close all tabs so it can restart. Copy the public IP again & re-launch RDP. Login with Admin credentials.
</p>
<br />

<p>
<img src="https://i.imgur.com/4Tb6nbX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 5: run powershell script to create additional users - the script command will create users in one of the OUsn(organizational units) created in step 3. Once logged in to the Domain Controller, Search > Powershell_ISE > right-click to run as Admin(you must run as admin otherwise the process will fail) > Create a new file > Paste the script contents > Run the script. Once complete, we can get the credentials of any of the created users and login as a way to familiarize ourself with the system.
</p>
