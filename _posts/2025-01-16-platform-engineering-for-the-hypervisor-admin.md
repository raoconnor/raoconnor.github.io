## Platform Engineering for the hypervisor admin


 ### 1. What is Platform Engineering and Why Should I Care?
At its core, platform engineering is about abstracting away the complexities of infrastructure and providing a standardized foundation for delivery. It's more than automation or infrastructure as code; platform engineering acts as the glue that binds various IT management and operational processes together.

### 2. How Platform Engineering Helps Simplify IT Operations
#### - Centralized Integration Layer:
Platform engineering often involves creating a centralized layer that connects tools and systems. This layer can serve as a bridge between ITSM (IT Service Management), CMDB (Configuration Management Database), monitoring tools, and capacity management systems.

#### - IT Asset Management:
Enterprises with hundreds or even thousands of servers need a consistent data model to ensure that information about IT assets, configurations, and services is standardized. Understanding system obsolescence, capacity, and growth requirements across an IT landscape that may span various sites, countries, and teams is often difficult. Platform Engineers centralize this responsibility to a core team, making it easier to correlate data and enhance the overall effectiveness of IT operations.

#### - Automated Workflows:
Platform engineering can implement automated workflows that tie these systems together. For example:

 - When an incident is detected in the monitoring system, it can automatically create a ticket in the ITSM tool.
 - Changes made through the ITSM process can automatically update the CMDB.
 - Capacity management decisions can be informed by real-time performance data from monitoring tools.

#### - Single Source of Truth:
By integrating these systems, platform engineering helps establish a single source of truth for IT operations. This reduces discrepancies and improves decision-making across the organization.
These automated workflows are crucial following a hypervisor failure where dozens of systems are impacted. In a major outage when managers want reports every hour, platform engineering ensures that information in the ticketing tool or CMDB is not missing, incorrect, or out of date.

#### - Enhanced Visibility:
Through dashboards and reporting tools, platform engineering can provide a holistic view of IT operations, pulling data from all these integrated systems into one place.
By integrating incident management with performance monitoring, platform engineering can help correlate events and identify root causes more quickly.

#### - Streamlined Processes:
By connecting these systems, platform engineering can help streamline processes. For instance, incident resolution can be faster when responders have immediate access to configuration data and performance metrics.

#### - Consistent User Experience:
At its core, platform engineering is about abstracting away the complexities of infrastructure and providing a standardized, automated foundation for IT. Platform engineering can provide a consistent interface for interacting with systems, to some extent replacing a complex interface with hundreds of knobs and buttons with a universal master control panel for all IT operations.

#### - API-First Approach:
Many platform engineering initiatives adopt an API-first approach, making it easier to integrate new tools and systems as they are adopted.
This approach will allow component tools to be swapped out and replaced as needs and abilities change.

### 3. How does a reliable hypervisor such as VCF fit into this?
When I was younger, I used to change the oil and do other maintenance on my car. Nowadays, I rarely lift the hood to look at the engine. I have neither the time nor inclination to learn the specialist skills needed to correctly service a modern car; basically, I am more interested in driving than understanding the nuts and bolts. Just as I want a reliable car that starts every time I get in it, most enterprises likewise want reliable building blocks without having to learn complex mechanics. Because making everything work together is what brings value to the business.





