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

- Setup Domain Controller in Azure
- Deploy Active Directory
- Creating Users with Powershell
- Group Policy and Managing Accounts

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="1902" height="826" alt="image" src="https://github.com/user-attachments/assets/335118af-5bf1-4bb4-9855-190181d1d77e" />

<img width="951" height="917" alt="image" src="https://github.com/user-attachments/assets/7741f8a8-d3c0-4c85-a0b6-474cad4246e7" />

<img width="1377" height="1016" alt="image" src="https://github.com/user-attachments/assets/1d761a29-8dd8-459f-94b5-1bdf47477f31" />

<p>
Setup Domain Controller in Azure

Create a research group with a virtual network and subnet.
Create the Domain Controller VM (Windows Server 2022) named DC-1 with a username and password.
Set Domain Controller's NIC Private IP address to be static
Log into the VM and disable the Windows Firewall with command wf.msc (right click windows icon and press run)

Setup Client-1 in Azure

Create Client VM (Windows 10) named "Client-1" with username and password
Attach it to the same reason and Virtual Netwrok as DC-1
Set Client-1's DNS settings to DC-1's Private IP address
Restart Client-1 from Azure Portal and then login to Client-1 
Attempt to ping DC-1's private IP address - make sure that the ping was successful
From Client-1, open Powershell and run ipconfig /all
the output for the DNS should show DC-1's IP Address<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Install Active Directory

Log into DC-1 and install Active Directory Domain Services
Promote as DC: Setup a new forest as mydomain.com (Can be anything just remember what was named)
Restart and then log back into DC-1 as user: mydomain.com\labuser

Create a Domain Admin user within the domain

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called "_EMPLOYEES" Create a new OU named "ADMINS"
Create a new employee "Jane Doe" (same password) with username (jane_admin)
Add jane_adminto the "Domain Admins" Security Group
Log out/close the connection to DC-1 and log back in as "mydomain.com\jane_admin"
User jane_admin as admin account from now on

Join Client-1 to domain (mydomain.com)

Login to Client-1 as the original local admin and join it to the domain (computer will restart)
Login to the Domain Controller and verify Client-1 shows up in ADUC
Create a new OU named "_CLIENTS" and drag Client-1 into there

Setup Remote Desktop for non-administrative users on Client-1

Log into Client-1 as mydomain.com\jane_admin
Open system properties
Click "Remote Desktop"
Allow "domain users" access to remote desktop
Log into Client-1 as a normal, non-administrative user now
This previous step would most likely be done with Group Policy which allows admin to change many systems at once
<br />
