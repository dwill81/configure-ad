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
<img src="https://i.imgur.com/SVRL4NQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6: Remote Desktop into DC-1 to create two "Organzational Units" (OU), one titled "Admins" and another titled "Employees" within Active Directory.
</p>
<br />

<p>
<img src="https://i.imgur.com/ShU8C26.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Urmjpmq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 7: Change Client-1's "DNS settings" in Azure to match the same private IP Address as DC-1 via network settings in DC-1. Go into Client-1's network settings --> Network Interface (NIC) --> DNS server --> custom DNS settings --> add DC-1's private IP Address as the DNS server to connect to for Client-1. Restart Client-1 to flush the DNS cache --> change Client-1 to the same domain as DC-1 via "about PC" --> rename this PC advanced --> type DC-1's domain name under the "domain section" --> create a new OU named "_clients".
</p>
<br />

<p>
<img src="https://i.imgur.com/ekCSO1N.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 8: Use Remote Desktop in the system settings to allow domain users access for all non-admin users on Client-1 VM under "user accounts" --> "select users that can remotely access this PC" --> click "add" and type in "domain users". 
</p>
<br />

<p>
<img src="https://i.imgur.com/XnDeUOB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 9: Use a random account generating script to create at least 100 users for this lab. Upload script via "Powershell ISE" (run as administrator) to Client-1. This will create 100 new users with random names. This is done to simulate employees within the company.
</p>
<br />

<p>
<img src="https://i.imgur.com/4wPUapk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 10: Log into any newly generated user account on Client-1 VM. The login attempt with the user's name & generic password should be successful. That is the conclusion of this lab.
</p>
<br />
