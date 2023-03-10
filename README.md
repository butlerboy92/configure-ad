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

- Setup Resources in Azure and ensure connectivity
- Install Active Directory
- Create accounts and join client to domain
- Setup remote desktop for non-admin users
- Create a bunch of accounts (using a script)

<h2>Deployment and Configuration Steps</h2>

<strong>Step 1:</strong> Setup Resources in Azure and ensure connectivity
<br />
<br />

<p>
Create two Virtual Machines in Azure, one named DC-1 (Server) and the other Client-1 (Win10)
<br />
<br />
Make sure you remember the username and password you create
</p>
<br />
<br />
<strong>✪ DC-1 VM</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/ToGULuK.jpg" height="80%" width="80%" alt="Creating DC-1 Virtual Machine"/>
</p>

<br />
<br />

<strong>✪ Client-1 VM</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/mcddAXN.jpg" height="80%" width="80%" alt="Creating Client-1 Virtual Machine"/>
</p>

<br />
<br />
<p>
Now we want to set DC-1 private IP address to be static so it doesn't change. (in Azure) Virtual Machines ⇒ DC-1 ⇒ Networking ⇒ (Next to Networking Interface click dc-***) ⇒ IP Configurations ⇒ Click the private ip address with dynamic ⇒ Switch it to static then click save
</p>
<br />
<br />
<strong>✪ Networking Interface w/ dc-***</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/QyDyni9.jpg" height="80%" width="80%" alt="Networking Interface Location"/>
</p>

<br />
<br />
<strong>✪ Swapping to Static</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/CGG1hVz.jpg" height="80%" width="80%" alt="Swapped IP to static"/>
</p>

<br />
<br />

<strong>Step 1.2:</strong> Ensure Connectivity with Client and Domain
<br />
<br />

<p>
Login to Client-1 remotely and ping DC-1 ip address with ping -t (so it constantly keeps pinging)
<br />
<br />
Login to DC-1 remotely and in the search bar on the task bar search <strong>wf.msc</strong> and continue
<br />
<br />
Sort by Protocol to find ICMPv4 and then enable both ICMP Echo requests and then check back on Client-1 for a response
</p>
<br />
<br />

<strong>✪ Client-1 failing ping to DC-1</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/GElhj1N.jpg" height="80%" width="80%" alt="Client-1 failing ping to DC-1"/>
</p>

<br />
<br />
<strong>✪ Enabling pings in DC-1</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/ABVH4Tl.jpg" height="80%" width="80%" alt="Enablinging Ping in DC-1"/>
</p>

<br />
<br />
<strong>✪ Checking Client-1 for response from DC-1</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/95PwvPd.jpg" height="80%" width="80%" alt="Response from DC-1"/>
</p>

<br />
<br />

<strong> Step 2:</strong> Install Active Directory
<br />
<br />
<p>
Login to DC-1 and install Active Directory
<br />
<br />
Make sure Server Manager is opened and then click Add roles and features then you want to click 'Next' till you get to Server roles and make sure Active Directory Domain Service is clicked. Then click 'Next' until you get to install.
<br />
<br />
Once installed go to the flag with a yellow exclimation mark and click Promote this server to a domain controller
<br />
<br />
In the radio selector select new root and name your domain (whatever).com and then 'Next' and just make the password whatever (it wont be used) then 'Next' through until you get to install and install AD and let it set up. It will need to restart so just reconnect after but you will have to use a different account called (nameofyourdomain)\(username) and then same password as the one you created during VM setup. Example:( mydomain.com\labuser1 ).
</p>

<br />
<br />
<strong>✪ Installing Active Directory</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/2xskjNc.jpg" height="80%" width="80%" alt="Installing Active Directory"/>
</p>

<br />
<br />
<strong>✪ Making into a Domain Controller</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/VkMTMnF.jpg" height="80%" width="80%" alt="Making into a Domain Controller"/>
</p>

<br />
<br />
<strong>✪ Reconnecting DC-1 as domain controller with full quailified domain name</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/IwOYVdR.jpg" height="80%" width="80%" alt="Making into a Domain Controller"/>
</p>

<br />
<br />


