# How-to-Install-and-Configure-PfSense-on-VMware
As my home firewall, pfSense has been running for a while. For many, the hardest part would be setting everything up for the first time. But once it's up and going, it performs flawlessly. Because it is free and open source, you can create a firewall system for nothing by utilizing an old computer as the firewall.
Objective.
On the LAN side of the firewall, we'll connect two boxes, install pfSense with WAN and LAN interfaces, and test both of their internet access. Block internet access from one of the LAN hosts after confirming internet access, so let's get started.
Prerequisite.
You need to have VMware workstation pro from here, https://www.vmware.com/products/workstation-pro.html 
You need also to download Pfsense from here, https://www.pfsense.org/download/ , make sure you choose AMD64, DVD image (iso) installer.
![image](https://user-images.githubusercontent.com/123830641/236872699-e98818da-52cd-4158-80aa-06508aa6ef45.png)
Steps to install pfSense on VMware.
1. Configure VMware workstation network for pfSense
We require two interfaces for pfSense, one for the WAN and the other for the LAN, for the WAN interfaces you need to have internet access. And the LAN side will act as a gateway for the LAN users. We need to configure these two interfaces on the VMware workstation first.
We are going to configure as follows.

WAN- We will be using the Bridge interface, which will bring the PfSense firewall WAN side to be part of the local network. Your local router will assign an IP address on the WAN side. There are times the bridge interface may not work well, and you can follow the guide here to troubleshoot the problem. As a workaround, you could use the NAT interface if the bridge interface doesn’t work.
LAN- For the LAN interface we will be using the host-only adapter and select VMnet1. By default, the VMnet1 will act as DHCP server for the Virtual machine. You need to disable the DHCP service on the VMnet1 first.
Open VMware Workstation and click on Edit > Virtual Network editor. In the Virtual Network Editor click on Change settings.

Note: You need to have admin rights, in order to change the settings.

![image](https://user-images.githubusercontent.com/123830641/236873080-d0d33fb5-8265-48b2-9c3a-261ba8ce3c12.png)

Alright, we just configured the network for the PfSense firewall in the VMware workstation, let’s go ahead and install pfSense on the VMware workstation.
2. Create PfSense Virtual machine.
We are now going to create the pfSense firewall VM, so Click on File and new virtual machine.

In the New virtual machine wizard choose Typical.

In the installer disk file image, choose the PfSense image that you have downloaded earlier and click on Next.
![image](https://user-images.githubusercontent.com/123830641/236873519-fac420f9-562f-4025-8412-d6186eb5b387.png)

By default, the VMware workstation would pick up the location where you wanted to install the pfSense as well as the name. You may leave the default location or choose a different one. And you may name the VM of your choice.

Maybe you will have a dedicated drive just for the VM installation, so you need to make sure you choose that specific drive here, otherwise, it is okay to leave the default.

![image](https://user-images.githubusercontent.com/123830641/236873850-bed42ed2-c0e7-445b-889a-be760fca581f.png)


3. Setup the pfSense VM hard disk.
Since I would be using pfSense firewall VM for the LAB purpose, I will configure the Hard Disk as the default value 20GB and choose split virtual disk into multiple files and click on Next

![image](https://user-images.githubusercontent.com/123830641/236874184-c524788c-15ab-4274-95d5-e828b07e5c93.png)


4. Assign the VM resources.
Before you click on next, you need to click on the customize hardware option here.

![image](https://user-images.githubusercontent.com/123830641/236874328-fec6b490-3015-4741-a6bf-76a52c10bfad.png)


First change the default RAM size to 2048MB.

CPU – 2

Note: The PfSense VM will work just fine with 1024MB memory and 1CPU as well.

I configure the RAM and the CPU next lets go ahead and add the Network interfaces.

Connect two network interfaces that you configured earlier.

The first interface is already configured as NAT by default, we will change it to use it as a bridge interface that connects to the WAN, and I will add the second interface for the LAN.

Attach the WAN interface.
Select the network interface that is configured as NAT, and change it to Bridge interface

Attach the LAN interface.
Currently, the VM has only one network interface. We need to attach the LAN interface by clicking Add. The Add Hardware wizard now open.

Choose network adapter and Finish.

![image](https://user-images.githubusercontent.com/123830641/236874530-dbe91d4f-d8e4-481f-b4e0-17145c664481.png)


![image](https://user-images.githubusercontent.com/123830641/236874724-a72c1207-fedc-41f4-846e-e13fc8408bfc.png)

Make sure you check the option which says, Power on this Virtual machine after creation. Click on Finish on the New virtual machine wizard.


![image](https://user-images.githubusercontent.com/123830641/236874868-2dd3589d-d113-49e4-a351-6cbcac418238.png)



5. PfSense installation.
The pfsense installation now will begin. You can accept the copyright notice.

![image](https://user-images.githubusercontent.com/123830641/236876096-5b35d33f-571c-4080-8225-f6ed505f3b51.png)


Click on Install on the pfSense installer welcome screen.


![image](https://user-images.githubusercontent.com/123830641/236876182-9754f703-d0ad-40b4-981c-a3ed793c1b2e.png)


You may choose the keymap of your choice, I am leaving the default one.

![image](https://user-images.githubusercontent.com/123830641/236876270-a793e861-e077-45ea-a5be-d6033b45a06a.png)

In the partitioning step, select Auto (UFS) BIOS and click on OK.

![image](https://user-images.githubusercontent.com/123830641/236876382-26293a15-e68b-4bea-9587-824dca31bba9.png)



After few seconds the installation will now be completed, and you may now go there is nothing much here, you can click on No.

![image](https://user-images.githubusercontent.com/123830641/236876587-1dc7a3be-0001-42f2-b8cb-91a4e86805b6.png)

And go ahead and reboot the firewall.


![image](https://user-images.githubusercontent.com/123830641/236876699-6ec96ce5-f082-405f-882e-9c90bc7a38f1.png)


After the reboot, you will be presented with the below screen, as you can see below the WAN interface got the DHCP IP address from my local network 192.168.1.10/24 and the LAN is configured with the default IP 10.0.0.1  you can set interfaces IP address like that from option 2

![image](https://user-images.githubusercontent.com/123830641/236877366-8f37c9d0-015f-4a93-b410-f48a9b301007.png)


6. Setup the client machine.
I have a Centos 8 and Linux mint configured in the VMware workstation; I will be using it as a client machine to test the end user connectivity on the PfSense LAN side.

Remember we have configured PfSense LAN side interface as Host-only network, go to the client operating system in VMware workstation and right-click on it and click on settings, add client VM to be part of Host-only network.

![image](https://user-images.githubusercontent.com/123830641/236877894-3c718bb4-18b1-441f-b909-5059546487a0.png)

Once the LAN side of the PfSense connected to the client operating systems, it should start getting IP addresses from the PfSense DHCP server on the LAN.

Verifying the connection between Pfsense and client machine 

![image](https://user-images.githubusercontent.com/123830641/236878174-f195db1d-9aef-4e23-b916-82ad19fd7f1f.png)


got the first IP from the range 10.0.0.2 

![image](https://user-images.githubusercontent.com/123830641/236878335-2ecf54c7-176f-4367-8905-c9e7824a681a.png)

7. pfSense initial setup.
As we have the IP address on both the Centos and the Linux mint, you now should be able to access the pfSense web GUI from either of the machine by typing the URL https://10.0.0.1

You would get a security warning, ignore that and click on continue.

You will be prompted to enter the credentials; the username is admin and the password is pfsense and click on Sign in.


![image](https://user-images.githubusercontent.com/123830641/236878538-ecd0e3bf-cb96-4b5e-8004-0bc4f3c8c850.png)

8. Install VMware tools in pfSense.
When you are using the any operating system on VMware workstation, it is recomeded that you install VMware tools to get best performance.  It is no different for the pfsense.

Click on system and package manager.

In the package manager, click on Available packages and search for VMware.

You should be able to see Open-VM-Tools appeared, click on Install.

When you get a prompt click on confirm under package installer.

Now we logged into pfsense admin page from client machine and we configure the wizrd , i left hostname as a deafult (pfsense) and domain like (Site.com) , primary DNS 8.8.8.8 and secondary 8.8.4.4 for google.com 

![image](https://user-images.githubusercontent.com/123830641/236883427-c538c9d8-c475-4213-811c-32f64a2d99d0.png)


Now hit next and set time zone , i set ,y current time zone and hit next

![image](https://user-images.githubusercontent.com/123830641/236883771-5bf2c439-3ad8-4b5f-a768-9b2d678b8ac4.png)

Configure -wan-interface , u can choose DHCP-Static then go to into Static IP Configuration , and set ip hit next and to go Configure LAN Interface


![image](https://user-images.githubusercontent.com/123830641/236884463-26a0b7e2-60be-484f-9393-905ce73b686e.png)


Configure LAN Interface - and hit next
 
 ![image](https://user-images.githubusercontent.com/123830641/236884623-fee3526e-df53-46ec-afaf-3ad73051767a.png)

Now Set admin GUI Password and hit next 

![image](https://user-images.githubusercontent.com/123830641/236884926-34fcb4c7-1a78-4349-93c9-bf8a3189864a.png)



9 - Here is dashboard after finished 

![image](https://user-images.githubusercontent.com/123830641/236934032-fad5b504-3945-4b0f-8826-8ec638b8f4f1.png)


Next Step we need to add new interfaces for Domain Controller and File Server or DMZ . First you need to add adapter VM -Settings-Add network Adapter 

![image](https://user-images.githubusercontent.com/123830641/236948697-9da81869-f7dd-4659-8e8f-200cc03385bf.png)







