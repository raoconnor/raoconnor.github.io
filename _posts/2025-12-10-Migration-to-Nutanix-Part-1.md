# Migration to Nutanix - Part 1

Moving workloads either to cloud or between hypervisors comes at a cost, change planning, downtime and administrator effort. 
I set up Nutanix Move in my home lab to test some migration scenarios, I’ll start with simple modern systems and work down to move complex, older systems in later posts

The homelab setup is simple AHV on the smaller NUC and VMware Workstation on larger machine

<img src="https://raoconnor.github.io/docs/assets/images/homelab.png" width="500">


## Download and Deploy a Move Appliance
http://portal.nutanix.com/page/downloads?product=move 

As I am running AHV is downloaded the Move ZIP file for AHV

<img src="https://raoconnor.github.io/docs/assets/images/move-1.png" width="900">

Unzip the move qcow2 file 
From Prism Central - > Settings and Images upload as DISK with Bus Type as SCSI

<img src="https://raoconnor.github.io/docs/assets/images/move-2.png" width="900">

Wait for the image to upload and turn ACTIVE

<img src="https://raoconnor.github.io/docs/assets/images/move-3.png" width="500">

Create a new VM, cloning the image to create a new disk

<img src="https://raoconnor.github.io/docs/assets/images/move-4.png" width="500">

The next step requires DHCP is active in the environment
Power on the VM, wait for the move instance to obtain an IP, open a web browser such as chrome and navigate to the instance 
Accept the license agreement and access Move

<img src="https://raoconnor.github.io/docs/assets/images/move-6.png" width="500">

## Add Environments 
Once you have accessed the Move console Add Environments 
In my case I have two source environments, and esx server and a hyper-v server
There are some additional actions needed for both 

<img src="https://raoconnor.github.io/docs/assets/images/move-7.png" width="900">

### Add VMware ESX as Source

I am going to add single ESX host as source, first I need the IP and credentiales

<img src="https://raoconnor.github.io/docs/assets/images/move-8.png" width="900">

Then for ESX a VDDK library needs to be downloaded from Broadcom 

<img src="https://raoconnor.github.io/docs/assets/images/move-9.png" width="900">

Clicking on the message opens a dialog with the link to the library

<img src="https://raoconnor.github.io/docs/assets/images/move-10.png" width="500">


<img src="https://raoconnor.github.io/docs/assets/images/move-11.png" width="900">

Once downloaded to teh local network, upload to the Move appliance

<img src="https://raoconnor.github.io/docs/assets/images/move-12.png" width="900">

<img src="https://raoconnor.github.io/docs/assets/images/move-13.png" width="500">

<img src="https://raoconnor.github.io/docs/assets/images/move-14.png" width="500">
























