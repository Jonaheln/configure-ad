<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1 Install Active Directory
- Step 2 Create a Domain Admin user within the domain
- Step 3 Join Client-1 to your domain (mydomain.com)
- Step 4 Setup Remote Desktop for non-administrative users on Client-1
- Step 5 Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/e75156b5-997d-4991-9cbc-9b11794fa6e1)


![image](https://github.com/user-attachments/assets/8d9f54ca-b547-4b83-a6c5-3e2996cddab5)

- Create a Resource Group,
Create a Windows 10 Virtual Machine (VM),
While creating the VM, select the previously created Resource Group,
While creating the VM, allow it to create a new Virtual Network (Vnet) and Subnet,
Create a Linux (Ubuntu) VM,
While creating the VM, select the previously created Resource Group and Virtual Network—the Virtual Network MUST BE THE SAME.
Authentication type: Username/Password,
Ensure both VMs are in the same Virtual Network / Subnet.


- Login to DC-1 and install Active Directory Domain Services,
Promote as a DC: Setup a new forest as mydomain.com,
Restart and then log back into DC-1 as user: mydomain.com\labuser.

![image](https://github.com/user-attachments/assets/da928e3f-0c05-46e8-8def-c570998ea512)


- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”,
Create a new OU named “_ADMINS”,
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!,
Add jane_admin to the “Domain Admins” Security Group,
Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”,
User jane_admin as your admin account from now on.

![image](https://github.com/user-attachments/assets/990d7fd4-b5bb-47b4-bf82-1ea6ae3e0452)


- Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart),
Login to the Domain Controller and verify Client-1 shows up in ADUC,
Create a new OU named “_CLIENTS” and drag Client-1 into there.

![image](https://github.com/user-attachments/assets/311b2032-cc06-4ef4-9fbf-b3f77cf0a23d)


- Log into Client-1 as mydomain.com\jane_admin,
Open system properties,
Click “Remote Desktop”,
Allow “domain users” access to remote desktop,
You can now log into Client-1 as a normal, non-administrative user now.


![image](https://github.com/user-attachments/assets/d059b717-d0e8-44bd-9ce0-75e6e7ee42dc)


- Login to DC-1 as jane_admin,
Open PowerShell_ise as an administrator,
Create a new File and paste the contents of the script into it,
Run the script and observe the accounts being created,
When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES),
attempt to log into Client-1 with one of the accounts (take note of the password in the script).
