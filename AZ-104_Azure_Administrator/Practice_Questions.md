# AZ-104 Practice Questions & Answers

## Domain 1: Identities & Governance (6 Questions)

### Question 1
**Scenario**: You need to grant a user ability to manage virtual machines but NOT modify role assignments or delete resources in a resource group.

**Which role should you assign?**
A) Owner
B) Contributor
C) User Access Administrator
D) Virtual Machine Administrator

**Answer**: B) Contributor

**Explanation**: Contributor can perform all operations except manage access. Owner can do everything including manage access. User Access Administrator can only manage access. VM Admin would be too restrictive.

---

### Question 2
**Scenario**: Your organization uses Azure AD for identity management. You need to create a group that will automatically add/remove users based on department attribute.

**What should you use?**
A) Static Group
B) Dynamic Group
C) Distribution Group
D) Security Group

**Answer**: B) Dynamic Group

**Explanation**: Dynamic groups use membership rules based on user properties. Changes are automatic when properties change. Static groups require manual management.

---

### Question 3
**Scenario**: You have 3 subscriptions. You need to apply an Azure Policy that prevents creation of resources outside East US region across all subscriptions.

**Where should you create the Management Group and apply the policy?**
A) On each subscription individually
B) On a Management Group above the subscriptions
C) On each Resource Group
D) On each resource

**Answer**: B) On a Management Group above the subscriptions

**Explanation**: Management Groups allow hierarchical organization and policy inheritance. Applying policy at management group level applies to all child subscriptions.

---

### Question 4
**Scenario**: You apply a CanNotDelete lock to a resource group. What actions are ALLOWED?

A) Modify resources, Delete resource group
B) Modify resources, Modify resource group tags
C) Only read resources
D) Delete individual resources only

**Answer**: B) Modify resources, Modify resource group tags

**Explanation**: CanNotDelete lock prevents deletion but allows modification. ReadOnly lock would prevent modifications.

---

### Question 5
**Scenario**: You need to delegate Azure SQL Database management to a developer team but they should only be able to manage databases in the "Production" resource group.

**How should you assign permissions?**
A) Assign role at subscription level
B) Assign role at the "Production" resource group scope
C) Assign role at resource level
D) Assign Owner role

**Answer**: B) Assign role at the "Production" resource group scope

**Explanation**: RBAC follows principle of least privilege. Scope should be limited to where permissions are needed - the specific resource group.

---

### Question 6
**Scenario**: A user reports they cannot access a storage account. They have Contributor role on the storage account. What could be the reason?

A) Contributor role doesn't include storage access
B) Storage account has firewall rules blocking the user
C) User needs Storage Account Key Operator role
D) User needs Key Vault access

**Answer**: B) Storage account has firewall rules blocking the user

**Explanation**: Network-level firewall rules can restrict access regardless of RBAC role. Contributor role includes storage management permissions.

---

## Domain 2: Storage (5 Questions)

### Question 7
**Scenario**: You have large amounts of compliance data that must be retained for 7 years but accessed less than once per year.

**Which storage tier should you use?**
A) Hot
B) Cool
C) Archive
D) Premium

**Answer**: C) Archive

**Explanation**: Archive tier is lowest cost for infrequently accessed data. Retrieval takes hours but storage cost is minimal. Perfect for compliance data.

---

### Question 8
**Scenario**: You need to replicate data across multiple geographic regions for disaster recovery. Your RTO is 4 hours and RPO is 1 hour.

**Which replication option should you use?**
A) LRS
B) ZRS
C) GRS
D) GZRS

**Answer**: C) GRS

**Explanation**: GRS provides geo-redundancy (multiple regions). LRS/ZRS only provide local/zone redundancy. GZRS is more expensive but provides best redundancy.

---

### Question 9
**Scenario**: You upload a 5 GB file to Azure Blob Storage. What is the maximum size for a single blob upload via API?

A) 256 MB
B) 1 GB
C) 4.75 TB
D) 8 TB

**Answer**: C) 4.75 TB

**Explanation**: Block blobs have 4.75 TB max size. Single upload limit is 256 MB, but can use multi-part upload for larger files.

---

### Question 10
**Scenario**: You need to generate a token that allows temporary access to a specific blob in storage without sharing the account key.

