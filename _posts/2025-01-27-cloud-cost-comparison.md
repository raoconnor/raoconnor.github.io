# Which is more expensive, cloud or on-premises?

## Well, of course, it depends. 

Independent studies show there is theoretical potential for savings when moving to public cloud compared to staying on-premises. To understand those savings a TCO (Total Cost of Ownership) study should be conducted, however, creating a baseline of on-premises infrastructure is complex, and precisely forecasting the cost of running the same workloads in public cloud has its challenges.

Once in the cloud, without cost management and good governance, bills will be much higher than what was estimated. This has more to do with the estimate being overly optimistic and the need for cloud maturity rather than public cloud being more expensive than on-premises infrastructure.

The following use case is for predictable IaaS workloads, the sort we see in manufacturing. Using the following workloads as a baseline, I calculated the costs for an on-premises colocation facility with separate redundant fire rooms, as well as AWS, Azure, and GCP costs in North America

  *Baseline: 358 Prod and Non prod Instances, some instances were very large, 90 recover on demand instances in a seperate region, 370 TB of Disk storage, 598 TB of Backup Storage*

  
![Image-1](https://raoconnor.github.io/docs/assets/images/CloudCompare-1.png)



## Line Items
Let's first break down these charges. 

Sometimes cloud Bills of Materials (BoMs) are submitted to potential clients only showing rightsized **compute and disk costs**. In the above scenarios, compute was just under 50% of the total cloud spend, so if you see a projected cost based on only compute and disk, you need to think twice.

**Backup and DR** costs are not trivial. The above example uses a pilot light DR strategy with asynchronous database copies to another region; the other virtual machines would need to be recreated

**Network costs** will vary based on traffic flows, gateways, firewall type, and activity. Try to identify in advance if there is a large amount of data churn or replication.
**Monitoring, log storage**, and **other minor service** costs should not be underestimated. If you have done a Proof of Concept (PoC), it may be possible to extrapolate some of these costs to get an idea of how they accumulate.

**Support costs** should also be considered. If business-critical support is needed, for mid-range customers it's going to be between 7-10% of cloud spend.

Finally, we have added a 12% **contingency** for cloud overspend. While this line will be unpopular, keep in mind that the FinOps Organization estimates even mature organizations have a spend accuracy variance of around 12%. Expect to have workloads without discount commitments which will drive up the cost from 3 year RIs .

[FinOps Org Maturity Model](https://www.finops.org/framework/maturity-model/)

***Note on Rightsizing:** Reducing CPUs and memory for virtual machines can optimize costs both on-premises and in the cloud. While this can save hardware and license costs, careful capacity forecasting is essential when buying hardware.* 

 
## Totals Comparison 

These numbers are estimates and should not be taken to show that one provider is always cheaper. There is not a huge difference between on-premises and public cloud, as the effort to move to the cloud would probably negate that difference. Between hyperscalers, licenses can be massive, but in this scenario —IaaS based on SLES— advantages are fractional. Additionally, discount programs are not included, which could make it even harder to pick a winner.

---

Does this look better to you? Let me know if you need any further adjustments!


![Image-2](https://raoconnor.github.io/docs/assets/images/CloudCompare-3.png)



#### The point is that for stable IaaS workloads infrastructure cost should not be the primary driver for deciding where to locate. ####

If a client only focuses on infrastucture savings without transformation their journey to cloud may fall short of expectations.



