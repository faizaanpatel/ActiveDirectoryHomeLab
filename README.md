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

<h2> What We'll Need </h2>

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

<h3>Setting up our first Virtual Machine on Virtual Box </h3>

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

<p>With this, our VM is now ready to launch so we can install the Windows Server 2019 OS. You can launch the VM by clicking Start at the top or double clicking on the VM itself. Please ensure that you may need to enable CPU Virtuatilization on your BIOS for the VM to succesfully power up. When you launch the VM, it will say it could not find an OS and to select an ISO file to mount. Locate and select your Windows Server 2019 ISO file and mount it so that the VM can start the installation process. Make sure that you install Windows Server 2019 Standard Evaluation (Desktop Experience) so that it installs with the standard Windows GUI. </p>

<p>Eventually the OS will launch and you will reach the sign in screen. Here you can sign in using the Administrator password you setup during the installation wizard. Once you do, you will finally see the Desktop profile: </p>

<p><img src="https://imgur.com/EZo9pGI.png" height="70%" width="70%"/> </p>

<h3>Setting up our Domain Controller and Services </h3>

<p>Before doing anything else, we want to take a quick second to rename the computer to "TEST-DC01" to make things more organized. You can do this by right clicking on the start menu and clicking on System. From here, you can click on "Rename this PC" and name the computer as "TEST-DC" </p>

<p>To start, we must setup the two NIC's on our Domain Controller VM. On your VM, click on the network icon on the bottom right of your screen and then click on "Network and Internet Settings" and then click on "Change Adapter Options". You will see two NIC's listed but it will not be obvious which one is which right away. You can check the status of both and use the details provided on screen to identify  each adapter: </p> 
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

