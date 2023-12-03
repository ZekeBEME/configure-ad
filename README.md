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

- Setup Resources in Azure
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users
- Test Connection

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/hNuARhr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First, we will create two Virtual Machines in Azure named DC-1 and Client-1. We will make sure the Domain Controller is using Windows Server as the image and Client-1 uses Windows 10. Take note of making sure the two VMs are hosted on the same network to communicate with each other. 
</p>
<br />

<p>
<img src="https://i.imgur.com/bBW3wdg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once our VMs are created we can ensure connectivity between the two by utilizing the command-line tool ping. If the connection is blocked check the local firewall on DC-1 and enable ICMPv4. Check back to Client-1 to see if the ping succeeds. Don't forget to set DC-1's private IP address to static in the Azure Portal, this will be important later on.
</p>
<br />

<p>
<img src="https://i.imgur.com/i0Z1h9v.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we can log into DC-1 and install Active Directory Domain Services. Next, promote DC-1 to a DC controller and Setup a new forest with "yourdomain.com" (can be anything you want). When setup and installation are complete restart and log back into DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/W9DVZnT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
With our Active Directory successfully installed, we can now create an Admin and Normal user Account in AD. In AD create two Organizational Units (OU) called "_WORKERS" and "_ADMINS". Place a worker called "Jane Doe" and give her a simple username and password you can easily remember. Add Jane to the "Domain Admins" security group. Now we can log out of DC-1 and log back in as "Jane Doe" for our admin account.</p>
<br />

<p>
<img src="https://i.imgur.com/20a83pb.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we need to join Client-1 to "yourdomain.com" by changing Client-1's DNS setting to DC-1's private IP address in the Azure portal. Then restart Client-1 from the Azure Portal and re-login as the original local admin to join the domain. Login to the DC-1 and verify Client-1 shows up in Active Directory Users and Computers inside the "Computers" container on the root of the domain
</p>
<br />

<p>
<img src="https://i.imgur.com/l7Vc4bT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we need to setup remote Desktop for non-administrative users on Client-1. Login to Client-1 with "Jane Doe's" credentials and open system properties. Click “Remote Desktop” and allow “domain users” access to remote desktop.
You can now log into Client-1 as a normal, non-administrative user now
</p>
<br />

<p>
<img src="https://i.imgur.com/u880ArJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To test if this works you can go back into Active Directory on DC-1 and make a "Domain User" "John Doe" to see if you can connect to Client-1 (remote desktop).
</p>
<br />

