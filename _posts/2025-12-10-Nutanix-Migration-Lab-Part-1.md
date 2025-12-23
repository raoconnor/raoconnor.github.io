# Nutanix  Migration Lab - Part 1

Moving workloads either to cloud or between hypervisors comes at a cost, change planning, downtime and administrator effort. 
I set up Nutanix Move in my home lab to test some migration scenarios, I’ll start with simple modern systems and work down to move complex, older systems in later posts

The homelab setup is simple, AHV on the smaller NUC and VMware Workstation on the larger machine

<kbd><img src="https://raoconnor.github.io/docs/assets/images/homelab.png" width="500"></kbd>


## Download and Deploy a Move Appliance
Note: it's best practice to install Nutanix Move in the Target enviroment
http://portal.nutanix.com/page/downloads?product=move 

As I am running AHV, I downloaded the **Move ZIP file for AHV**

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-1.png" width="900"></kbd>

Unzip the move qcow2 file

Then from Prism Central - > Settings and Images upload as DISK with Bus Type as SCSI

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-2.png" width="900"></kbd>

Wait for the image to upload to turn ACTIVE

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-3.png" width="500"></kbd>

Create a new VM, cloning the image to create a new disk

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-4.png" width="500"></kbd>

The next step requires DHCP is active in the environment
Power on the VM, wait for the move instance to obtain an IP, open a web browser such as chrome and navigate to the instance 
Accept the license agreement and access Move

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-6.png" width="500"></kbd>

## Add Environments 
Once you have accessed the Move console first step is to Add Environments 
In my case I have two source environments, an esx server and a hyper-v server
There are some additional actions needed for both 

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-7.png" width="900"></kbd>

### Add VMware ESX as Source

I am going to add single ESX host as source, first I need the IP and credentiales

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-8.png" width="900"></kbd>

Then for ESX, I must download a VDDK library from Broadcom 

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-9.png" width="900"></kbd>

Clicking on the message opens a dialog with the link to the library

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-10.png" width="500"></kbd>



<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-11.png" width="700"></kbd>

> ### VDDK Library Versions
> - For ESX 6.0, and 6.5 as source the VDDK Library Version is 7.0.3.1 (for Linux)
> - For ESX 6.7, 7.0, and 8.0 as source the VDDK Library Version is 8.0.3.2 (for Linux)
>

Download the library coresonding to the ESX version

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-12.png" width="800"></kbd>

Once downloaded to the local network, upload to the Move appliance

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-13.png" width="500"></kbd>

### Add a Nutanix Cluster as Source

Now add the Nutanix Enviroment which will be the target for migrations
Note I am using the cluster external address, not the AHV address

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-14.png" width="500"></kbd>


### Add Hyper-V Source

Before adding Hyper-V 
WinRM should be configured on the Hyper-V server
Also the following inbound and outbound ports using the TCP protocol should be enabled for the Windows Remote Management (WinRM) feature to work.
- WinRM-HTTPS: 5986
- WinRM-HTTP: 5985

See https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v6_0:top-enable-winrm-hyperv-ahv-t.html

If using a local "administrator" account, disable the following Local Security policy:
Computer Configuration\Windows Settings\Security Settings\Local Policies\Security Options\User Account Control: Admin Approval Mode for the Built-in Administrator account

Or disable Windows UAC (User Account Control) - In windows search for User Account Control and set it to Never Notify


## Next Steps

Next steps are to create a migration plan and test some migrations









































