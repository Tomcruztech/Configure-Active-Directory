# configure-ad
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


Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
Set Domain Controller’s NIC Private IP address to be static
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/26c9c7ec-9245-4535-adae-493e4aa6d8df)


Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
Check back at Client-1 to see the ping succeed

![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/dd7c2123-46b7-4655-b876-9f80f89ab38d)
![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/2eea6439-2321-4569-adfc-154cec2e3efd)


Install Active Directory
Login to DC-1 and install Active Directory Domain Services
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
Restart and then log back into DC-1 as user: mydomain.com\labuser

![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/45a6e014-0c9d-46c7-a07a-46a0b9854240)


Create an Admin and Normal User Account in AD
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
Create a new OU named “_ADMINS”
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
Add jane_admin to the “Domain Admins” Security Group
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on

![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/74208d45-debb-4154-8ba8-982657a50be4)
![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/d356344c-bcaf-44ec-b908-3d8abd447447)
![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/8050f75b-8f11-4c18-9feb-7e70ce9a1683)



Join Client-1 to your domain (mydomain.com)
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)




Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
Allow “domain users” access to remote desktop
You can now log into Client-1 as a normal, non-administrative user now
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/5adaf531-a0b5-480d-8d06-76d8b6855811)


Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)


![image](https://github.com/Tomcruztech/Configure-Active-Directory/assets/160645953/ded1201b-8c22-4515-bbb7-5436a5b9d2ad)

Finish.
