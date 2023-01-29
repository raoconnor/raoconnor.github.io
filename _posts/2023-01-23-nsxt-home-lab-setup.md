# NSX-T Homelab

Setting up a homelab to run NSX-T is probably not the best place to start if you are just rebuilding a homelab after a couple of years of hiatus in the cloud.

Basic requirments, dns, shared storage, vCenter, ESX cluster of two or three hosts, lots of RAM (12 GB for the VCSA, 16 GB for a single NSX-T Manager, and 4GB each for the two edge nodes) and sufficent CPU.

In my case I had a Intel NUC8BEB with a i5-8259U CPU which gives me up to 3.80 Ghz and 64 GB of RAM and a Maxtang NX6412 with a Celeron J6412 which provides 2.60GHZ and 32GB of RAM. 

I use the NUC as my home PC, it has windows 11 installed and after having some issue with installing ESXi 7 on the Maxtang of decided to go down the VMware workstation route for both systems. 

![This was the plan] (https://raoconnor.github.io/docs/assets/images/homelab-nw1.png)

