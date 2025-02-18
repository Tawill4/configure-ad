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

- Create a Domain Controller Virtual Machine in Azure
- Set Domain Controllers NIC to static
- Log into DC-1 using RDP and disable the firewall
- Create a Client-1 Virtual Machine in azure
- Set Client-1's DNS settings to DC-1's private IP address
- Log into Client-1 using RDP to observe the connectivity between the two

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
