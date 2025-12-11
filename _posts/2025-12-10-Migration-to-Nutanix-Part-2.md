# Migration to Nutanix - Part 2


In part 2 I deployed a Move appliance, and added some source environments 
In this post I will migrate workloads to AHV

The move dashboard has the two source environments and the AHV cluster as target
Clicking on the three dots show the Environment Details

<img src="https://raoconnor.github.io/docs/assets/images/move-30.png" width="900">

### Source - esx 

<img src="https://raoconnor.github.io/docs/assets/images/move-32.png" width="900">

### Source - hyper-v

### Target - AVH

<img src="https://raoconnor.github.io/docs/assets/images/move-31.png" width="900">

### Notes: 
- The standalone free version of ESX has restrictions on allowing Move to autheticate to VMs, 
in later blogs I'll investigate show some workarounds. 
- Hyper-V Gen 2 instances use EFI not legacy BIOS, this can cause some issues in later blogs I'll investigate show some workarounds. 

## Prerequisites Hyper-V

•	Ensure guest VMs have connectivity with Move.
•	Ensure that the guest VMs have integration services installed and an IP address is present in the Networking section of the Hyper-V manager for automatic preparation of the source VMs.
•	Disable backups.
• Inbound and outbound ports TCP 5986 and 5985 are enabled for (WinRM) feature to work.
•	In the Hyper-V host, download move-agent-installer.exe from http://<nutanix-move-ip>/downloads/agents/move-agent-installer.exe .
•	Launch the command prompt with Run as Administrator to run the following command from C:\Users\Administrator.
  move-agent-installer.exe -o [operation] -ip [move ip] -u [user]
  Replace <nutanix-move-ip> with the IP address of the Move VM.
  
<img src="https://raoconnor.github.io/docs/assets/images/move-33.png" width="900">


<img src="https://raoconnor.github.io/docs/assets/images/move-19.png" width="900">

<img src="https://raoconnor.github.io/docs/assets/images/move-20.png" width="900">

<img src="https://raoconnor.github.io/docs/assets/images/move-21.png" width="900">

<img src="https://raoconnor.github.io/docs/assets/images/move-23.png" width="900">

<img src="https://raoconnor.github.io/docs/assets/images/move-24.png" width="900">



