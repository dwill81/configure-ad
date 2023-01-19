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


<h2>Deployment and Configuration Steps</h2>

<p>
<img width="856" alt="image" src="https://user-images.githubusercontent.com/122701786/213235483-6131a53f-2060-42bd-88db-60e3a73393ba.png">
</p>
<p>
Step 1: Log into Azure --> search "virtual machines" --> click "create azure virtual machine" to create VM#1. Name this first virtual machine "DC-1" using your current region --> set the image type as "Windows Server 2022" (effectively making it a domain for the lab) --> Set username and password --> create VM #2 --> title it "Client-1" (repeat the same steps used to create VM#1 except for the image type select "Windows 10 pro" since this VM will be the employees'/ clients' computer).
</p>
<br />

<p>
<img width="392" alt="image" src="https://user-images.githubusercontent.com/122701786/213237250-f0ffe0a2-761a-44d6-a32d-5a20ba8402d3.png">
<p>
Step 2: Go to DC-1's network settings --> select networking --> click the hyperlink next to "network interface" --> "IP Configurations" --> "ipconfig1" --> change the assignment from dynamic to static (this ensures DC-1's IP address will not change) --> check the NIC settings to make sure both VMs are on the same "Vnet". This will ensure both VMs can communicate & connect with each other.
</p>
<br />

<p>
<img width="690" alt="image" src="https://user-images.githubusercontent.com/122701786/213237763-00985e49-80d7-4ab9-b769-ff521289f2a9.png">
<img width="774" alt="image" src="https://user-images.githubusercontent.com/122701786/213237857-4bea45d8-d425-4f09-89c2-dfe3c8548c1c.png">
</p>
<p>
Step 3: Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). The ping is expected to fail or "timeout". Then Remote Desktop into DC-1 via windows firewall security settings --> Advanced settings --> inbound/outbound rules to allow "ICMPv4 permissions" on 
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
Step 6: Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
</p>
<br />


<p>
Step 7: Restart and then Remote Desktop back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
<img src="https://imgur.com/JNPV4Q6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 8: In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and “_ADMINS” 
</p>
<p>
 
<img width="580" alt="image" src="https://user-images.githubusercontent.com/122701786/213261134-711a4d8c-8596-4a5d-bb37-6febb47627dc.png">

<p> 
Step 9: Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” to the _ADMINS OU. Then add jane_admin to the “Domain Admins” Security Group 
</p>
<p>

<img width="356" alt="image" src="https://user-images.githubusercontent.com/122701786/213263313-3d72da15-0e03-46a6-8c52-45a0a33ad518.png">

<p>
Step 10: Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
<p>


 <img width="396" alt="image" src="https://user-images.githubusercontent.com/122701786/213265787-a9d0d5be-a169-4953-a61a-4f1e50dd2dce.png">
<p>
 Step 11: From the Azure Portal, set Client-1’s DNS settings to DC-1’s Private IP address then restart Client-1 from the Azure Portal.</p>
<p>

<img width="693" alt="image" src="https://user-images.githubusercontent.com/122701786/213269898-c039cf2e-8767-48fb-a7b1-b248870afc66.png">
<img width="589" alt="image" src="https://user-images.githubusercontent.com/122701786/213269972-2145eada-5b8e-4973-ac69-bc65206f2f38.png">

Step 12: Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart). To do this you right click the start menu the go to system the rename this pc(advanced) then click change and change it from workgroup to domain.</p>

<p>
<img width="347" alt="image" src="https://user-images.githubusercontent.com/122701786/213271688-5759c621-6d90-4b79-9c36-c46733fb6e32.png">
</p>

Step 13: When Client-1 restarts log back in (Remote Desktop) as mydomain.com\jane_admin and verify that client-1 shows up in ADUC.</p>

</p>
<br />

<p>
<img width="854" alt="image" src="https://user-images.githubusercontent.com/122701786/213288218-6c1eb94d-4048-4557-b2c6-cba2255d4ace.png">
</p>
<p>
Step 14: Use Remote Desktop in the system settings to allow domain users access for all non-admin users on Client-1 VM under "user accounts" --> "select users that can remotely access this PC" --> click "add" and type in "domain users". 
</p>
<br />

<p>
<img width="860" alt="image" src="https://user-images.githubusercontent.com/122701786/213292693-6948338c-38ce-428a-bc40-d39f673d71cb.png">
</p>
<p>
Step 15: Use a random account generating script to create a list of at least 100 users. Upload script via "Powershell ISE" (run as administrator) to Client-1. This will create new users with random names. This is done to simulate employees within the company.
</p>
<br />

<p>
<img width="350" alt="image" src="https://user-images.githubusercontent.com/122701786/213295274-9c941a07-ae8f-49b1-a171-d0ca962bdd44.png">
 <img width="201" alt="image" src="https://user-images.githubusercontent.com/122701786/213295334-3f13c7fe-3aee-417f-913d-2fd1788ef4b4.png">
<img width="382" alt="image" src="https://user-images.githubusercontent.com/122701786/213295371-13c49ad4-9568-4f7f-8f0c-c8e2ad3d3c2c.png">

</p>
<p>
Step 16: Log into any newly generated user account on Client-1 VM. The login attempt with the user's name & generic password should be successful.
</p>
<br />
