## vExpert Maxtang Home Lab


Many in the vExpert community received a Maxtang nuc at VMware Explore, here is my experience in setting up the Maxtang

 
### 1. Get the right SSD
So I bought a TB of the wrong SSD on Black Friday - Samsung PCIe NVMe (2280) it fitted but was not recognized.
Fortunately the folks at Amazon accepted a return and I replaced it with Transend MTS420S M.2 SATA III, the important factor is the M2 should be SATA II
If you are not sure of the difference SATA M.2 SSD has two gaps in the interface NVMe M.2 SSD has one gap


### 2. Memory 
I used a Crucial RAM CT2K16G4SFRA266 32GB (2x16GB) DDR4 2666MHz CL19 Kit that was in my Intel NUC and upgraded the Intel NUC to 64 GB
The memory was recognized without issue


### 3. Installing ESX. 
I was able to install ESX 7.03g  from a custom image with the Community Network Driver and the USB Network Native Driver for ESXi, However the install fails at 81% with Exception: No vmknic tagged for management was found. There is a workaround, to remove the installation media reboot the system, and Restore the Network, however I found on host reboot the static address was lost.
I tried recreated the image, but could not fix the issue.


### 4. Networks. 
As the built-in NICs are not recognized, I was having to use USB nics, I tried using two USB NICs but this led to other issues.

 
### 5. Finally 
I gave up and installed Ubuntu and VMware Workstation as that allows me to use the two built-in network ports and simulate complex environments

Big thanks to VMware and Cohesity as this gave an excuse to rebuild my homelab environment, I’ve already tried out vSAN and installed ESXi 8, next on the list is NSX-T, and TKG, so I'm currently trying to understand how to simulate some complex multisite network senarios.

In summary I read recently that you don’t really know something well until you break it, which is what home labs are all about, getting from objective to up and running and understanding what breaks in between.
