
# The search for five nines

High levels of uptime is essential for critical applications as it ensures the availability and reliability of services and applications. Downtime can result in financial losses, reputational damage, regulatory fines and decreased customer satisfaction. 
Uptime or availability is often measured in nines. Five nines being consider the gold standard.


![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table1.png)



However, it’s easy to misinterpret claims on availability. As components will differ in expected uptime, for example traditional storage arrays have higher availably SLA than virtual machines, which makes sense as data loss can be far more disruptive than losing a front-end web server.
We could try calculating service availability by calculating the availability of  individual components, and calculating the SLAs together, for example [ 0.99982 * 0.99999 *  0.995 = 0.99481095 ]
This would give us a combined SLA of **99.4811%**. The overall system's availability being slightly lower than the individual SLAs due to the combined effect of potential downtimes.

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table2.png)


The number of components used by a single and dependencies make this difficult, tedious, and prone to error if single points of failure are not understood. 

So, design and architecture, or the combination of components is vital, having a datacenter with 99.999% uptime makes no sense if a service has dependency non-redundant components. 
For example, look at these two tables for Microsoft Virtual Machines 

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table3.png)


For single Instance VMs the disk type will effectively reduce the overall SLA.

![Table - N Redudancy](https://raoconnor.github.io/docs/assets/images/Availability-Table4.png)

For multiple instances the application needs to be distributed, that can be done at an application level or with a load balancer. And here we have perhaps the place on which we focus or energy, what can be done at the application and database layer.
Spreading some services across sites to improve availability reduces the dependency of underlying infrastructure and may be the vendors the preferred method for availability. Even so  if we take a critical business service such as SAP. Native SAP database synchronization only protects against losing data, there is a primary active system and secondary system standby system, during a failure downtime is required while the secondary database takes over. 

In conclusion. 
-	Don’t develop a false sense of security by SLAs with many 9s, take a holistic view of the architecture, especially that of the database or stateful services are key. 
-	Some single point of failures such as DNS, BGP, or the control plane have been the cause of major downtime for some of the world best engineered services. 
-	Humans are guaranteed to make mistakes at some point, I’m not sure what our SLA is but 99.999 is unlikely.

