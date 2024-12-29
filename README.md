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

- Step 1 Setup Client-1 in Azure
- Step 2 Setup Domain Controller in Azure
- Step 1 Install Active Directory
- Step 2 Create a Domain Admin user within the domain (VM-1)
- Step 3 Join (VM-2) to your domain (mydomain.com)
- Step 4 Setup Remote Desktop for non-administrative users on (VM-2)
- Step 5 Create additional users and attempt to log into (VM-2) with one of the users

<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/b79949da-60ba-42d6-a6da-42e6954dd807)
![image](https://github.com/user-attachments/assets/04de5a89-c39f-4125-ad2d-4d6185e8422e)


- Create a Resource Group
Create a Virtual Network and Subnet
Create the Domain Controller VM (Windows Server 2022), (VM-1)
After (VM-1) is created, set Domain Controller’s NIC Private IP address to be static
Log into the (VM-1) and disable the Windows Firewall (for testing connectivity)

- Create the Client VM (Windows 10) (VM-2)
Attach it to the same region and Virtual Network as the (VM-1),
After the (VM-2) is created, set (VM-2) DNS settings to (VM-1) Private IP address
From the Azure Portal, restart (VM-2)
Login to (VM-2)
Attempt to ping (VM-1) private IP address
Ensure the ping succeeded
From (VM-2), open PowerShell and run ipconfig /all
The output for the DNS settings should show (VM-1) private IP Address

![image](https://github.com/user-attachments/assets/8d9f54ca-b547-4b83-a6c5-3e2996cddab5)

- Login to (VM-2) and install Active Directory Domain Services,
Promote as a DC: Setup a new forest as mydomain.com,
Restart and then log back into (VM-2) as user: mydomain.com\user.

![image](https://github.com/user-attachments/assets/da928e3f-0c05-46e8-8def-c570998ea512)


- In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”,
Create a new OU named “_ADMINS”,
Create a new employee named “J D” (same password) with the username of “J_admin” / password.
Add J_admin to the “Domain Admins” Security Group,
Log out / close the connection to (VM 1) and log back in as “mydomain.com\J_admin”.

![image](https://github.com/user-attachments/assets/990d7fd4-b5bb-47b4-bf82-1ea6ae3e0452)


- Login to (VM-2) as the original local admin and join it to the domain (computer will restart),
Login to the Domain Controller and verify (VM-2) shows up in ADUC,
Create a new OU named “_CLIENTS” and drag (VM-2) into there.

![image](https://github.com/user-attachments/assets/311b2032-cc06-4ef4-9fbf-b3f77cf0a23d)


- Log into (VM-2) as mydomain.com\J_admin,
Open system properties,
Click “Remote Desktop”,
Allow “domain users” access to remote desktop,
You can now log into (VM-2) as a normal, non-administrative user now.


![image](https://github.com/user-attachments/assets/d059b717-d0e8-44bd-9ce0-75e6e7ee42dc)


- Login to (VM-1) as J_admin,
Open PowerShell_ise as an administrator,
Create a new File and paste the contents of the script into it,
Run the script and observe the accounts being created,
When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES),
attempt to log into (VM-2) with one of the accounts (take note of the password in the script).
