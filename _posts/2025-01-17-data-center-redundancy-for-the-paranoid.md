
# Data Center Redundancy for the Paranoid 

Over the last year, I conducted an investigation into the advisability of twin data centers and the probability of site outages.

## Causes of data center outages

According to the Uptime Institute, power is the principal technical cause of site loss, followed by network, cooling, hardware, and software failures. My personal experience confirms this, with 5 out of 6 data center outages being power-related. (Note that most power failures are internal to the data center and not due to grid failure.)

Besides purely technical causes, human error—whether by an operator, misconfiguration, or due to a single point of failure in design—is a significant factor in outages.

Of all outages, fire strikes fear into all IT personnel. While data center fires are unusual, they can result in more than a few hours of downtime. A fire will usually require all building power to be abruptly shut down so the fire department can safely enter the building. Even a small fire can introduce delays in restarting equipment until authorities investigate the cause and give the green light to resume service.

There have been reports that the introduction of Li-ion batteries into data centers presents a greater fire risk than valve-regulated lead-acid batteries. It's crucial to ensure that data center operators have appropriate fire safety mechanisms such as Very Early Smoke Detection Apparatus (VESDA) systems and the ability to ensure proper fire suppression in their facilities.

Uncontained fires, such as those affecting South Korea's SK Group or OVHcloud in Strasbourg, are reminders that data center fires do happen and can have catastrophic consequences.

## Data center Redudancy

Data center providers will explain that all equipment is redundant, but there are different standards of redundancy.

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/table-n-redudancy.png)

To add to the complexity, individual components may have higher or lower tolerance to failure. For example, power supply may be configured as 2N with fully redundant or duplicate components. If one of the power circuits were to fail, the other circuit has the capacity available for the needed amperage. 

However, other components may have N+1 redundancy, which provides a minimal level of resiliency by adding a single additional component to the N architecture. For instance, if multiple cooling systems are needed to manage the site running at full load, when one system is offline, a single additional system is available to take over its load.
This configuration follows recognized design standards of one additional component for every four required to support full capacity. While N+1 introduces some redundancy, it still presents a risk in the event of multiple simultaneous failures.

## So what do we do?

In the past, part of my job was the design and setup of VMware metro clusters across twin data centers. This configuration was common in government, financial institutions, and the automotive industry. Active-active data centers protect against site-wide errors such as power or cooling failures, or fire.

Latency considerations for storage replication or core services such as mainframe and SAP clusters will limit the distance between sites, reducing protection from extreme weather or natural disasters. Hence, separate fully redundant fire rooms in a single site are often considered a viable alternative.

However, in the case of active-active sites, logically extending services such as storage and network can result in failures that affect both physical sites. Additionally, this does not provide protection against ransomware or other nefarious attacks.

In my opinion, a data center outage of more than a few hours per year is unlikely. Strategic decisions need to be made on cost-benefit—in other words, how much will downtime cost the business compared to how much we spend to try and mitigate it?

Then, and this is very important, where do we spend the money: on high availability or disaster recovery? Given that budgets are limited, this is a risk assessment that should happen before you put your money on the table.

Though keep in mind the words of Andrew Grove former Inter CEO, Only the paranoid survive. 