<strong> Step 3:</strong> Create Admin and Normal User in Active Directory
<br />
<br />
<p>
Start by creating a few organizational units in Active Directory
<br />
<br />
Click 'Tools' and then click 'Active Directory Users and Computers' and right click 'mydomain.com' ⇒ New ⇒ Organizational Units
<br />
Create _EMPLOYEES and _ADMINS
<br />
<br />
Then in _ADMINS right click and create 'New' ⇒ 'User' name it whatever then right click your user and go to Properties >> Member of >> Add >> Domain Admins 
<br />
<br />
OK and Apply to finish. Then logout of DC-1 and reconnect as mydomain.com\jane_admin
  and use this as your admin from now on.
</p>

<br />
<br />
<strong>✪ Creating _EMPLOYEES and _ADMINS</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/dyKLGfD.jpg" height="80%" width="80%" alt="Creation of _EMPLOYEES and _ADMINS"/>
</p>

<br />
<br />
<strong>✪ Creating a user in _ADMINS</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/EZZGgnf.jpg" height="80%" width="80%" alt="Creation of user in _ADMINS"/>
</p>

<br />
<br />
<strong>✪ Making user in _ADMINS an Admin</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/v0PyInY.jpg" height="80%" width="80%" alt="Making user into an Admin"/>
</p>

<br />
<br />
<strong>✪ Reconnecting into DC-1 as new admin</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/HftVymZ.jpg" height="80%" width="80%" alt="Reconnecting to DC-1 as new Admin"/>
</p>


<strong> Step 3.1:</strong> Join Client-1 to the domain
<br />
<br />
<p>
(in Azure) Set Client-1's DNS settings to DC-1's private IP Address
<br />
<br />
Virtual Machines ⇒ Client-1 ⇒ Networking ⇒ Network Interface: client-**** ⇒ DNS Servers ⇒ Custom ⇒ addin DNS private IP Address
<br />
<br />
Restart Client-1 from Azure Virtual Machines ⇒ Client-1 ⇒ Restart
<br />
<br />
Reconnect to Client-1 and join Client-1 to DC-1
<br />
Right click start ⇒ System ⇒ Rename This PC ⇒ Change ⇒ Domain ⇒ mydomain.com ⇒ use admin account when prompt to make the changes ⇒ Restart Client-1
<br />
<br />
Now login to Client-1 with the new admin account
</p>

<br />
<br />
<strong>✪ Changing Client-1's DNS Settings</strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/U7a8Ssl.jpg" height="80%" width="80%" alt="Client-1's VNIC"/>
</p>
<p>
<img src="https://i.imgur.com/HdvaX95.jpg" height="80%" width="80%" alt="Client-1's DNS settings changed with DC-1's Private IP Address"/>
</p>

<br />
<br />
<strong>✪ Changing Client-1's PC name to Domain mydomain.com for conneciton to DC-1 </strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/X3T7jf2.jpg" height="80%" width="80%" alt="Client-1's PC name setting change to mydomain.com"/>
</p>


<strong> Step 4:</strong> Set up remote access for non-admin users on Client-1
<br />
<br />
<p>
Login to Client-1 as the admin from DC-1 
<br />
Right click start ⇒ System ⇒ Remote Desktop ⇒ Select users that can remotely access this PC ⇒ ADD ⇒ Domain users
<br />
<br />
Now you can log into Client-1 as a normal non-admin user
</p>

<br />
<br />
<strong>✪ Setting up Remote access for non-admin users </strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/YJNCxAe.jpg" height="80%" width="80%" alt="Setting up remote access for non-admin users"/>
</p>

<br />
<br />

<strong> Step 5:</strong> Create additional users using a script!
<br />
<br />
<p>
Login to DC-1 as admin
<br />
Open Powershell ise as administrator
<br />
Search in task bar powershell.ise ⇒ right click open as administrator
<br />
<br />
Create a new file and paste the contents of the <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">script</a> in there
<br />
The accounts will be created and places into _EMPLOYEES (you can stop the script or let it run)
<br />
<br />
Now you can login to Client-1 with any of these accounts created using mydomain.com\(user name created) and Password1
</p>

<br />
<br />
<strong>✪ Creating new Users with a script in Powershell ISE </strong>
<br />
<br />
<p>
<img src="https://i.imgur.com/acXvRHL.jpg" height="80%" width="80%" alt="Creating new users using a script"/>
</p>

<br />
<br />
<h2>Finished!</h2>
<p>
Now you can mess with the accounts in DC-1 (reset passwords, disable account, etc.) and get a feel for Active Directory and how it works!
<br />
You can also research more things that Active Diretory can do and try it out for yourself.
</p>

<br />
<br />

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>
