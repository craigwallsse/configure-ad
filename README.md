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

- Set up resources 
- Ensure connectivity between the client and Domain Controller 
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Creat additional user (script) and attempt to log into client-1 with one of the users 

<h2>Deployment and Configuration Steps</h2>

<h3>Setup Resources in Azure</h3>
<p>
<img src="https://i.imgur.com/zty4oHY.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
<img src="https://i.imgur.com/tzjjqp3.jpeg" height="80%" width="80%" alt="Domain Controller VM"/>
</p>
<p>
First I created a Domain Controller VM with Windows Server 2022 and named it "DC-1". During this process, I took note of the Resource Group and Virtual Network (Vnet) that were created. I then set the Domain Controller's NIC Private IP address to be static. Next, I created a Client VM with Windows 10 and named it "Client-1", ensuring it used the same Resource Group and Vnet as "DC-1". 
</p>
<br />

<h3>Ensure Connectivity between the client and Domain Controller</h3>
<p>
<img src="https://i.imgur.com/QxoDn64.jpeg" height="80%" width="80%" alt="Enable ICMP echo request"/>
</p>
<p>
<img src="https://i.imgur.com/NSB1HnV.jpeg" height="80%" width="80%" alt="CLI ping request"/>
</p>
<p>
I logged into Client-1 using Remote Desktop and initiated a perpetual ping to DC-1's private IP address using the command ping -t <ip address>. Then, I logged into the Domain Controller and enabled ICMPv4 on the local Windows Firewall. After doing this, I checked back at Client-1 and saw that the ping was successful.
</p>
<br />

<h3>Install Active Directory</h3>
<p>
<img src="https://i.imgur.com/ALxQCLm.jpeg" height="80%" width="80%" alt="AD configuration wizard"/>
</p>
<p>
<img src="https://i.imgur.com/j01wWrG.jpeg" height="80%" width="80%" alt="Login page for labuser3"/>
</p>
<p>
I logged into DC-1 and installed Active Directory Domain Services. Then, I pointed the server to a Domain Controller and set up a new forest with the domain name craigsdomain.com. After the installation, I restarted the server and logged back into DC-1 as the user craigsdomain.com\labuser3.
</p>
<br />

<h3>Create an Admin and Normal User Account in AD</h3>
<p>
<img src="https://i.imgur.com/zwfHnPX.jpeg" height="80%" width="80%" alt="Logging in as craig_admin"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), I created an Organizational Unit (OU) called "_EMPLOYEES" and another OU named "_ADMINS". I then created a new employee named "Craig Walls" with the username "craig_admin" and the same password. I added "craig_admin" to the "Domain Admins" Security Group. After logging out and closing the Remote Desktop connection to DC-1, I logged back in as "craigsdomain.com\craig_admin" and used "craig_admin" as my admin account 
</p>
<br />

<h3>Join Client-1 to your domain</h3>
<p>
<img src="https://i.imgur.com/83c4twt.jpeg" alt="Client-1 in active directory domain"/>
</p>
<p>
<img src="https://i.imgur.com/Kwfzofe.jpeg" alt="Clients OU in active directory"/>
</p>
<p>
From the Azure Portal, I set Client-1's DNS settings to the DC's private IP address and then restarted Client-1. I logged into Client-1 via Remote Desktop as the original local admin (labuser) and joined it to the domain, which caused the computer to restart. Next, I logged into the Domain Controller via Remote Desktop and verified that Client-1 showed up in Active Directory Users and Computers (ADUC) inside the "Computers" container at the root of the domain. For organizational purposes, I created a new OU named "_CLIENTS" and moved Client-1 into it.
</p>
<br />

<h3>Setup Remote Desktop for non-administrive users on Client-1</h3>
<p>
<img src="https://i.imgur.com/OIqnbf4.jpeg" alt="Logging in as admin user"/>
</p>
<p>
I logged into Client-1 as craigsdomain.com\craig_admin and opened the system properties. I clicked on "Remote Desktop" and allowed "domain users" access to remote desktop. This configuration allowed me to log into Client-1 as a normal, non-administrative user. This part of the lab could also be done with Group Policy.
</p>
<br />

<h3>Created additional users using powershell script and attempt to log into client-1 with one of the users</h3>
<p>
<img src="https://i.imgur.com/JPQxMIL.jpeg" alt="script running in powershell ISE"/>
</p>
<p>
<img src="https://i.imgur.com/ZQSIV8Y.jpeg" alt="Users being generated in the employees OU"/>
</p>
<p>
I logged into DC-1 as jane_admin and opened PowerShell_ise as an administrator. I created a new file and pasted the contents of the script into it. Then, I ran the script and observed the accounts being created. After the script finished, I opened ADUC and observed the accounts in the appropriate OU. Finally, I attempted to log into Client-1 with one of the accounts, taking note of the password in the script.
</p>
<br />

<h3>Unlocking Accounts and Reseting Passwords</h3>
<p>
<img src="https://i.imgur.com/R5yP8PB.jpeg" alt="User entered the worng password"/>
</p>
<p>
<img src="https://i.imgur.com/oq3PRSe.jpeg" alt="Unlocked account in active directory"/>
</p>
<p>
<img src="https://i.imgur.com/r980mCp.jpeg" alt="Reset password in active directory"/>
</p>
<p>
In order to practice helping users who experienced getting locked out of their account, I used client-1 to enter the wrong password repeadetly until it trigged a locked account. I identified the affected user account in Active Directory Users and Computers (ADUC). Next, I right-clicked on the user account and selected "Properties." In the "Account" tab, I unchecked the "Account is locked out" option to unlock the account. To reset the password, I right clicked on the user name in the employees OU and clicked on the "Reset Password" button, following the prompts to set a new password. 
<br />

