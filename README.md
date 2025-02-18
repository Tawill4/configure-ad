<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://youtu.be/EQpU-NxvcaM?si=PfNa4UWkI1welb4X)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Domain Controller Virtual Machine in Azure.
- Set Domain Controllers NIC to static.
- Log into DC-1 using RDP and disable the firewall.
- Install and configure Active Directory Domain Services on DC-1.
- Create a Client-1 Virtual Machine in azure.
- Set Client-1's DNS settings to DC-1's private IP address.
- On DC-1, Create Organizational Units to store Users and Computers within Active Directory.
- Create an Admin user in the _ADMINS OU.
- Connect Client-1 VM to the domain, and drag into _CLIENTS OU.
- Give Domain Users / Non-Admins access to Client-1 through Remote Desktop.
- Run a script to create a ton of random users.
- Use Group Policy to configure Accout Lockout Policy.
- Force apply the changes made from client-1 command prompt.
- Lockout a random created user, and unlock the account using the Domain Controller.
- Reset users password.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/xMnPqex.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/uF5Mlio.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
The first step in setting up Active Directory is to create a Domain Controller virtual machine. Make sure to put it in "Windows server 2022 Datacenter Azure Edition". After creation of the VM you want to set the Private IP address to static.. this makes sure the IP address stays the same no matter how many times it gets refreshed or restarted. This is done because later in the lab, the DNS server for Client-1's VM needs to be the DC's private IP address for communication purposes (so the domain we create can be found).
</p>
<br />

<p>
<img src="https://i.imgur.com/pN0fpbl.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/F1V1EVr.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next in order to make sure the VM's can communicate with eachother, the DC's firewall needs to be deactivated. To do this, right click the windows logo at the bottom right and type "wf.msc" this opens the windows firewall stuff. Now click "windows defender firewall properties". Turn off the firewall for the first three tabs.
</p>
<br />

<p>
<img src="https://i.imgur.com/hpynVvf.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/mgSgB6B.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Eo6OBOI.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that the firewall is deactivated, Active Directory needs to be installed. To do this go to "server manager" from within the DC and click on "add roles and features", it will pop up another window where you click next until you get to "server roles". From there click the check box next to "Active Directory Domain Services". Then click "add roles". All thats left is to click next to the end of the pop-up, and click install.
</p>
<br />

<p>
<img src="https://i.imgur.com/Thpn1Xm.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/nuEDAYL.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/OAFxXS7.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/REw4FoF.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/fmroHdh.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
After installing Active Directoy Domain Services it needs to be configured, so first; click on the small flag at the top right and click "promote this server to a Domain Controller". It will pop up another window. From there click on "create a new forest" then name it "mydomain.com" for this labs purposes. Click next and then uncheck the "create DNS delegation" box. After that click next until "prerequesites check", then press install. After this step, anytime you login to the Domain Controller, you need to specify the domain in the username slot. For example the username for the VM is "labuser" so it would now need to be "mydomain.com\labuser".
</p>
<br />

<p>
<img src="https://i.imgur.com/n3Bhivm.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/fsWX9Kt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that the DC and active directory is setup, create a new VM camed client one running windows 10 (anything over 2vcpus is fine). After creation click on the client-1 Vm and go to dns servers under network settings. From there click custom and put the DNS server as DC-1's private IP address. This is why DC-1's IP address was changed to static earlier.
</p>
<br />

<p>
<img src="https://i.imgur.com/ujCwvZN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/BU385Vs.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/0kaVPLu.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
With Active Directory being downloaded and configured to our DC-1, OU's need to be made (Organizational Units). To do this click the windows icon on the bottom of your DC-1 VM, and click "windows administrative tools" -> "Active Directory Users and Computers". Then right click on the domain that was created (mydomain.com in this case) and press "new" -> Organizational Unit. Name this OU "_EMPLOYEES" make sure this is spelt correctly, has an underscore at the beginning, and is in caps. It is very important for a future step. Then repeat the process with another OU named "_ADMINS".
</p>
<br />

