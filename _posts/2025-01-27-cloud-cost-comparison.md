# Is cloud more expensive than on-premise?

Well, of course, it depends. Creating a Total Cost of Ownership (TCO) is difficult. While independent studies show there is theoretical potential for savings, the ability to create new workloads and leave them running without cost management and governance will likely result in a shock when someone starts comparing the bills with the expected budget.

The following use case is for predictable IaaS workloads, the sort we see in manufacturing. Using the following workloads as a baseline, I calculated the costs for an on-premises colocation facility with separate redundant fire rooms, AWS, Azure, and GCP in North America

- *Instances	358*
- *Instances DR	90*
- *Storage VM Disk TB	370*
- *Backup VM TB	598*

![Image-1](https://raoconnor.github.io/docs/assets/images/CloudCompare-1.png)


Let's first break down these charges. 

Sometimes cloud Bills of Materials (BoMs) are submitted to potential clients only showing rightsized **compute and disk costs**. In the above scenarios, compute was just under 50% of the total cloud spend, so if you see a projected cost based on only compute and disk, you need to think twice.

**Backup and DR** costs are not trivial. The above example uses a pilot light DR strategy with asynchronous database copies to another region; the other virtual machines would need to be recreated

**Network costs** will vary based on traffic flows, gateways, firewall type, and activity. Try to identify in advance if there is a large amount of data churn or replication.
Monitoring, log storage, and **other minor service** costs should not be underestimated. If you have done a Proof of Concept (PoC), it may be possible to extrapolate some of these costs to get an idea of how they accumulate.

**Support costs** should also be considered. If business-critical support is needed, for mid-range customers it's going to be between 7-10% of cloud spend.

Finally, we have added a 12% **contingency** for cloud overspend. While this line will be unpopular, keep in mind that the FinOps Organization estimates even mature organizations have a spend accuracy variance of around 12%. Expect to have 20% of workloads without discount commitments.

[FinOps Org Maturity Model](https://www.finops.org/framework/maturity-model/)

**Note on Rightsizing:** Reducing CPUs and memory for virtual machines can optimize costs both on-premises and in the cloud. While this can save hardware and license costs, careful capacity forecasting is essential. \\

With all that out of the way, there is not a huge amount of difference; these numbers are estimates and should not be taken to show that one provider is always cheaper. The point is that for stabel IaaS workloads infrastructure cost is not a primary driver for deciding where to locate.

![Image-2](https://raoconnor.github.io/docs/assets/images/CloudCompare-3.png)


One point to add to the above is that numerous operating costs, as well as the effort involved in hardware renewal, are reduced when relocating to the public cloud. At the same time, cost control specialists are vital in keeping cloud costs within budget.

The value of public cloud can be challenging to articulate, especially when a client is primarily concerned with cost. Assigning a monetary value to advantages such as increased speed, accessibility of complex services, regional or global presence, and a host of other benefits aligns with business values but can sometimes sound like wishful thinking.



