
# The search of five nines (highly available IT services)

High levels of uptime are essential for critical applications as they ensure the availability and reliability of services and applications. Downtime can result in financial losses, reputational damage, regulatory fines, and decreased customer satisfaction. Uptime, or availability, is often measured in "nines," with five nines being considered the gold standard.


![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table1.png)



However, it’s easy to misinterpret claims on availability. Components will differ in expected uptime; for example, traditional storage arrays have higher availability SLAs than virtual machines, which makes sense as data loss can be far more disruptive than losing a front-end web server. We could try calculating service availability by determining the availability of individual components and then combining the SLAs. For example, (0.99982 * 0.99999 * 0.995 = 0.99481095). This would give us a combined SLA of 99.4811%, with the overall system's availability being slightly lower than the individual SLAs due to the combined effect of potential downtimes

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table2.png)


The number of components used by a single and dependencies make this difficult, tedious, and prone to error if single points of failure are not understood. 

Design and architecture, or the combination of components, are vital. Having a data center with 99.999% uptime makes no sense if a service depends on non-redundant components.
For example, look at these two tables for Microsoft Virtual Machines: 

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table3.png)


For single instance VMs, the disk type will effectively reduce the overall SLA.

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table4.png)

For multiple instances, the application needs to be distributed, which can be done at the application level or with a load balancer. This is where we should focus our energy—on what can be done at the application and database layers.

Spreading services across sites to improve availability reduces dependency on the underlying infrastructure and may be the vendor's preferred method for availability. Even so, if we take a critical business service, such as SAP, native SAP database synchronization only protects against data loss. There is a primary active system and a secondary standby system; during a failure, downtime is required while the secondary database takes over. Understanding the capabilities of the application will help avoid over-architecting the infrastructure.

### In conclusion. 
- Don’t develop a false sense of security from SLAs with many 9s. Take a holistic view of the architecture, especially the database or stateful services.
- Some single points of failure, such as DNS, BGP, or the control plane, have caused major downtime for some of the world's best-engineered services.
- Humans are guaranteed to make mistakes at some point. I’m not sure what our SLA is, but 99.999 is unlikely.

