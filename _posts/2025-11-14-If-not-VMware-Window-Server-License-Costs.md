# If Not VMware, Then Where?  - License Costs Part 1

### You have maybe heard the claim that public cloud is more expensive than on premises

If you are looking at alternatives to VMware, make sure you have impartially calculated the numbers on what it’ll cost to move and run your workloads elsewhere. Otherwise, you might end up trading one headache for another — jumping out of the frying pan and straight into the fire.

## Public Cloud 

Estimating future run costs in the public cloud involves countless tweaks and assumptions that can easily throw budgets off track.
I want to focus on a couple of critical assumptions teams often make when moving to the cloud — and how these can significantly impact the cost equation. I'm not saying don't go to cloud, only you need to have your eyes open and  realistic expectations.

## Windows Server Licences 

Microsoft’s terms and conditions only allow Windows Server licenses with Software Assurance to be transferred to Azure—not AWS, GCP, or OCI. For those providers, you’ll need to either repurchase the licenses under a cloud subscription model or use dedicated hardware.

For example, compare the pricing for a 4-core, 16 GB memory instance:

- The AWS Linux image has no operating system cost and serves as a baseline for compute capacity.
- When you include a Windows license, the price more than doubles.

A similar scenario occurs in Azure:

- Hybrid Benefit,  removes the license cost
- When compared to a license-included instance, results in an increase of more than double.

 ![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/windows-cost-compare1.png)

I’m ignoring dedicated hosting for the moment, and I’ll come back to it later, first lets dive into how licensing changes when moving to cloud, and how that impacts core count which is the basis for how many software licenses are purchased 

---

#### Key Warning
**Be cautious of any TCO model that estimates Windows servers as AWS Linux or assumes Hybrid Benefit when you don’t have Software Assurance**

---


## Software Assurance and Hybrid Benefit
Software Assurance does have a cost, and any TCO model should include that when applying Hybrid Benefit. Additionally, Hybrid Benefit imposes an **8-core minimum** per VM—meaning you need 8 core licenses even if you only use 4 cores. 

## Cores Are the Currency
The number of cores is the key metric for determining cost. This applies to licenses, as well as cloud compute capacity. While compute calculations are straightforward, moving licenses from on-premises to the cloud introduces important changes.

Many enterprises purchase Microsoft and Oracle licenses based on **physical cores**. As stated, Microsoft only allows Windows Server licenses with Software Assurance to be transferred to Azure. However, SQL Server can be transferred to AWS if Software Assurance is in place. This can also extend to other software such as Oracle or Red Hat licenses.

Transferring physical core licensing to the cloud is not a like-for-like process and can result in significant cost increases.

## Physical vs. Virtual Core Ratios
VMware architects are familiar with pCPU-to-vCPU ratios, which represent how many virtual cores can run “simultaneously” on a physical core.

- 1:4 is considered standard.
- 1:1 is reserved for high-performance workstations.
- I've seen ratios as high as 1:12, without noticable slowness

Moving to public cloud usually requires transferring physical core licenses to virtual cores. For example, if the license applied to a 8 core physical host running at a 1:4 ratio, you may need **four times the virtual core licenses** in the cloud.

 ![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/pCPU-vCPU-Compare.png)

## Destination Matters for Virtual Machines
When it comes to virtual machines, the destination matters — first, which cloud provider you choose, and then the type of hosting service.


## IaaS Hosting Services
There are two main hosting options for IaaS workloads:

- Shared tenancy (VM-based): No host-level administration; you simply select a size and deploy the instance.
- Dedicated hardware: You pay for the entire host and assign VMs of the same family to that host. All host capacity is billed.

## Which Public Cloud?

**Azure**
If you have Windows Server Datacenter Edition with Software Assurance (SA), Azure allows simultaneous use on-premises and in Azure for an unlimited duration when licensing VMs in shared tenancy.
Azure also offers Unlimited Virtualization for Windows Server Datacenter Edition. This means you can run any number of Windows Server VMs on an Azure dedicated host if you allocate Datacenter licenses with active SA or subscription for all physical cores on that server.
Note: Unlimited Virtualization negates the option of Simultaneous license assignment.

**Important:** Your licensing specialist must validate these conditions with Microsoft. Your cloud architecture and cost estimates can change significantly if you cannot leverage these benefits. Validate before making any commitments.

## AWS and Other Providers
AWS often suggests dedicated hardware to reduce license costs. However:

While dedicated hardware allows **BYOL (Bring Your Own License)**, the cloud hypervisor does not permit CPU oversubscription, so you may still need more licenses.
Dedicated hardware can be administratively challenging because workloads of the same family must fit into the physical instance.
Check whether your Microsoft T&Cs prevent in-place upgrades.

Shared tenancy with new Windows Server subscriptions is attractive for many organizations. Customers without SA—and those with SA—should include or remove that cost in their TCO models accordingly.

## Additional Considerations

- Don’t overlook how **powering off instances during weekends and nights** affects license costs. If 50% of your Windows environment is test/dev and can be shut down, running those instances only 30% of the time can yield up to **35% savings** in compute and licensing.
- Non-Microsoft clouds often offset licensing disadvantages with benefits like storage, backup, support, and discounts.
- It may be time to evaluate Windows Server usage. Migrating applications and databases off Windows could result in significant savings.

## Final Thoughts
If saving money is your primary reason for choosing public cloud, scrutinize license costs carefully. You may need to engage an independent consultant. For some organizations, the business case for public cloud does not hold up—and licensing costs can tip the balance.

There are advantages to running an enterprise-grade third-party hypervisor that supports **physical core licensing and vCPU oversubscription**.

Unfortunately, Broadcom has strained relationships with many customers and cloud providers. **Nutanix Cloud Infrastructure** is considered the closest to VMware in functionality and hybrid-cloud capabilities. Switching to **Nutanix AHV** may be a viable alternative, offering physical core licensing and oversubscription benefits both on-premises and in dedicated public cloud hardware.

*In the future blog I’ll deep dive into SQL licencing in public cloud*


#MCExperts, #MCX






















