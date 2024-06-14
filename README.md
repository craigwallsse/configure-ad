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