<p>
<img src="https://i.imgur.com/f1GCdij.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/bqFDFs1.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/R5Rl2IW.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Inside the _ADMINS OU, you want to create a new user. Right click inside the _ADMINS OU and click "new" -> User. This will pull up a pop up screen where you just create a name and password for this user. For this labs purposes, Uncheck User must change password at next logon, and check the password never expires (not good to do in real life practice).
</p>
<br />

<p>
<img src="https://i.imgur.com/hglXCFK.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/bzKGp5P.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Just creating a user inside the _ADMINS OU does not make them an actual admin yet. To do so, right click the created user and go to properties. Then under the member of tab click "add", in the text box type "Domain Admins". This is a default role from within the domain so click check names and the actual group will appear then press "okay". Now log into Client-1 as the newly created admin user (remember to specify the domain before the username when logging in).
</p>
<br />

<p>
<img src="https://i.imgur.com/gRrD7v8.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/B7cM38r.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/cmU6RzV.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Logged into Client-1 as our admin user, Client-1 needs to be connected to our created domain. This is done by right clicking the Windows logo on the bottom left, going to system, clicking "Rename this PC (advanced)", going to "Change", and clicking the circle next to domain. Then put the domain name in that text box (mydomain.com in this case). A small pop up may pop up underneat the current window if u think nothing happens. This should restart your VM. Next, go to DC-1 and create a new OU named "_CLIENTS". IF you click "computers" inside the AD users and Computers window, you can drag "Client-1" into the newly created _CLIENTS OU.
</p>
<br />

<p>
<img src="https://i.imgur.com/kYMW3kG.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/x4zQHlC.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/YrcGTKy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
There is about to be a ton of random users created to simulate an office with a lot of employees for a later part of the lab, but they need to be able to access client-1 without being an admin.. in order to do this i need to give "Domain Users" access to RDP for client-1. In client-1 right click the Windows logo at the bottom left and go to system. On the right click Remote Desktop, and then click "Select users that can remotely access this PC". Click "Add" and then type "Domain Users". Click "okay" and its all set.
</p>
<br />

<p>
<img src="https://i.imgur.com/HcLPZUX.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/XOwlxxa.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
In order to create a bunch of randomly generated users to work with in this lab, you need to go to DC-1, and run "Windows Powershell ISE" as an administator. Then copy and paste this code into the white section on the top... heres the link. [this link](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) then press run. This creates 1000 users all with random constanants and vowels. They all share the same password "Password1". This was also the reason _EMPLOYEES needed to be spelled correctly, because this specific script puts these "employees"/users in the _EMPLOYEES OU. Since we allowed "Domain Users" to access client-1 Remotely in the previous step, you should be able to log into Client-1 with any of these created users.
</p>
<br />

<p>
<img src="https://i.imgur.com/OPNLRSX.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/3Y4vi2C.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/33jhbw6.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/F9PvhZo.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
To gain intuition on fixing account lockouts do to failed password attempts or anything similar, create a Group Policy for Account Lockout. This way were able to lock an account out, then go back into DC-1 and unlock it. To open the Group Policy Management Console, in DC-1, Right click the Windows icon and select "run". Then type gpmc.msc. This opens the console. Right click on "Default Domain Policy" and go to edit. Then click the following drop down selections; Policies -> Windows Settings -> Security Settings -> Account Lockout Policy. Then configure the options with whatever units you decide and apply. This does not update immediately. You can wait 90 minutes or you can log in to Client-1 as an admin and go to powershell to type in this command... "gpupdate /force". This will force apply the changes made to default domain policy.
</p>
<br />

<p>
<img src="https://i.imgur.com/9AQn9fN.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/0yKgIRy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/wBrOhEm.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/gOpbnJw.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that Account Lockout is configured, Select a randomw created user and lock them out of their account from failed password attempts, then go to DC-1 and find that user. Right click the user and click properties. Then go to account and check the box that says unlock account... you can also right click the user again and reset their password.
</p>
<br />
