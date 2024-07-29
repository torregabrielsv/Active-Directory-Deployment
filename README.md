# Active-Directory-Deployment
This lab guides you through setting up an Active Directory environment in Azure.

# Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Take note of the Resource Group and Virtual Network (Vnet) that is created at this time

Set Domain Controller’s NIC Private IP address to be static

Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created for DC-1

Ensure that both VMs are in the same Vnet 

![Screenshot 2024-07-28 at 10 39 23 PM](https://github.com/user-attachments/assets/efe82b5c-3e2a-436c-8ba4-473699eba003)

# Ensure Connectivity between the client and Domain Controller

Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)

Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

Check back at Client-1 to see the ping succeed

<img width="991" alt="Screenshot 2024-07-28 at 8 48 08 PM" src="https://github.com/user-attachments/assets/192df915-fad7-488c-877c-a4883f0bc5ad">

# Install Active Directory

Login to DC-1 and install Active Directory Domain Services

Promote as a DC: Setup a new forest as mydomain.com, it can be anything, just remember what it is

Restart and then log back into DC-1 as user: mydomain.com\labuser

<img width="809" alt="Screenshot 2024-07-28 at 8 50 27 PM" src="https://github.com/user-attachments/assets/9e5440d6-0da9-4e77-9e9a-4dd809276eab">

# Create an Admin and Normal User Account in AD

In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”

Create a new OU named “_ADMINS”

Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

Add jane_admin to the “Domain Admins” Security Group

Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”

User jane_admin as your admin account from now on

<img width="783" alt="Screenshot 2024-07-28 at 10 15 34 PM" src="https://github.com/user-attachments/assets/ed5a8f5a-8728-4bec-9d0b-a99369cd7e87">

# Join Client-1 to your domain (mydomain.com)

From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address

From the Azure Portal, restart Client-1

Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)

Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain

Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes)

<img width="783" alt="Screenshot 2024-07-28 at 10 55 46 PM" src="https://github.com/user-attachments/assets/51baa5aa-4504-4c24-b35c-7c1928743ad5">

# Setup Remote Desktop for non-administrative users on Client-1

Log into Client-1 as mydomain.com\jane_admin and open system properties

Click “Remote Desktop”

Allow “domain users” access to remote desktop

You can now log into Client-1 as a normal, non-administrative user now

Normally you’d want to do this with Group Policy that allows you to change MANY systems at once

<img width="399" alt="Screenshot 2024-07-28 at 11 00 37 PM" src="https://github.com/user-attachments/assets/8e440d81-acf5-44b2-9d11-8aef8420c6c5">

# Create a bunch of additional users and attempt to log into client-1 with one of the users

Login to DC-1 as jane_admin

Open PowerShell_ise as an administrator

Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

When finished, open ADUC and observe the accounts in the appropriate OU, attempt to log into Client-1 with one of the accounts (take note of the password in the script)

<img width="1440" alt="Screenshot 2024-07-28 at 10 30 09 PM" src="https://github.com/user-attachments/assets/f67e28ab-f1d5-42c6-8f86-5cab5da01589">