**What should you use?**
A) Account key
B) Connection string
C) Shared Access Signature (SAS)
D) Storage account access tier

**Answer**: C) Shared Access Signature (SAS)

**Explanation**: SAS tokens provide temporary, scoped access without exposing account keys. Can set expiration, permissions, and IP restrictions.

---

### Question 11
**Scenario**: Your application needs to move files from on-premises to Azure Files. The files are accessed via SMB protocol.

**What protocol should the Azure Files share use?**
A) NFS only
B) SMB only
C) HTTPS
D) Either SMB or NFS

**Answer**: D) Either SMB or NFS

**Explanation**: Azure Files supports both SMB (Windows/Linux/Mac) and NFS (Linux/Mac). Choice depends on OS and requirements.

---

## Domain 3: Compute (6 Questions)

### Question 12
**Scenario**: You need to deploy 100 identical web servers that automatically scale based on CPU usage.

**What service should you use?**
A) Virtual Machines
B) Virtual Machine Scale Sets
C) App Service
D) Azure Functions

**Answer**: B) Virtual Machine Scale Sets

**Explanation**: VMSS is designed for auto-scaling identical VMs. Individual VMs don't scale. App Service is PaaS. Functions is serverless.

---

### Question 13
**Scenario**: You need to run Docker containers with minimal management overhead for short-lived workloads.

**What should you use?**
A) Azure Container Instances
B) Azure Kubernetes Service
C) App Service
D) Azure Functions

**Answer**: A) Azure Container Instances

**Explanation**: ACI is simplest for running containers - no orchestration needed. AKS requires cluster management. Best for dev/test and short tasks.

---

### Question 14
**Scenario**: Your application has a front-end and back-end. The front-end receives 10 Gbps of traffic.

**What should you use to distribute load?**
A) Layer 4 Load Balancer
B) Application Gateway
C) Traffic Manager
D) Front Door

**Answer**: A) Layer 4 Load Balancer

**Explanation**: Layer 4 LB handles TCP/UDP traffic at higher throughput. AppGw is Layer 7 (slower for non-HTTP). Need to handle extreme throughput.

---

### Question 15
**Scenario**: You delete a managed disk by mistake. Can you recover it?

A) No, it's permanently deleted
B) Yes, from Backup
C) Yes, from Snapshots
D) Yes, from soft delete

**Answer**: D) Yes, from soft delete

**Explanation**: Azure has soft delete for managed disks (7 day retention by default). Snapshots can also be used if taken previously.

---

### Question 16
**Scenario**: You're deploying a Windows Server VM. During deployment, you need to run a PowerShell script to install software.

**How should you do this?**
A) Manually RDP and run script after deployment
B) Use Custom Script Extension during VM creation
C) Use VM Extensions after deployment
D) Use Azure Automation

**Answer**: B) Use Custom Script Extension during VM creation

**Explanation**: Custom Script Extension runs during initial deployment. More efficient than manual or post-deployment installation.

---

### Question 17
**Scenario**: Your app requires predictable performance. You need guaranteed IOPS and throughput.

**What disk type should you use?**
A) Standard HDD
B) Standard SSD
C) Premium SSD
D) Ultra SSD

**Answer**: D) Ultra SSD

**Explanation**: Ultra SSD provides highest performance with guaranteed IOPS/throughput. Premium SSD is good but not guaranteed. Standard disks have lower performance.

---

## Domain 4: Networking (5 Questions)

### Question 18
**Scenario**: You have a VNet with address space 10.0.0.0/16. You need 5 subnets with ~250 usable IPs each.

**What subnet mask should you use?**
A) /24
B) /25
C) /27
D) /30

**Answer**: A) /24

**Explanation**: /24 provides 251 usable IPs (256 - 5 reserved). /25 = 123 IPs (too small), /27 = 27 IPs (too small), /30 = 2 IPs (too small).

---

### Question 19
**Scenario**: You need to block traffic from a specific IP address range to your Azure VMs.

**Where should you configure this rule?**
A) VM Firewall (Windows Firewall)
B) Network Security Group
C) Application Security Group
D) Route Table

**Answer**: B) Network Security Group

**Explanation**: NSG is the network-level firewall for Azure resources. Applied to NIC or subnet. Most efficient place to block traffic.

---

