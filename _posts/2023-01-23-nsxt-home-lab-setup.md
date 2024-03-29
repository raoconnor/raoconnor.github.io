# NSX-T Homelab

Setting up a homelab to run NSX-T is probably not the best place to start if you are just rebuilding a homelab after a couple of years of hiatus in the cloud.

#### Basic requirements
DNS, shared storage, vCenter, hosts to run VCSA, NSX-T Manager, and the transport zones, each host needs at least 4 CPUs each, 12 GB of memory for the VCSA, another 16 GB for a single NSX-T Manager, and 16GB each for the two edge nodes to run the Transport Zones
*(note) I tried deploying the VCSA directly from workstation and for some wierd reason it fails on stage two)*

**NUC** 
- [x] dns vm          2 vcpus, 4 GB ram
- [x] nfs vm          2 vcpus, 4 GB ram
- [x] nested host1    4 vcpus, 24 GB ram
- [x] nested host2    4 vcpus, 24 GB ram

- [x] vcsa (host1)    2 vcpus, 12 GB ram   
- [x] msx mgr (host2) 4 vcpus, 16 GB ram   

**Maxtang**
- [x] nested host6   4 vcpus, 16 GB ram
- [x] nested host7   4 vcpus, 16 GB ram

- [x] edge1 (host6)  2 vcpus, 4 GB ram  
- [x] edge2 (host7)  2 vcpus, 4 GB ram  


#### Hardware
In my case I had a Intel NUC8BEB with a i5-8259U CPU which gives me up to 3.80 Ghz and 64 GB of RAM and a Maxtang NX6412 with a Celeron J6412 which provides 2.60GHZ and 32GB of RAM. 

I use the NUC as my home PC, it has windows 11 installed and after having some issue with installing ESXi 7 on the Maxtang of decided to go down the VMware workstation route for both systems. 


![Home lab network design v1](https://raoconnor.github.io/docs/assets/images/homelab-nw1.png)


#### Issues with network design
And while I got it to work, I had a lot of intermittent network errors, especially routing over the backbone between the two boxes with workstation virtual networks (each host only mgmt network 172.16.10.x & 172.16.20.x routed via the local pfsense to the remote pfsense over 172.16.200.x network) changes to the network required vms to be rebooted, pfsense firewalls and routing was taking a lot of my time.

#### Revised network with physical router and switch
I replaced the two pfsense virtual routers/firewalls with a physical router and switch, the router can do single vlans, but so far I vlan trunking doesn't work, I suspect the downstream workstation virtual networks (I can use pfsense for a host only vlan trunk if needed). 

![Home lab network design v2](https://raoconnor.github.io/docs/assets/images/lab-nw2.png)

Jumbo frames are limited to the switch, the router doesn't have that capacity.
***As a learning point***, use a small footprint linux (photon OS in my case) to check the networks/ vlans and jumbo frames at each step, photon requires some work on setting up a static ip and installing iputils and ping 

#### More CPU please
As you can see I was running vcsa and nsxt manager on the NUC and the edge cluster on the Maxtang, and needed more than the 6.40GHz of CPU those two processors provided, maybe directly on ESXi this would be enough





#### Conclusion 
I was only able to get as far as setting up transport zones, segments and trying to deploy the edge nodes. Still it was a good experience and I enjoyed getting my hands on NSX again.

