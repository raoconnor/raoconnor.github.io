# If not VMware, then where?

For most organizations, the decision to exit VMware isn’t primarily a technical one. ESX remains a formidable hypervisor—hard to beat in performance and reliability. But ultimately, it’s just a hypervisor, and alternatives do exist. What’s driving these decisions is a mix of **financial pressure, strategic direction**, and **executive sentiment**.

Of these, **sentiment** is often underestimated. Strategy and financial models can be realigned if leadership is motivated. But when C-level executives express a desire to sever ties with Broadcom, the brand damage is difficult to repair.

So, if not VMware—then where? This document explores **public and private cloud** as target platforms, along with associated **caveats, blockers**, and **sentiment drivers**.


## Blockers 
One of the biggest blockers is technical debt, especially in the form of legacy operating systems. If application upgrades are required, this introduces budget constraints. So, if the motivation to leave VMware is financial, consider the **investment needed to modernize applications and systems**.
While unsupported operating systems can technically run in public cloud or on alternative hypervisors, these migrations often become problematic:

Lack of official virtual hardware drivers
Issues with migration tools
Post-migration instability

It’s not uncommon to see **10–20% of workloads running on unsupported OS versions**. Many organizations retain a minimal VMware footprint to host these workloads until they can be replaced.


## Licences

Licensing costs in public cloud can account for **30% or more of total cost of ownership**. 

Organizations that aren’t hosting providers often purchase Microsoft and Oracle licenses based on physical cores.

- Microsoft only allows Windows Server licenses with Software Assurance to be transferred to shared hardware on Azure.
- SQL Server licenses can be transferred to AWS if Software Assurance is in place.
- Oracle licenses purchased directly from Oracle are generally transferable.

However, moving to cloud may still result in **significant licensing cost increases**.

VMware architects are familiar with *pCPU to vCPU ratios* — the number of virtual cores that can run concurrently on a physical core. A 1:4 ratio is standard; 1:1 is reserved for high-performance workloads. Some production systems run at 1:12 without noticeable performance degradation, typically due to gradual load increases rather than greenfield design.
In public cloud, physical cores are translated to virtual cores. If your current ratio is 1:4, you may need **four times the licenses**. SQL Developer and replication licenses can help reduce this, but an increase is expected. 

![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/pcpu-vcpu.png)

**Dedicated hardware** is often suggested by hyperscalers to enable Bring Your Own License (BYOL). However:

- Cloud hypervisors don’t allow CPU oversubscription, so you will still need more licenses.
- Dedicated hardware introduces administrative complexity — e.g., grouping similar workloads into physical instances.
- Check your Microsoft T&Cs to confirm whether in-place upgrades are permitted.

If cost savings are the primary reason for moving to public cloud, factor in:

- Project costs for resolving obsolescence and technical debt
- Third-party licensing costs
- The shift from over-architected capacity to a **pay-as-you-go** model without a spend limit


## Staying on premises
If budget constraints are key, but executive sentiment means VMware is no longer viable, what are the on-premises alternatives?

Let’s be clear: the decision to exit VMware isn’t technical—it’s about **trust, stability**, and **relationship quality**. You’ll need to carefully evaluate potential providers.

While I won’t make specific recommendations, I will share that I was very impressed by my conversations with **Nutanix**. At recent .Next events, I attended closed-door sessions with their CTO and engineering teams. These are the same people who championed hyperconverged infrastructure and encouraged us to move away from traditional storage arrays.
Now, they’re listening to customers and investing engineering effort to support third-party storage—because that’s what the market wants.

While Nutanix’s integration with third-party storage has limitations:

- IP-based only (NAS or iSCSI)
- Currently only supports Dell, with Pure Storage nearing general availability

These constraints are **engineering-driven**, not punitive. They are concerned about the support impact of a rushed, poorly engineered solution.
It seems to me Nutanix is moving in the opposite direction from VMware — **more customers at lower revenue**, which strains engineering and support but speaks volumes about their brand values.

#MCExperts, #MCX






