# Migration to Nutanix - Part 4


In **Part 1** I deployed the Nutanix Move appliance, in **Part Two** I tested a migration of a Windows 2019 Server from ESX to AHV. 
In **Part 3** I tested migration test of Windows 2012 and Windows 2008 R2.


In this lab I am going to migrate some older Linux systems, these are some of the oldest supported versions for each distribution 

•	Ubuntu Linux 18 (64 bit)\
•	Red Hat Enterprise Linux 7.7 (64 bit)\
•	SUSE Linux Enterprise 12 SP3 (64 bit)\
•	Rocky 8.10\
•	Centos 7.0\


See Nutanix documentation for supported systems\
https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v6_1:top-esxi-vm-migration-c.html


**Note:**
Tests are limted to systems that use BIOS firmware, Nutanix CE has issues with UEFI, licenced or NFR versions of Prism do not have the same limitation.

### Test Lab

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/Homelab2.png" width="500"></kbd>

### Create New Migration Plan

From the Move Console create a New Migration Plan

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-0.png" width="800"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-1.png" width="450"></kbd>

### Source and Target

Select ESX as source and Nutanix cluster as target

### Select VMs

Select the Linux VMs (Note none have warning advising that migration is not supported)

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-2.png" width="800"></kbd>

Note: As Rocky Linux 8 is not supported on ESX, move is unable to migrate it

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-3.png" width="300"></kbd>

### Network
 
Select the target Network

### Prepare VMs

Choose manual preparation as these are legacy systems, and we want to be sure the VirtIO drivers are installed prior to cutover

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-4.png" width="800"></kbd>


Copy the provided preparation script - Note your move ip address will be embedded\
 ```curl -sSk https://**<Move IP>**/resources/uvm/linux/esx_setup_uvm.sh | sudo sh /dev/stdin --move-address ’**Move IP**’ /--retain-ip --install-amd --reconfig-lvm --uninstall-vmware-tools --install-ngt ```

 <kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-plan-5.png" width="400"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/ubuntu-1.png" width="700"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/rhel-1.png" width="700"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/sles-1.png" width="700"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/centos-1.png" width="700"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/centos-2.png" width="700"></kbd>

The basic issue is that there are no active repositories for Centos 7
While there is a vault archive, and theoretically its possible to point to it as the repository, it highlights the complexity of migrating legacy systems

### VM Settings

Keep as default

### Summary

Check the details and start the migration

The various stages of migration are indicated though View Details 

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-1.png" width="900"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-2.png" width="900"></kbd>

Once data is seeded the option to cutover will appear

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-3.png" width="900"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-4.png" width="300"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-5.png" width="900"></kbd>

<kbd><img src="https://raoconnor.github.io/docs/assets/images/movelab4/linux-migrate-6.png" width="1100"></kbd>




















