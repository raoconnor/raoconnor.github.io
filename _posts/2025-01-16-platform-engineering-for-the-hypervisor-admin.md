## The role of VMware Cloud Foundation in Platform Engineering




 
### 1. What is Platform Engineering and why should I care
At its core, platform engineering is about abstracting away the complexities of infrastructure and providing a standardized, foundation for delivery. Its more than automation, or infrastucture as code, platform engineering acts as the glue that binds these various IT management and operational processes together.




### 2. How Platform Engineering Helps Simplify IT Operations

#### Centralized Integration Layer:
Platform engineering often involves creating a centralized layer that connects tools and systems. This layer can serve as a bridge between ITSM (IT Service Management), CMDB (Configuration Management Database), monitoring tools, and capacity management systems.

#### IT Assest Management:
Enterprises with hundreds or even thousands of servers, need a consistent data model to ensures that information about IT assets, configurations, and services is standardized. Being able to understand system obsolencence, capacity and growth requirement across an IT landscape that may span varous sites, countries and teams is often difficult. Platform Engineers centralizes the responsablity to a core team making it easier to correlate data, enhancing the overall effectiveness of IT operations.


#### Automated Workflows:
Platform engineering can implement automated workflows that tie these systems together. For example:
When an incident is detected in the monitoring system, it can automatically create a ticket in the ITSM tool.
Changes made through the ITSM process can automatically update the CMDB.
Capacity management decisions can be informed by real-time performance data from monitoring tools.

#### Single Source of Truth:
By integrating these systems, platform engineering helps establish a single source of truth for IT operations. This reduces discrepancies and improves decision-making across the organization.

#### Enhanced Visibility:
Through dashboards and reporting tools, platform engineering can provide a holistic view of IT operations, pulling data from all these integrated systems into one place.

#### Streamlined Processes:
By connecting these systems, platform engineering can help streamline processes. For instance, incident resolution can be faster when responders have immediate access to configuration data and performance metrics.

#### Improved Capacity Planning:
Integration of performance monitoring with capacity management tools allows for more accurate and proactive capacity planning based on real-world usage data.

#### Consistent User Experience:
Platform engineering can provide a consistent interface for interacting with these various systems, improving usability for IT staff.

#### API-First Approach:
Many platform engineering initiatives adopt an API-first approach, making it easier to integrate new tools and systems as they are adopted.

#### Automated Discovery and Updates:
Platform engineering can implement automated discovery mechanisms that keep the CMDB up-to-date with changes in the IT environment, ensuring that it remains accurate and useful.

#### Correlation of Events:
By integrating incident management with performance monitoring, platform engineering can help correlate events and identify root causes more quickly.

#### Compliance and Auditing:
Integration of these systems can also aid in compliance efforts by providing comprehensive, correlated data for audits.


### 3. Installing ESX. 
I was able to install ESX 7.03g  from a custom image with the Community Network Driver  and the USB Network Native Driver for ESXi, However the install fails at 81% with Exception: No vmknic tagged for management was found. There is a workaround, to remove the installation media reboot the system, and Restore the Network, however I found on host reboot the static address was lost.
I tried recreated the image, but could fix the issue.


### 4. Networks. 
As the built-in NICs are not recognized, I was having to use USB nics, I tried using two USB NICs but this led to other issues.

 
### 5. Finally 
I gave up and installed Ubuntu and VMware Workstation as that allows me to use the in-bult two network ports and simulate complex environments

Big thanks to VMware and Cohesity as this gave an excuse to rebuild my homelab environment, I’ve already tried our vSAN and installed ESXi 8, next on the list is NSX-T, and TKG, so I'm currently trying to understand how to simulate some complex multisite network senarios.

In summary I read recently that you don’t really know something well until you break it, which is what home labs are all about, getting from objective to up and running and understanding what breaks in between.
