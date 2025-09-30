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

Setup Domain Controller in Azure<p>

<img width="1902" height="826" alt="image" src="https://github.com/user-attachments/assets/335118af-5bf1-4bb4-9855-190181d1d77e" />

<img width="951" height="917" alt="image" src="https://github.com/user-attachments/assets/7741f8a8-d3c0-4c85-a0b6-474cad4246e7" />

<img width="1377" height="1016" alt="image" src="https://github.com/user-attachments/assets/1d761a29-8dd8-459f-94b5-1bdf47477f31" />


Create a research group with a virtual network and subnet.
Create the Domain Controller VM (Windows Server 2022) named DC-1 with a username and password.
Set Domain Controller's NIC Private IP address to be static
Log into the VM and disable the Windows Firewall with command wf.msc (right click windows icon and press run)

Setup Client-1 in Azure

<img width="1916" height="927" alt="image" src="https://github.com/user-attachments/assets/e44bdb8b-fe4d-41ea-8427-cff08d6ff4a2" />
<img width="1903" height="856" alt="image" src="https://github.com/user-attachments/assets/ada334f5-dc92-4514-9aeb-888546c59d8b" />
<img width="1504" height="764" alt="image" src="https://github.com/user-attachments/assets/312cb968-4a45-48f1-b8e3-6d8455cf6ec7" />
<img width="1486" height="786" alt="image" src="https://github.com/user-attachments/assets/a22ff16a-a027-46f6-bef9-81b1a634f703" />


Create Client VM (Windows 10) named "Client-1" with username and password
Attach it to the same reason and Virtual Netwrok as DC-1
Set Client-1's DNS settings to DC-1's Private IP address
Restart Client-1 from Azure Portal and then login to Client-1 
Attempt to ping DC-1's private IP address - make sure that the ping was successful
From Client-1, open Powershell and run ipconfig /all
the output for the DNS should show DC-1's IP Address<br />

Install Active Directory

<img width="1910" height="989" alt="image" src="https://github.com/user-attachments/assets/d21ffcd0-ea59-4924-9355-d2c5849df972" />
<img width="1104" height="799" alt="image" src="https://github.com/user-attachments/assets/49823662-a052-4c49-9e14-b3cbf8406fe4" />
<img width="553" height="685" alt="image" src="https://github.com/user-attachments/assets/d16faa62-4b99-477b-bd12-a7270c1eb3dc" />

Log into DC-1 and install Active Directory Domain Services
Promote as DC: Setup a new forest as mydomain.com (Can be anything just remember what was named)
Restart and then log back into DC-1 as user: mydomain.com\labuser

Create a Domain Admin user within the domain

<img width="962" height="677" alt="image" src="https://github.com/user-attachments/assets/5692f3e9-ea90-4856-b7f9-8aa49250d44e" />
<img width="1894" height="987" alt="image" src="https://github.com/user-attachments/assets/50bf965d-e26a-4cfe-b6f5-001c166e2793" />


In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called "_EMPLOYEES" Create a new OU named "ADMINS"
Create a new employee "Jane Doe" (same password) with username (jane_admin)
Add jane_adminto the "Domain Admins" Security Group
Log out/close the connection to DC-1 and log back in as "mydomain.com\jane_admin"
User jane_admin as admin account from now on

Join Client-1 to domain (mydomain.com)

<img width="949" height="690" alt="image" src="https://github.com/user-attachments/assets/2486de7e-6823-4559-9b06-75e42149c1b3" />
<img width="949" height="662" alt="image" src="https://github.com/user-attachments/assets/550face5-b8cb-4322-9271-f80362487fdc" />

Login to Client-1 as the original local admin and join it to the domain (computer will restart)
Login to the Domain Controller and verify Client-1 shows up in ADUC
Create a new OU named "_CLIENTS" and drag Client-1 into there

Setup Remote Desktop for non-administrative users on Client-1

Log into Client-1 as mydomain.com\jane_admin

<img width="1280" height="987" alt="image" src="https://github.com/user-attachments/assets/9f3fabe1-03c9-4977-9bb3-0ed782a938ab" />

Open system properties
Click "Remote Desktop"
Allow "domain users" access to remote desktop
