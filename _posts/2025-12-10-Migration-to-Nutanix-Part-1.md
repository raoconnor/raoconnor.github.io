# Migration to Nutanix - Part 1

Moving workloads either to cloud or between hypervisors comes at a cost, change planning, downtime and administrator effort. 
I set up Nutanix Move in my home lab to test some migration scenarios, I’ll start with simple modern systems and work down to move complex, older systems in later posts

The homelab setup is simple AHV on the smaller NUC and VMware Workstation on larger machine
![move](https://raoconnor.github.io/docs/assets/images/homelab.png)


## Download and Deploy a Move Appliance
http://portal.nutanix.com/page/downloads?product=move 

As I am running AHV is downloaded the Move ZIP file for AHV
![move](https://raoconnor.github.io/docs/assets/images/move-1.png)


Unzip the move qcow2 file 
From Prism Central - > Settings and Images upload as DISK with Bus Type as SCSI
![move](https://raoconnor.github.io/docs/assets/images/move-2.png)

Wait for the image to upload and turn ACTIVE
![move](https://raoconnor.github.io/docs/assets/images/move-3.png)

Create a new VM, cloning the image to create a new disk
![move](https://raoconnor.github.io/docs/assets/images/move-4.png)

![move](https://raoconnor.github.io/docs/assets/images/move-5.png)

![move](https://raoconnor.github.io/docs/assets/images/move-6.png)

![move](https://raoconnor.github.io/docs/assets/images/move-7.png)

![move](https://raoconnor.github.io/docs/assets/images/move-8.png)

![move](https://raoconnor.github.io/docs/assets/images/move-9.png)

![move](https://raoconnor.github.io/docs/assets/images/move-10.png)


		

