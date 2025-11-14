# If not VMware, then where?  - Window Server License Costs

### You have maybe heard the claim that public cloud is more expensive than on premises

If you are looking at alternatives, make sure you have agonistically calculated the numbers on what it’ll cost to move and run your workloads elsewhere. Otherwise, you might end up trading one headache for another — jumping out of the frying pan and straight into the fire.

## Public Cloud 
Calculating the future run costs of public cloud is open to myriads of tweaks, assumptions that can throw budgets way off track.
I want to zero in on a couple of really important assumptions teams commonly make when moving to the cloud — and how those can seriously shift the cost equation.

## Windows Server Licences 
Microsoft terms and conditions only allow Windows Server Licences with software assurance to be transferred to Azure, not AWS, GCP or OCI, for those providers you will need to either repurchase the licences under a cloud subscription model or go to dedicated hardware.
As an example check out the comparative pricing for a 4 core 16 GB memory instance –the AWS Linux image has no operating system cost and is a baseline for compute capacity, comparing that with windows licence included and the price more than doubles. A similar scenario plays out in Azure, Hybrid Benefit - which removes the cost of the licence, compared the licence included, and the increase is more than double.

###Be careful of any TCO model that estimates Windows servers as AWS Linux or assumes Hybrid benefit when you don’t have software assurance.

 ![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/windows-cost-compare.png)

Software assurance does have a cost, a TCO model should add that on to Hybrid Benefit. Also Hybrid Benefit imposes an 8-core minimum per VM, 8 core licenses are required even if you only use 4 cores. The concern is that you will not have enough eligible Windows Server licenses for your VMs deployed in Azure.
Cores are the currency
The number of virtual cores has become the key metric in determining cost, the applies to licences on prem and in cloud, as well as cloud compute capacity. While for compute capacity the calculation is straightforward, when moving licences from on premises to cloud there are important changes.
Many enterprises purchase Microsoft and Oracle licences based on the physical cores. As stated Microsoft only allow Windows server licences with software assurance to be transferred to shared hardware on Azure, however SQL Server can be transferred to AWS if software assurance has been acquired. This can extend to other software such Oracle or Redhat licences.
Transferring physical cores licencing to cloud is not like for like, and cloud result in significant cost increase.
VMware architects are familiar with pCPU to vCPU ratios, which is the amount to virtual cores that can run “simultaneously” on a physical core, 1:4 is considered standard, 1:1 is reserved for a few very high-performance workstations, I’ve seen production systems running at 1:12 without any noticeable slowness. 
Moving to public cloud will usually requires transferring physical cores licences to virtual cores. For example, if 8 physical licence cores are applied to a host running with 1:4 ratio you may need four times the licences. 

 ![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/pCPU-vCPU-Compare.png)

In the case of virtual machines, the destination matters, first which cloud provider, then the type of hosting service
IaaS Hosting Services 
There are two hosting services for your IaaS workloads, shared tenancy / or VM – no host system admin, just select a size and deploy the instance, and dedicated hardware – you pay for the entire host assign VMs of the same family to that host, all host capacity is billed.

## Which Public Cloud
**Azure**
If you have Windows Server Datacenter edition with SA, then Azure allows simultaneous use of on-premises and in Azure for an unlimited duration when used for licensing VMs with Windows Server in shared tenancy.
Azure allows Unlimited Virtualization for Windows Server Datacenter edition; allowing you to use any number of Windows Server VMs on an Azure dedicated host if you allocate Windows Server Datacenter licenses with active Software Assurance or subscription for all the available physical cores on that Azure server. Note this negates the option above of simultaneous licence assignment.

Important note: your licencing specialist needs to get this validated from Microsoft, your cloud architecture and estimated cost will change significantly if you are not able to leverage the above conditions, this must be validated before any commitment is made.

**AWS and the others**
Dedicated hardware will often be suggested by the AWS to keep licence costs down, but keep in mind while dedicated hardware allows BYOL, the cloud hypervisor does not allow CPU oversubscription so the issues of needing more licences still applies. Additionally dedicated hardware can be a bit of an administrative headache due to fitting workloads of the same family into the physical instance, also you need to check your if your Microsoft T&Cs prevent in place upgrades.
The simplicity of shared tenancy with new Windows server subscription is attractive to many organizations, some customers don’t have Software Assurance, and those that do should include a line removing that cost as part of the TCO. 

Don’t overlook, how powering off instances over weekends and nights affects the cost of licences, if 50% of your Windows environment is test and dev and can be shut down and restarted, running those instances 30% of the time will also result in 35% savings in compute and licences. 

It’s important to keep in mind the non-Microsoft clouds will look to offset this disadvantage in other ways, such as storage, backup, support and of course discounts.

It may also be a time to evaluate Windows Server usage, a program to move applications and databases off windows cloud result in valuable savings.
In conclusion if saving money is the reason to choose public cloud, take a good look at licence costs, you may need to engage an independent consultant. 
The business case for some organizations is not compiling, and it can be these licence costs that tip the balance - there advantages to running an enterprise grade 3rd Party hypervisor that allows physical core licencing and vCPU oversubscription.

Sadly Broadcom has soured it relationship with many customers and cloud providers, Nutanix Cloud Infrastructure is considered as the closest to VMware in functionality, and hybrid-cloud capabilities. Switching Hypervisors to Nutanix AHV may be a viable alternative, allowing the advantages of physical core licensing and core oversubscription either on premises or in dedicated hardware in public cloud.

*In the future blog I’ll deep dive into SQL licencing in public cloud*



