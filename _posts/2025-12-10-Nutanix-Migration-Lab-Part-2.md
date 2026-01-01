# Nutanix  Migration Lab - Part 2


In part 1 I deployed a Move appliance, and added some source environments. 
In this post I will migrate a single workload to AHV to test the migration patern

## Validate Source and Target Enviroments

I previously I added two source environments (ESX and Hyper-V) and the AHV cluster as target

On each Enviroment clicking on the three dots will show the Environment Details and VMs that can be moved

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-30.png" width="700"></kbd>

### Source - esx 

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-32.png" width="700"></kbd>

### Source - hyper-v

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-17a.png" width="700"></kbd>

### Target - AVH

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-31.png" width="700"></kbd>

**Warning:**
The standalone free version of ESX has restrictions on allowing Move to autheticate to VMs.
Hyper-V Gen 2 instances use UEFI as default not legacy BIOS, Nutanix CE has issues migrating UEFI


 ## Prerequisites Hyper-V
<ul>
<li>Guest VMs are Gen 2 with legacy BIOS.</li>
<li>Guest VMs have connectivity with Move.</li>
<li>Disable backups.</li>
<li>Guest VMs have integration services installed and an IP address is present in the Networking section of the Hyper-V manager for automatic preparation of the source VMs.</li>
<li>Inbound and outbound ports **TCP 5986 and 5985** are enabled for (WinRM) feature to work.</li>
<li>In the Hyper-V host, download move-agent-installer.exe from http://**nutanix-move-ip**/downloads/agents/move-agent-installer.exe.</li>
 <ul> 
 <li>Launch the command prompt with Run as Administrator to run the following command from C:\Users\Administrator.</li>
 <li>`move-agent-installer.exe -o [operation] -ip [move ip] -u [user]`</li>
 <li>Replace **nutanix-move-ip** with the IP address of the Move VM.</ul>
 </li>
</ul>
 
---
/  

## Create and Execute a Migration Plan

Click on create a migration plan and give the plan a name

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-34.png" width="700"></kbd>

**1.** Choose source and destination

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-35.png" width="700"></kbd>

**2.** select the instances to move

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-36.png" width="700"></kbd>

**3.** select the target network

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-37.png" width="700"></kbd>

**4.** The preparation mode 

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-38.png" width="700"></kbd>

**5.** VM Settings

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-39.png" width="700"></kbd>

Run the setup script on the VM

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-28.png" width="700"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-29.png" width="700"></kbd>

Review the summary and start

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-40.png" width="700"></kbd>

Wait for plan to validate

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-41.png" width="700"></kbd>

Plan should change to **In Progress**

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-42.png" width="700"></kbd>

In **View Details** the remaining time can be seen

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-43.png" width="700"></kbd>

Finally the plan VM status will change to **Ready to Cutover**

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-44.png" width="700"></kbd>

Open **View Details** and start the Cutover

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-45.png" width="700"></kbd>

Wait for Cutover to complete

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-46.png" width="700"></kbd>

Check the source the VM should be powered off and the network card removed

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-47.png" width="700"></kbd>

On the nutanix cluster, check the VM is running, connect to the OS, open devmgr and verify the Nutanix drivers are present

<kbd><img src="https://raoconnor.github.io/docs/assets/images/move-49.png" width="700"></kbd>



---
#MCExperts, #MCX









































