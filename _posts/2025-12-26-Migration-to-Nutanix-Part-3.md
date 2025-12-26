# Migration to Nutanix - Part 3


In part 1 I deployed the Nutanix Move appliance, in Part two I test a migration of a Windows 2019 Server from ESX to AHV. 
In this post I will test older versions of Windows: Windows 2012 and Window 2008 R2 with VMware tools installed and another without the tools.

**Warning:**
**The standalone free version of ESX has restrictions on allowing Move migrations

These are the oldest supported Windows versions if you are using recent version of Nutanix software
https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Move-v6_1:top-esxi-vm-migration-c.html

Note that versions are divided into BIOS and UEFI

**Warning:**
** Windows 2008 R2 requires VMware tools, to install the VMware tools SP 1 and KB4474419 need to be installed that 
All versions use BIOS, Nutanix CE has issues with UEFI, these are seen on the community edition, licenced or NFR do not have the same limitation.**


## Validate Source and Target Enviroments

I previously I added source and target environments (ESX and the AHV)

On each Enviroment clicking on the three dots to see VMs that can be moved

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove1.png" width="700"></kbd>

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove2.png" width="300"></kbd>

Unless we upgrade Windows 2008 R2 to SP1 we cannot migrate that instance. 

Select Source and Target 

Select VMs

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove3.png" width="700"></kbd>

Select Network

## VM Preparation
 
Manual – as these are older systems, there are a number of dependencies that we need to take care of first

The default version of PowerShell on Windows 2008 R2 needs updating to PowerShell version 4.0 or later.

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove4.png" width="400"></kbd>

We will stick with minimal supported versions rather than the latest


First download and install .NET Framework 4.5.2 or later
https://www.microsoft.com/en-us/download/details.aspx?id=42642


Next download and install Windows Management Framework (WMF) 5.1  file from Microsoft
https://www.microsoft.com/en-us/download/details.aspx?id=54616
The required file is Win7AndW2K8R2-KB3191566-x64.zip 


Once the update is installed check the Powershell version again
<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove5.png" width="400"></kbd>

Go back to the Move console, Copy and Run the Preparation script on the target 2008 server 

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove6.png" width="700"></kbd>

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove7.png" width="700"></kbd>

Window Server 2012 has PowerShell version 4.0 so no update required  

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove8.png" width="400"></kbd>

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove9.png" width="700"></kbd>

VM Settings – keep default setting

## VM Migration

Review Summary and start 

Wait for plan to validate and move to in progress, click on View Details to see the estimated remaining time

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove10.png" width="700"></kbd>
<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove11.png" width="700"></kbd>
<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove12.png" width="700"></kbd>
<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove13.png" width="700"></kbd>

From Prism element connect to each server and open device manager 

<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove14.png" width="700"></kbd>
<kbd>!<img src="https://raoconnor.github.io/docs/assets/images/winmove15.png" width="700"></kbd>

## Conclusion 

The current supported Nutanix software requires systems to be Windows 2008 SP1 with VMware tool installed, powershell 4 which requires NET Framework 4.5.2 or later

See the AVH Compatibility and Interoperability Matrix
https://portal.nutanix.com/page/compatibility-interoperability-matrix/guestos/compatibility


There is additional work for Windows 2008 Server, but the process is straightforward once the steps are understood
Typically, the VM would be checked and prepared in advance of running the migration plan 
 
