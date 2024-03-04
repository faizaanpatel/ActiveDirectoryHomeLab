<h1>Active Directory Home Lab</h1>
<img src="https://imgur.com/xlLorkv.png" height="70%" width="70%"/>

<h2>Description</h2>
In this project I will demonstrate how to create an Active Directory Environment using Oracle Virtual Box. Configuring and operating this environment will help develop an understanding of how Active directory and Windows networking works.
<br />

<h2>Languages and Utilities Used</h2>

- PowerShell
- Oracle Virtual Box

<h2>Environments Used </h2>

- Windows 10 (22H2)
- Windows Server 2019

<h2> Download Links </h2>

- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [A Windows 10 22H2 ISO](https://www.microsoft.com/en-us/software-download/windows10ISO)
- [A Windows 2019 Server ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)
- [PowerShell Script to Add Users](https://github.com/joshmadakor1/AD_PS/archive/master.zip)

<h2>Program walk-through:</h2>
<p align="left">

<h3>Downloading the ISO's and Installing VirtualBox: </h3>

<p>We can start by downloading Oracle VirtualBox and VirtualBox Extension Pack</p>

<p><img src="https://imgur.com/XWnyIFH.png" height="70%" width="70%"/> </p>

<p>This is what the application will look like if you install it on Windows: </p>

<p><img src="https://imgur.com/Lm9kNLs.png" height="70%" width="70%"/> </p>

<p>Now we can download the ISO files for Windows 10 22H2 and Windows Server 2019: </p>

<p><img src="https://imgur.com/aA6ZYEa.png" height="70%" width="70%"/> </p>
<p><img src="https://imgur.com/KCPZoLN.png" height="70%" width="70%"/> </p>
<p><img src="https://imgur.com/HvJTOoB.png" height="70%" width="70%"/> </p>
<p><img src="https://imgur.com/BQlWRGt.png" height="70%" width="70%"/> </p>

<h3>Setting up our first Virtual Machine on VirtualBox (Domain Controller) </h3>

<h4>Setting up the VM</h4>

<p>We can now go ahead and setup our Virtual Machines on VirtualBox. Go ahead and launch VirtualBox and click on "NEW" at the top of the screen so we can create our first VM which will be our Domain Administrator. We can name the VM whatever we want but I will be calling it "TEST-DC01" as that is a common nomenclature for naming servers in corporate environments: </p>

<p><img src="https://imgur.com/sV1JDwS.png" height="70%" width="70%"/> </p>

<p>You can setup the RAM, amount of Processors, and virtual hard disk size based on your machines available resources but I would recommend between 2-8 GB's of RAM, 4-8 processing cores and 20-50GB of disk space: </p>

<p><img src="https://imgur.com/oTIPYjC.png" height="70%" width="70%"/> </p>

<p><img src="https://imgur.com/OrSRi7o.png" height="70%" width="70%"/> </p>

<p>The VM should now be created but we will need to change a few settings before launching it. </p>

<p>Under General > Advanced, we can change both Shared Clipboard and Drag'N'Drop to "bidirectional": </p>

<p><img src="https://imgur.com/Wd0ebHh.png" height="70%" width="70%"/> </p>

<p>We want to create two NIC's on the machine, one to connect to the Internet and the other to connect to our local network. To do this, we can create enable Network Adapters or NIC's on the machines settings, one using NAT for the internet access and the other set to use the Internal Network: </p>

<p><img src="https://imgur.com/gBFchjW.png" height="70%" width="70%"/> </p>

<p><img src="https://imgur.com/D7ePk3A.png" height="70%" width="70%"/> </p>

<h4>Launching the VM and installing the OS</h4>

<p>With this, our VM is now ready to launch so we can install the Windows Server 2019 OS. You can launch the VM by clicking Start at the top or double clicking on the VM itself. Please ensure that you may need to enable CPU Virtuatilization on your BIOS for the VM to succesfully power up. When you launch the VM, it will say it could not find an OS and to select an ISO file to mount. Locate and select your Windows Server 2019 ISO file and mount it so that the VM can start the installation process. Make sure that you install Windows Server 2019 Standard Evaluation (Desktop Experience) so that it installs with the standard Windows GUI. </p>

<p>Eventually the OS will launch and you will reach the sign in screen. Here you can sign in using the Administrator password you setup during the installation wizard. Once you do, you will finally see the Desktop profile: </p>

<p><img src="https://imgur.com/EZo9pGI.png" height="70%" width="70%"/> </p>

<h3>Setting up our Domain Controller and Services </h3>

<h4>Renaming the computer</h4>

<p>Before doing anything else, we want to take a quick second to rename the computer to "TEST-DC01" to make things more organized. You can do this by right clicking on the start menu and clicking on System. From here, you can click on "Rename this PC" and name the computer as "TEST-DC01" and go ahead and click Restart Now. The machine will eventually reboot and the machine will be named TEST-DC01. </p>

<p><img src="https://i.imgur.com/KgWnQjA.png" height="70%" width="70%"/> </p>

<h4>Configuring the Network Adapters </h4>

<p>Now, we must setup the two NIC's on our Domain Controller VM. On your VM, click on the network icon on the bottom right of your screen and then click on "Network and Internet Settings" and then click on "Change Adapter Options". You will see two NIC's listed but it will not be obvious which one is which right away. You can check the status of both and use the details provided on screen to identify  each adapter: </p> 
</br> 
- The Internet NIC will have an assigne dIP within your ISP's network </br>
- The Internal NIC will have an auto-assigned IP from VirtualBox as it will not be assigned one with your home Routers DHCP </br>
</br>
<p><img src="https://imgur.com/TxKELFo.png" height="70%" width="70%"/> </p>

<p>I would suggest renaming the two adapters to "INTERNET" and "INTERNAL" or something along those lines so that it's easier to differentiate them at a glance. Now we can see that the INTERNET adapter already has an assigned IP and DNS server but we will need to manually configure the networking configuration for the INTERNAL adapter. We will apply the following settings for it: </p>
</br> 
- We can assigned the IP Address as 172.16.0.1 </br>
- We can assigned the Subnet Mask as 255.255.255.0 </br>
- We do not need to setup the Default Gateway as the Domain Controller will act as it's own gateway </br>
- We can assigned the DNS server as 127.0.0.1 (which you may notice is the loopback address as the Domain Controller will also be setup as the DNS server) </br>
</br>
<p><img src="https://imgur.com/UHgA8UX.png" height="70%" width="70%"/> </p>

<h4>Installing Active Direcory Domain Services </h4>

<p>Now that the NIC's have been setup, we can install the Active Directory Domain Services which allows us to access administrative tools and domain services on our DC to setup/configure our domain and its many users. Start by launching the Server Manager (Found in the Start Menu) and clicking on "Add Roles and Features" at the top: </p>

<p><img src="https://i.imgur.com/7qYFtCi.png" height="70%" width="70%"/> </p>

<p>This will launch a setup wizard for installing different roles and features on your server. We can ignore the first two section until we arive at Server Selection. From here, we should see our DC in the available list to select: </p>

<p><img src="https://i.imgur.com/pWacvAI.png" height="70%" width="70%"/> </p>

<p>On the next screen, select "Active Directory Domain Services" and click on "Add Feature" on the pop up that comes up. You can leave the "File and Storage Services" feature checked off since it was there by default and hit Next. Keep going through the rest of the wizard and just keep on hitting Next until you get to the final screen and you can hit Install. You will then see the feature installing: </p>

<p><img src="https://i.imgur.com/hjvSLMC.png" height="70%" width="70%"/> </p>

<p>After it's done, you can hit close to close the window. Back on the home page of Server Manager, you should see a flag icon with a caution symbol on it at the top right of the screen. If you click on this, you should see an option to "Promote this server to a domain controller" that you can click on. This option will allow us promote our server to a Domain Controller and setup our brand new domain. Go ahead and click on it. </p>

<p><img src="https://i.imgur.com/VBeoy0P.png" height="70%" width="70%"/> </p>

<p>In the setup wizard, select the option to "Add a new forest" to create our new domain. We can name the domain whatever we'd like but I will be calling it "mydomain.com" just to keep it simple. As you move along in the wizard, it will ask you to setup a DRSM password as well. Keep clicking Next until you get to the final page and then click Install: </p>

<p><img src="https://i.imgur.com/GPpsDT3.png" height="70%" width="70%"/> </p>

<p>The VM will reboot after this setup is completed. Once it comes back up, you should see that your domain name will show up infront of your Administrator account in the format MYDOMAIN\Administrator. Go ahead and sign into the administrator profile as we can now setup our official Domain Administrator account. </p>
  
<p>Go ahead and open your start menu, click on the Windows Administrative Tools folder and then click on Active Directory Users and Computers. This will open the AD UI that will allow us to create user accounts and other such objects in our domain. We are going to start by creating a new Organizational Unit for our Admins. You can think of OU's almost like folders that contain a set of objects. We are going to name the new OU "ADMINS". To add it, right click on your domain, hover over "New" and then click "Organizational Unit": </p>

<p><img src="https://i.imgur.com/v2HG2ZJ.png" height="70%" width="70%"/> </p>
<p><img src="https://i.imgur.com/WXulv0q.png" height="70%" width="70%"/> </p>

<p>Now that the OU has been made, you can double click on it to open it. Within the OU, we can right click on any blank space and hover over New and then click on User. We can name the admin account whatever we'd like but I'll be naming it "admin.fpatel" which is a common nomenclature for admin accounts within corporate environemnts: </p>

<p><img src="https://i.imgur.com/D7SWwSP.png" height="70%" width="70%"/> </p>

<p>Once you finish creating the object and setting up the password, we will need to add this account to the "Domain Admins" group. The Domain Admins group comes built into every new domain and gives the user administrative priveleges. To do this, we can right click on the new user and click Properties. From here, click on the "Members of" tab up top and hit "Add". Here, you can search for Domain Admins and hit enter to populate and click OK to confirm the change. We can now sign out of the default administrator account and sign into our Domain Admin account. </p>

<h4>Configuring RAS/NAT on our Domain Controller </h4>

<p>The RAS/NAT service allows client machines to access the Internet through our DC while still being connected to our private local network. To get this service installed, we can go back to the Server Manager. From here, click on "Add Roles and Features" again like in previous steps. Keep moving through the screens until you arrive at the Server Roles page and then click on the "Remote Access" checkbox and click "Add Feature" on the pop-up. Keep going through the screens until you arrive at the Roles Service page and then click on the "Routing" checkbox, this will automatically select the DirectAccess and VPN (RAS) service for you. Click Next until the final page and then hit Install: </p>

<p><img src="https://i.imgur.com/D1Sl6cM.png" height="70%" width="70%"/> </p>

<p>Once this is finished installing, we can head back to the home page. From here, click on Tools on the top right and then click on Routing and Remote Access from the drop-down menu: </p>

<p><img src="https://i.imgur.com/8XjfH8G.png" height="70%" width="70%"/> </p>

<p>On this screen, you should see your DC listed on the left pane. Right click on it and click "Configure and Enable Routing and Remote Access". On the wizard, hit next on the introduction page and select NAT on the next page. On the next page, you should see both of your NIC's listed. Make sure the option "Use this public interface to connect to the Internet:" and that your INTERNET network adapter is selected on the list. </p>

<p><img src="https://i.imgur.com/jRcIIWa.png" height="70%" width="70%"/> </p>
  
<p>Hit next and then hit Finish on the final screen. Back on the Routing and Remote Access home page, you should now see a green arrow beside your domain and that'll indicate that your RAS/NAT is configured: </p>

<p><img src="https://i.imgur.com/3VH65cD.png" height="70%" width="70%"/> </p>

<h4>Configuring the DHCP Server on our DC</h4>

<p>Next, we'll want to configure the DHCP service on our DC so that client machines can lease an IP from our network. Head back to Server Manager and click on "Add Roles and Features" once again. Keep clicking next until you arrive to the Server Roles page and then click on the "DHCP Server" checkbox, click "Add Feature on the popup, and hit next. Keep hitting next until you arrive on the final page and the hit Install: </p>

<p><img src="https://i.imgur.com/gXICt2N.png" height="70%" width="70%"/> </p>

<p>Once you're back on the home page, navigate to Tools on the top right and then select "DHCP" from the drop-down menu: </p>

<p><img src="https://i.imgur.com/atyifcs.png" height="70%" width="70%"/> </p>

<p>We are looking to define a scope which is a range or pool of available IP Addresses within our subnet. On the DHCP page, we can expand the menu from our domain in the left panel and see a section for both IPv4 and IPv6. </p>

<p><img src="https://i.imgur.com/NS7UzO0.png" height="70%" width="70%"/> </p>

<p>We want to expand the section for IPv4 and then right click on it and select "New Scope". Click next and then fill the name/description of the scope with whatever you'd like. I am going to be using the range 172.16.0.100-200 for the purposes of this lab so I'll be naming the scope accordingly: </p>

<p><img src="https://i.imgur.com/bSfyfPg.png" height="70%" width="70%"/> </p>

<p>On the next screen, fill in the range as discussed and the length/subnet mask: </p>

<p><img src="https://i.imgur.com/OgwqHJv.png" height="70%" width="70%"/> </p>

<p>We can ignore the next page as we do not need to add any exclusions to add our range. The next page will ask to setup a lease length which we can leave as default for our purposes. On the next page, we want to select "Yes, I want to configure these options now" and hit next. On the next page, we can setup the Router/Default Gateway for the DHCP server. Since the DC will also be acting as the default gateway, we can just add its own IP Address: </p>

<p><img src="https://i.imgur.com/13deki4.png" height="70%" width="70%"/> </p>

<p>On the next screen, it will ask to specify the parent domain that clients will use for DNS name resolution and DNS servers. We can keep the default settings here and move on. At this point, we can keep clicking next till we reach the final page and can hit Finish. After that we will end up on the home page for the DHCP service. To finalize this setup, we need to right click on our Domain and hit "Authorize" and then "Refresh:. You should then see green arrows beside the IPv4 and IPv6 sections indicating that it's configured: </p>

<p><img src="https://i.imgur.com/dQpZY3h.png" height="70%" width="70%"/> </p>
<p><img src="https://i.imgur.com/wWplMe5.png" height="70%" width="70%"/> </p>

<h3>Adding 1000+ user accounts to AD via PowerShell</h3>

<p> Next, we want to populate our Domain with user accounts. To accomplish this, we will be using a custom Powershell script and a list of randomly generated names.</p>

<h4>Disabling Enhanced Security Configuration for IE </h4>

<p>Before we do that, we'll want to make one quick change to make our browsing experience on the DC easier. Head to Server Manager and click on "Configure this Local Server" at the top of the screen. On the right side of the screen, you should see the setting for "IE Enhanced Security Configuration". Click on it and disable it for both Adminsitrators and Users: </p>

<p><img src="https://i.imgur.com/t4CvF97.png" height="70%" width="70%"/> </p>

<h4>Downloading the PowerShell Script </h4>

<p>Now on the DC, we can open up Internet Explorer and you can use the link at the beginning of this guide to download the script. You can go ahead and save it to your Desktop and extract the folder within. The folder contains a text file with a 1000 randomly generated names and also the script that will create accounts based on the list and add them to AD. I would suggest adding your name to the top of the text file so a regular account can be made for yourself as well. </p>

<h4>Running and executing the PowerShell script </h4>

<p>Now to run the script, we first need to launch PowerShell ISE as an administrator which can be done from the Start Menu. Within PowerShell itself, you cango to File > Open and then navigate to the script we downloaded on your desktop to open it within PowerShell. Before we can actually run the script, we need to configure PowerShell to allow all scripts. This can be done by entering the following command into the PowerShell Console and selecting "Yes to all" on its popup: </p>

<p><i>Set-ExecutionPolicy Unrestricted</i> </p>

<p><img src="https://i.imgur.com/WV4zppx.png" height="70%" width="70%"/> </p>

<p>One more thing we must do before running the script is to navigate PowerShell's console to the location where our script is stored. Assuming you saved the folder to your desktop, you can run the following command: </p>

<p><i>cd C:\users\$username\desktop\AD_PS-master</i> ($Username being whatever your domain admin username is) </p>
<p>You can confirm that you are in the right location by inputting "ls" into the console and seeing the listed contents </p>

<p><img src="https://i.imgur.com/kk1dtk3.png" height="70%" width="70%"/> </p>

<p>Now that we are in the right directory, we can run the script by pressing the green arrow located at the top of PowerShell. On the dialogue box, click on "Run Once" and then you will start seeing the console populate with confirmations whenever a new user is created from the list: </p>

<p><img src="https://i.imgur.com/TTRrzYR.png" height="70%" width="70%"/> </p>

<p>Once the script is finished running, you can check Active Directory Users and Computers for the new _USERS OU and within it for all the new user accounts. The script will set them all up with the same common password "Password1" and their usernames will be formed with their first initial followed by their last name, the common nomenclature for usernames. </p>

<p><img src="https://i.imgur.com/9KouVej.png" height="70%" width="70%"/> </p>

<h3>Setting up our second Virtual Machine on VirtualBox (Client) </h3>

<h4>Setting up the VM</h4>

<p>Now for the final step, we'll need to setup a client VM (for this lab we will name it CLIENT1) that will connect to our private network and domain. You can start by heading back to your host machine (Keep your DC running but minimized) and launching VirtualBox. Go through the process of creating a new VM just like with the DC including the same configurations. We will also need to change the settings on it before launch. </p>

<p>Under General > Advanced, we can change both Shared Clipboard and Drag'N'Drop to "bidirectional": </p>

<p><img src="https://i.imgur.com/4pUvKnk.png" height="70%" width="70%"/> </p>

<p>This time, we only need one Network Adapter that's set to Internal Network: </p>

<p><img src="https://i.imgur.com/3Qk5XwW.png" height="70%" width="70%"/> </p>

<h4>Launching the VM and installing the OS</h4>

<p>We can now start our CLIENT1 VM to install the OS. Once again, the VM will ask you to mount an ISO file on launch as there will not be one installed. Here we can select our Windows 10 22H2 ISO file from earlier.  </p>

<p>Go through the standard Windows OS setup procedure until you get to the point that asks you to enter a product key, here we want to select "I don't have a product key" located at the bottom of the screen. On the next screen, ensure that we install Windows 10 PRO as other editions might not allow you to connect to a domain. </p>
  
<p>Keep going through the installation until it asks you to join a network. Here we want to select the option "I don't have Internet" at the bottom left and then "Continue with limited setup" so that Windows will let us setup a local account. </p>

<p>Setup your local account with whatever credentials you'd like. I'd suggest disabling as many options that Windows tries to throw at you as you finish the setup. Eventually you will hit the desktop. We want to sign in and launch CMD so we can run the "ipconfig /all" command and see if our networking setup is correctly configured: </p>

<p><img src="https://i.imgur.com/yAAvsvz.png" height="70%" width="70%"/> </p>

<p>As you can see from the screenshot, our machine was assigned the first available IP in our DHCP Scope and the Default Gateway, DHCP, and DNS servers are all set to our Domain Controller. This is because the Network Adapter for this machine was set to our internal network which has now been configured via our DC. </p>

<h3>Joining our client machine to our Domain</h3>

<h4>Joining the domain</h4>

<p>Now that everything is setup, we can join our test client to the domain to see if it's all working right. To do this, right click on your Start Menu and select System. From here, scroll to the bottom of the screen and click on "Rename this PC (Advanced)" as this will allow us to rename our computer while joining the domain. Click on Change near the bottom of the window to open up the settings page that will allow us to join the domain. Fill in the following information into the screen: </p>
</br> 
- Computer Name: CLIENT1 </br>
- Domain: mydomain.com </br>
</br>
<p><img src="https://i.imgur.com/3UlDJU4.png" height="70%" width="70%"/> </p>

<p>It will now ask you to enter a username and password that can join the domain, you must enter your Domain Admin credentials here. It will start loading and eventually it will tell you that the machine succesfully joined the domain and that the computer now needs to restart. Go ahead and restart the machine. </p>

<h4>Testing to see if everything is setup right</h4>

<p>When your machine finally comes back up, you should notice the option to sign in as an "other user" - this indicates that you can sign in with any user account thats setup on the Active Directory. Go ahead and test this out by signing in with your regular domain account: </p>

<p><img src="https://i.imgur.com/buWXq7B.png" height="70%" width="70%"/> </p>

<p>It should now sign you into your domain profile and place you on your new desktop. We can now check to see that everything was joined correctly with a couple of methods.</p>

<p>On the DC, we can check Active Directory Users and Computers to see if our Client machine shows up: </p>
<p><img src="https://i.imgur.com/zUp7Y0C.png" height="70%" width="70%"/> </p>
<p>Another thing we can check on the DC is the DHCP to see if our Client machine shows up in the leased IP Addresses section: </p>
<p><img src="https://i.imgur.com/QOBd7OT.png" height="70%" width="70%"/> </p>
<p>Finally, on our Client machine, we can launch CMD and run "hostname" and "whoami" to ensure our identity is correct: </p>
<p><img src="https://i.imgur.com/esuBnYi.png" height="70%" width="70%"/> </p>

<h2>Conclusion </h2>

<p>With this, our lab is concluded and we have successfully setup our Active Directory home lab and simulated a corporate environment! </p>









