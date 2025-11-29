
# If Not VMware, Then Where? – *SQL Server License Costs* 


Infrastructure costs are only one part of the overall IT budget. When moving to the public cloud, ignoring how license agreements with third parties such as Microsoft and Oracle impact compliance can lead to significant cost increases.

Whether you’re migrating to **Azure**, **AWS**, or another cloud provider, understanding these licensing changes is critical to avoiding unexpected expenses.

---

## From Hardware-Based to Consumption-Based Licensing

On-premises **SQL Server** licensing often uses **Per Core Licensing** based on physical cores.  
In the cloud, you have several licensing options:

- **License Included**  
  Licenses are bundled as part of the service, such as Azure SQL Database, Amazon RDS for SQL Server, or SQL Server on Amazon EC2.

- **License Mobility through Software Assurance**  
  Allows you to move eligible Microsoft licenses (like SQL Server) to shared cloud infrastructure (e.g., AWS EC2 or Azure VMs).  
  *Note: This does **not** cover Windows Server OS licenses, except in Azure*

- **Bring Your Own License (BYOL)**  
  A broader term for using your own licenses in the cloud. This can apply to both dedicated and shared environments, depending on license terms.  
  Software Assurance may or may not be required, depending on the product and cloud provider.

---

## Cost Impact of Licensing Choices

The following diagrams illustrate how using existing licenses can significantly reduce costs.  

![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/sql1.png)

For example, in **Azure**, committing to an instance and using license mobility via **Hybrid Benefit** can reduce costs from **\$1.173** to **\$589**, nearly 50% less than deploying an instance with a bundled SQL license.

For **Pay-As-You-Go (PAYG)**, SQL licensing for 16 CPUs can add **\$583**. 


There is price parity for **cloud-native Azure Database Service** 
Note: Azure SQL Database Service is typically suitable for new cloud-native apps and Managed Instance for migration from on-prem SQL Server with minimal changes.


![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/sql2.png)

For **AWS**, the choice between **Standard** and **Enterprise** editions is a huge red flag - this could become a serious issue if DBAs and application architects select Enterprise simply because it was previously paid for on physical hosts. Having to aquire a new SQL enterprise licence is a 500% percent increase on a BYOL reserved instance.

![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/sql3.png)

RDS is the full PaaS service, the higher price tag is in consideration of the lower operational effort to maintain.

![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/sql4.png)

---

## The FinOps Perspective

Public clouds make deploying databases and services incredibly easy. However, if teams fail to plan for licensing costs, expenses can **double, triple, quadruple—or in the worst case, be nearly six times higher** than the BYOL option.

---

## Predictability Through Fixed Capacity

Changing from a 1:4 core oversubscription to 1:1 can have license implications. Having fixed capacity in the form of dedicated hosts, that enables **physical cores licensing** (and more importantly the **oversubscription those cores**) provides both cost predictability and peace of mind. This approach ensures that computing and licensing costs remain stable.

![pcpu-vcpu](https://raoconnor.github.io/docs/assets/images/sql5.png)

This doesn’t have to be on premises. Hybrid cloud infrastructure solutions such as **Nutanix Cloud Platform (NCP)**, which combines hyperconverged infrastructure (HCI) with cloud services allows organizations to run servers either on-premises and public cloud environments on dedicated hardware with the ability to oversubscribe CPU and maximize the usage of expensive database licenses.

*For addtional details see my earlier post* 

[If Not VMware, Then Where? - Windows Server License Costs](https://raoconnor.github.io/2025/11/14/If-not-VMware-Window-Server-License-Costs.html)

---

### Key Takeaway
- While the cloud abstracts much of the technical complexity of running workloads, it requires **in-depth knowledge of the consequences of licensing choices** when migrating or deploying new workloads. 
- Licensing decisions in the cloud are not just technical—they’re financial. Understanding these nuances is essential for cost optimization and compliance.