### Question 20
**Scenario**: You're setting up VNet peering between VNet-A and VNet-B. Will traffic from VNet-A to VNet-C transit through VNet-B?

A) Yes, automatically
B) No, peering is not transitive
C) Only if gateway transit is enabled
D) Only if forwarding is enabled

**Answer**: B) No, peering is not transitive

**Explanation**: VNet peering connects only the two peered networks. To connect VNet-C, you need either separate peering or a gateway.

---

### Question 21
**Scenario**: Your application requires a guaranteed 10 Gbps connection to Azure with no internet routing.

**What should you deploy?**
A) Site-to-Site VPN
B) Point-to-Site VPN
C) Express Route
D) VNet Peering

**Answer**: C) Express Route

**Explanation**: Express Route provides dedicated, private connections with guaranteed bandwidth. VPN uses internet. Peering is for VNet-to-VNet.

---

### Question 22
**Scenario**: You deployed an App Service web app. Users cannot resolve the DNS name you assigned.

**What could be the issue?**
A) App Service requires static IP
B) DNS propagation takes 24-48 hours
C) Network Security Group is blocking DNS
D) App Service IP changed

**Answer**: B) DNS propagation takes 24-48 hours

**Explanation**: DNS changes propagate gradually across internet. App Service handles DNS. NSG doesn't block DNS for Azure-assigned names.

---

## Domain 5: Monitoring (4 Questions)

### Question 23
**Scenario**: You need to be alerted when a VM's CPU usage exceeds 85% for more than 10 minutes.

**What should you create?**
A) Log Analytics query
B) Metric alert
C) Activity log alert
D) Application Insights rule

**Answer**: B) Metric alert

**Explanation**: Metric alerts monitor numerical data like CPU %. Log analytics is for logs. Activity log is for admin actions.

---

### Question 24
**Scenario**: You want to track every time a user is assigned a role in your Azure subscription.

**Where should you look?**
A) Azure Monitor metrics
B) Azure Monitor logs
C) Activity Log
D) Application Insights

**Answer**: C) Activity Log

**Explanation**: Activity Log tracks administrative actions like role assignments. Not in metrics or logs. AppInsights is for app-level events.

---

### Question 25
**Scenario**: You need to query VM performance data for the last 30 days and create custom visualizations.

**What tool should you use?**
A) Azure Monitor metrics chart
B) Log Analytics with KQL
C) Application Insights Analytics
D) Azure Advisor

**Answer**: B) Log Analytics with KQL

**Explanation**: Log Analytics with KQL provides powerful querying and visualization. Metrics chart is for basic charts. AppInsights is for apps. Advisor gives recommendations.

---

### Question 26
**Scenario**: You want to ensure RPO of 1 hour and RTO of 4 hours for your VMs.

**What should you implement?**
A) Snapshots only
B) Azure Backup with hourly backup frequency
C) Azure Site Recovery
D) Storage replication

**Answer**: C) Azure Site Recovery

**Explanation**: ASR provides continuous replication with controlled RTO/RPO. Backup is scheduled (doesn't meet 1hr RPO). ASR replicates continuously.

---

## Answer Summary

| Q# | Answer | Domain |
|----|--------|--------|
| 1 | B | Identity |
| 2 | B | Identity |
| 3 | B | Governance |
| 4 | B | Governance |
| 5 | B | Identity |
| 6 | B | Identity |
| 7 | C | Storage |
| 8 | C | Storage |
| 9 | C | Storage |
| 10 | C | Storage |
| 11 | D | Storage |
| 12 | B | Compute |
| 13 | A | Compute |
| 14 | A | Compute |
| 15 | D | Compute |
| 16 | B | Compute |
| 17 | D | Compute |
| 18 | A | Networking |
| 19 | B | Networking |
| 20 | B | Networking |
| 21 | C | Networking |
| 22 | B | Networking |
| 23 | B | Monitoring |
| 24 | C | Monitoring |
| 25 | B | Monitoring |
| 26 | C | Monitoring |

---

## Study Recommendations

**Score 60-70%**: Review weak domains in detail study modules
**Score 71-85%**: Practice more scenarios, focus on edge cases
**Score 86-95%**: Ready for exam, do final review
**Score 95%+**: Take full practice exam to confirm readiness

