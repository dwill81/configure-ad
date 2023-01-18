<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
<h1>Implementing Active Directory (On-Premises) in Azure </h1>
In this tutorial, we will be activating Active Directory within a virtual machine. We will also create another client desktop within another virtual machine and have the users that are configured on the Active Directory attempt to login to the client desktop.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Command Line/Windows Powershell
- Microsoft Active Directory
- Control Panel

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>List of Prerequisites</h2>

- Windows Powershell
- Control Panel
- Remote Desktop

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="856" alt="image" src="https://user-images.githubusercontent.com/122701786/213235483-6131a53f-2060-42bd-88db-60e3a73393ba.png">
</p>
<p>
Step 1: Log into Azure --> search "virtual machines" --> click "create azure virtual machine" to create VM#1. Name this first virtual machine "DC-1" using your current region --> set the image type as "Windows Server 2022" (effectively making it a domain for the lab) --> Set username and password --> create VM #2 --> title it "Client-1" (repeat the same steps used to create VM#1 except for the image type select "Windows 10 pro" since this VM will be the employees'/ cleints' computer).
</p>
<br />

<p>
<img width="392" alt="image" src="https://user-images.githubusercontent.com/122701786/213237250-f0ffe0a2-761a-44d6-a32d-5a20ba8402d3.png">
<p>
Step 2: Go to DC-1's network settings --> select networking --> click the hyperlink next to "network interface" --> "IP Configurations" --> "ipconfig1" --> change the assignment from dynamic to static (this ensures DC-1's IP address will not change) --> check the NIC settings to make sure both VMs are on the same "Vnet". This will ensure both VMs can communicate & connect with each other later in this lab.
</p>
<br />

<p>
<img width="690" alt="image" src="https://user-images.githubusercontent.com/122701786/213237763-00985e49-80d7-4ab9-b769-ff521289f2a9.png">
<img width="774" alt="image" src="https://user-images.githubusercontent.com/122701786/213237857-4bea45d8-d425-4f09-89c2-dfe3c8548c1c.png">
</p>
<p>
Step 3: Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). The ping is expected to fail or "timeout". Then Remote Desktop into DC-1 via windows firwall security settings --> Advanced settings --> inbound/outbound rules to allow "IPV4 permissions" on 
 DC-1's Firewall. This will open the firewall for connectivity after DC-1 is converted into a domain.
</p>
<br />

<p>
<img width="692" alt="image" src="https://user-images.githubusercontent.com/122701786/213240241-c8c38a79-3367-49fb-a27f-fb7f5d8d6f61.png">
</p>
<p>
Step 4: Ensure communication between both VMs via perpetual ping using cmd:ping -t (Ip Address).
</p>
<br />

<p>
<img width="659" alt="image" src="https://user-images.githubusercontent.com/122701786/213243335-8d4d2dc1-7e2e-4455-94a6-e359e360cec1.png">
</p>
<p>
Step 5: Install "Active Directory" on DC-1. Set up DC-1 as a new domain.
</p>
<br />


<p>
<img width="594" alt="image" src="https://user-images.githubusercontent.com/122701786/213254127-7bc319b0-be36-4b0a-9d66-a8a99f2ab824.png">
</p>
<p>
9. Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
</p>
<br />


<p>
10. Restart and then Remote Desktop back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://imgur.com/JNPV4Q6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
11. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and “_ADMINS” 
</p>
<p>
 
<img width="580" alt="image" src="https://user-images.githubusercontent.com/122701786/213261134-711a4d8c-8596-4a5d-bb37-6febb47627dc.png">

<p> 
 13. Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” to the _ADMINS OU. Then add jane_admin to the “Domain Admins” Security Group 
</p>
<p>

<img width="356" alt="image" src="https://user-images.githubusercontent.com/122701786/213263313-3d72da15-0e03-46a6-8c52-45a0a33ad518.png">

<p>
  15. Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
<p>


 <img width="396" alt="image" src="https://user-images.githubusercontent.com/122701786/213265787-a9d0d5be-a169-4953-a61a-4f1e50dd2dce.png">
<p> 17. From the Azure Portal, set Client-1’s DNS settings to DC-1’s Private IP address then restart Client-1 from the azure portal.</p>


<img width="693" alt="image" src="https://user-images.githubusercontent.com/122701786/213269898-c039cf2e-8767-48fb-a7b1-b248870afc66.png">
<img width="589" alt="image" src="https://user-images.githubusercontent.com/122701786/213269972-2145eada-5b8e-4973-ac69-bc65206f2f38.png">

<p> 19. Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). To do this you right click the start menu the go to system the rename this pc(advanced) then click change and change it from workgroup to domain.</p>

<p>
<img width="347" alt="image" src="https://user-images.githubusercontent.com/122701786/213271688-5759c621-6d90-4b79-9c36-c46733fb6e32.png">
</p>

<p> 20. When Client-1 restarts log back in (Remote Desktop) as mydomain.com\jane_admin and verify that client-1 shows up in ADUC</p>


<p>
<img src= "https://imgur.com/KhR7rDB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 21. Create a new OU named “_CLIENTS” and drag Client-1 into there</p>

<p>
<img src= "https://imgur.com/AP2QvUl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p> 22.Log into Client-1 as mydomain.com\jane_admin and open system properties. Click “Remote Desktop” and allow “domain users” access to remote desktop.</p>

<p> 23.You can now log into Client-1 as a normal, non-administrative user. Login to DC-1 as jane_admin and
open PowerShell_ise as an administrator</p>


<img src= "https://imgur.com/eOOi5LN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 24.Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).</p>

<img src= "https://imgur.com/ngYiYVI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 25.Run the script and observe the accounts being created. Open ADUC and observe the accounts in the appropriate OU. Note: Since this Github lab was completed on multiple days, I changed the domain name from www.mydomain.com to mydomain.com</p>

<img src= "https://imgur.com/DhzSers.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>


<p> 26. Attempt to log into Client-1 with one of the accounts (take note of the password in the script).</p>
