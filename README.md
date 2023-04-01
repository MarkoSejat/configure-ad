<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines. Active Directory allows us to manage thousands of user accounts, passwords, and devices in a single space. It also allows us to control access to resources on a large scale. Whenever we hear of people who work at help-desk speak about performing tasks such as reseting passwords, chances are that it was done in Active Directory. In this tutorial, I will show how to set everything up. Enjoy. <br />


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
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com or whatever you wanted to name yours)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/1Cst138.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
As I stated before, AD allows us to centrally control resources, user accounts and devices. But before we can do that, we have to set up and house the AD on a server or what we call "Domain Controller" (DC-1). So if we can imagine a university where there are thousands of students, we want to make it so that each individual student can log into any computer (Client) that is "joined" to the AD with their own unique login credentials (account name/password). This will allow students to do things such as see the same desktop that is unique to them from any computer that they log in from and utilize all the information stored. In this lab, the focus will mainly be on permissions and user accounts but there are steps we must do before we get to that part. In this section We will start by setting up our Active Directory. Since I am doing this from home for demonstration purposes, I will be doing this whole process through Virtual Machines in Microsft Azure to simulate a domain controler and a client desktop that will be joined to it. Now to clerify, there are two versions of active directory. One version is the "on-premise" version which is basically software that you install on a computer you own. The second version is one that is called Azure Active Directory which is NOT what we are doing here. We will be setting up the on-premise version USING Azure. 
</p>
<br />

<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/6GSnCvB.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
I am going to start off by creating a Virtual Machine in Azure. We are going to be inside of a virtual network which will automatically be created when we are in the VM. When you create a VM in Azure, a bunch of other things get created and one of the things that get created is what is called a NIC or Network Interface Card where IP addresses are assigned to it. We are also going to create a Domain Controller which will also have a NIC. We know that when computers get IP addresses in two ways. One of those is through a DHCP where the home network serving as a DHCP assigns an IP. The same thing happens in Azure where both a DHCP and DNS server hidden on the Azure server. When we create our DC we will get automatically get an IP from the VP. Usually when you have some type of server offering services such as in our case with the DC, we dont want the IP address changing or being "Dynamic". So once we create our Domain Controller we will change the settings from Dymaic to Static. We will also be creating our "Client-1" to have Windows 10 and we will be joining it to the DC. We know that whenever we create a VM in Azure, that VM will automatically use the DNS built into Azure. We will be configuring the DNS on our "Client-1" to use the DNS of our Domain Controler instead of the one automatically assigned by Azure. The reason we want to do this is because whenever Active Directory is installed on a server and that server becomes a Domain Controller, a DNS services is also installed as well. In order for the Client to be able to join the domain, it has to use the DNS of the DC as its DNS setting. I will explain this further in the next lab repository that I post after this. 
</p>
<br />


<p>
<img src="https://i.imgur.com/96Kqktg.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/daFK83r.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/n5xzLGO.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Let us begin by creating a Virtual Machine in Microsoft Azure. We can do this by going to our Azure Portal > Virtual Machines > Create ? Azure Virtual Machine. 

</p>
<br />

<p>
<img src="https://i.imgur.com/NO4vVXk.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/uzpUQ03.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once we are in our "Create a virtual machine" page we can go ahead and create a new resource group and name it whatever you want. I named mine "AD-Lab". For image we want to use Windows Server 2022 as that is going to act as our Domain Controller. We can go ahead and name the VM "DC-1", set security to trusted and set our region to whatever we want. I used West US 3. Remember, it is VERY important that both VMs that we create are set to the same region otherwise this will not work and you will have to do it all over again. Where it says "size" I selected "standard_E2s_v3 - 2 vcpus, 16 GiB memory" as it has two CPUs just to speed things up. I also created a user named "labuser" and gave it a password which can be anything you want, just dont forget what it is!. Go ahead and click Review+Create, let it validate, and click create. Now we can move on to setting up our Client-1 VM. 
</p>
<br />


<p>
<img src="https://i.imgur.com/jJRYsdN.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Following the same steps as we did for creating our DC-1, I went ahead and set up "Client-1" which will act as our user desktop. Instead of using Windows Server, we are going to set the image to use Windows 10 and remember to place it into the same resource group and region as our DC-1 VM. Once again, set up the same username and password and create. 
</p>
<br />


<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/h73ORUc.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/u8dNpDY.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/hdhLo0V.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that both our domain controller and client are set up we can go ahead and change the private IP of DC-1 from dynamic to static. We can do this by going to DC-1 from our virtual-machines page and clicking "Networking" on the lefthand side. We should see a something that says "Networking Interface" with the name of our domain controller next to it. Yours might appear different from mine but just look at the above image for reference. We can then click "IP Configurations" on the left and then click on the bottom where it says "Private IP Address" and a new window will open up. Go to where it says "Assignment" and change it from Dynamic to Static. Click save. See above photo for reference. 
</p>
<br />


<p>
<img src="https://i.imgur.com/VLcvgQu.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Xgy9tfG.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Let us log into Client-1 using remote desktop connection from your actual computer. You can find this by going to your windows search bar and typing in "Remote Desktop Connection". We can do this by going to Client-1 and copying the Public IP address into Remote Desktop and loging in as the account that we created (labuser). 
</p>
<br />


<p>
<img src="https://i.imgur.com/iBLZvmU.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
From here we can go ahead and try to ping DC-1's private IP address. In order to get DC-1's private IP address we can simply just click on DC-1 from our VMs in our Azure portal and go down to where it says "Private IP Address". For me its 10.0.0.4 so just go ahead and copy whatever it shows for you. We can then go back into our Client-1 VM on remote desktop connection and opening our command promt. We can do this by going to our search bar in our start menu and typing "cmd". We can then try to do a perpetual ping of DC-1's private IP by typing "ping -t 10.0.0.4" and pressing enter. We should see that it now continuously says "Request timed out". This is because DC-1's Windows firewall is blocking ICMPv4 traffic. Our next step is going to be enabling it. 
</p>
<br />


<p>
<img src="https://i.imgur.com/vC28MS4.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Using the same steps we did to log into Client-1 Remote Desktop Connection, we can do the same for DC-1. Go back to your Azure portal on your actual desktop, go to DC-1, copy the public IP Address, go to remote desktop connection and log in. Once insdie of DC-1 we should see something like the above image. 
</p>
<br />


<p>
<img src="https://i.imgur.com/p5XQWr5.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/7aYiVyp.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
We can go ahead and access the firewall by going to our windows search bar and either typing "firewall" and selecting "Windows Defender Firewall with Advanced Security on Local COmputer" OR typing in "wf.msc" which will take you to the same screen. From there we can go ahead and click on "Inbound Rules" on the left hand side and then sort the menu by "Protocal". We want to go ahead and find all the ICMPv4 which is what ping uses and enable the three selections shown in the photo above. We can either right click each one and press "enable" or just do it from the righthand side menu. 
</p>
<br />


<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />


<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />



<p>
<img src="https://i.imgur.com/saZR1Y8.jpg" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

