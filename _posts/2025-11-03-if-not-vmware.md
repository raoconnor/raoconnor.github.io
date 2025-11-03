# If not VMware, then where?

For most organizations, deciding to exit VMware isn’t a technical discussion, as a hypervisor ESX is difficult to beat, but it’s just a hypervisor, and there are others, what is driving these decisions is more a mix of finance, strategic goals and sentiment 
Of these Sentiment is probably more important than many realise, strategy and even finance can be realigned, if leadership are motivated, so when C level executives say they want nothing more to do with Broadcom, the brand damaged is hard to repair.
So, if not VMware, then where? I’m going to discuss public and private cloud as target locations, caveats, blockers and sentiment.


## Blockers 
Obsolescence and technical debt in the form of legacy operating systems, if upgrades to applications are needed, this is a budget constraint, so if the reason to get out of VMware is due to financial restrictions, have you thought about the investment required to update applications and systems.
And while its true unsupported operating systems can run in public cloud and alternative hypervisors, these migrations can turn into nightmares, lack of official virtual hardware drivers, issues with migrations tools and problems post migration.  
As it’s not unusual to see anything between 5-20% of workloads out of OS support, understanding the cost of obsolescence and technical debt has led many to keep a small VMware hypervisor footprint, until those systems are replaced. 



## Licences

##· Licence costs in public cloud can be more than the cost of compute infrastructure

Organizations that are not hosting providers frequently purchase Microsoft and Oracle licences based on the physical cores. Microsoft only allow Windows server licences with software assurance to be transferred to shared hardware on Azure, SQL Server can be transferred to AWS if software assurance has been acquired. If Oracle licences are purchased directly from Oracle, then it’s probable those can also be transferred.
However, you might find move to cloud will require a significant cost increase in licences 

VMware architects are familiar with pCPU to vCPU ratios, which is the amount to virtual core that can run “simultaneously” on a physical core, 1:4 is considered standard, 1:1 is reserved for a few very high-performance workstations, I’ve see production systems running at 1:12 without and noticeable slowness, but that’s due to gradual load increase not green field design.

Moving to shared hardware in public cloud will require transferring physical cores to virtual cores, so if your current pCPU to vCPU ratio is 1:4 you will need four times the licences, for SQL developer and replication licences can help reduce that, but and increase is to be expected. 

![pcpu-vcpu](docs/assets/images/pcpu-vcpu.png)

Dedicated hardware will often be suggested by the Hyperscalers as a way around this, but keep in mind while it allows BYOL, the cloud hypervisor does not allow CPU oversubscription so the issues of needing more licences still applies. Additionally dedicated hardware can be a bit of an administrative headache due to fitting workloads of the same family into the physical instance, also you need to check your if your Microsoft T&Cs prevent in place upgrades.

So, if saving money is the reason to choose public cloud, include project costs for correcting obsolescence and technical debt, and take a good look at possible 3rd party licences costs, then be sure to add some extra, because the cloud financial model, is very different, it’s a switch from over architected excess capacity to a buy the minimum and pay for the extras model. 

## Staying on premises
If staying within a fixed budget is a primary motivation, but executive level sentiment means your days with VMware are numbered, then what on premise options are available. 
Let’s start with why your organization is exiting VMware, it’s not technical, it’s trust, stability, quality of relationship, skin in the game.  You need to size up you potential provider. 

I’m not going to make any recommendations, because there is a lot I just don’t know, but I will say I was extremely impressed with the conversations I had with Nutanix. At recent .Next events I was invited to some closed-door sessions with the CTO and engineering teams, now these are the guys who have been pushing hyperconverged and trying to get us to give up our beloved storage arrays, to hear them same they are backing down and putting in the engineering hours because that’s what customers want is convincing. 

Yes, Nutanix’s integration with third party storage has its constraints, IP only (NAS, or iSCSI) for now only Dell, with Pure about to GA shortly, but these are engineering driven not punitive. This is putting them on the opposite direction of VMware, more customers at lower revenue, that’s a strain on engineering and support, and says a lot about the brand.
