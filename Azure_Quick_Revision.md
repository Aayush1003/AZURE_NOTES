# 🚀 Azure Assessment & Quick Revision Guide

This document is a highly condensed cheat sheet designed for rapid revision before technical assessments or certification exams. It focuses on high-yield topics, "gotchas," and scenario-based service selection.

## 1. Cloud Concepts: The "Gotchas"
- **CapEx vs. OpEx:** 
  - *CapEx (Capital Expenditure):* Buying physical servers upfront (On-Premises).
  - *OpEx (Operational Expenditure):* Pay-as-you-go, deducting expenses as they happen (Cloud).
- **Elasticity vs. Scalability:**
  - *Scalability:* The *ability* to handle increased load (adding more RAM or more VMs).
  - *Elasticity:* The system doing it *automatically* based on real-time demand.
- **Service Models:**
  - *IaaS:* You manage the OS, runtime, and apps (e.g., VMs). Best for lift-and-shift.
  - *PaaS:* You manage only the app and data (e.g., App Service, Azure SQL). Best for modern cloud-native dev.
  - *SaaS:* You manage nothing but your data (e.g., Office 365).

## 2. Global Infrastructure
- **Availability Zones (AZs):** Protect against *datacenter* failures (power, cooling). Not all regions support AZs.
- **Region Pairs:** Protect against *regional* disasters. Azure automatically pairs regions (e.g., East US & West US). Data residency is maintained within the same geography.

## 3. Which Compute Service to Choose?
- Need full control over the OS? **➔ Virtual Machines (IaaS)**
- Need to host a web app without managing servers? **➔ App Service (PaaS)**
- Need to run background code in response to an event? **➔ Azure Functions (Serverless)**
- Need to run a single Docker container quickly? **➔ Azure Container Instances (ACI)**
- Need to orchestrate hundreds of containers? **➔ Azure Kubernetes Service (AKS)**

## 4. Storage Redundancy Options
- **LRS (Locally Redundant):** 3 copies in ONE datacenter. Cheapest, lowest durability.
- **ZRS (Zone Redundant):** 3 copies across THREE datacenters (AZs) in one region.
- **GRS (Geo-Redundant):** 6 copies total. 3 in primary region (LRS) + 3 in paired region (LRS). Protects against regional outages.

## 5. Security & Networking
- **Network Security Group (NSG):** Acts as a firewall for a VNet. Applied at the Subnet or VM (NIC) level. Uses allow/deny rules based on IP/Port.
- **Azure Application Gateway:** Layer 7 Load Balancer. Use this if you need **Web Application Firewall (WAF)** or URL-based routing.
- **Azure Load Balancer:** Layer 4 Load Balancer (TCP/UDP). No WAF capabilities.
- **Azure ExpressRoute:** Private, dedicated connection to Azure. *Does not go over the public internet.* Faster and more secure than VPN.

## 6. Identity & Governance
- **Microsoft Entra ID (Azure AD):** Identity and access management. Used for Authentication (AuthN).
- **RBAC (Role-Based Access Control):** Controls *who can do what* (e.g., Who can delete a VM?).
- **Azure Policy:** Controls *what resources can be created* (e.g., You can only deploy VMs in the 'East US' region).
- **Resource Groups:** Logical containers. Deleting a resource group deletes ALL resources inside it. Resources can be moved between groups.

## 7. Cost & Support
- **Pricing Calculator:** Estimate costs *before* you deploy.
- **TCO Calculator (Total Cost of Ownership):** Compare on-premise costs vs. Azure cloud costs.
- **Cost Management:** Monitor and analyze *existing* Azure spending.
- **SLA (Service Level Agreement):** Microsoft's uptime guarantee. Note: Free/Dev tiers often have NO SLA.

## 8. Management & Deployment Tools
- **Azure Portal:** Web-based GUI. Best for single-resource creation and visual management.
- **Azure CLI:** Command-line interface available on Windows, macOS, and Linux. Uses Bash syntax (e.g., `az group create`).
- **Azure PowerShell:** Module for managing Azure via Windows PowerShell cmdlets (e.g., `New-AzResourceGroup`).
- **Azure Cloud Shell:** Browser-based terminal directly in the portal. Comes pre-installed with CLI, PowerShell, and common tools (Docker, Git). Requires a Storage Account to persist files.
- **ARM Templates (Azure Resource Manager):** Infrastructure as Code (IaC) using JSON. Allows declarative deployment of resources (you state *what* you want, not *how* to build it).
- **Azure Bicep:** A domain-specific language (DSL) that is a cleaner, easier-to-read alternative to JSON ARM templates.

## 9. Monitoring & Diagnostics
- **Azure Monitor:** The central hub for collecting, analyzing, and acting on telemetry data. Monitors performance and health of cloud and on-prem resources.
- **Log Analytics:** A tool in the Azure portal used to edit and run log queries against data collected by Azure Monitor logs (using Kusto Query Language - KQL).
- **Application Insights:** A feature of Azure Monitor specifically for developers. Provides APM (Application Performance Management) to track live web apps (detects anomalies, tracks request rates, response times, and failure rates).
- **Azure Service Health:** Provides a personalized view of the health of the Azure services and regions *you* are using. Alerts you about Microsoft-side outages.

## 10. Deep Dive: Governance & Compliance
- **Resource Locks:** Prevents accidental deletion or modification of resources.
  - *CanNotDelete:* Authorized users can read/modify, but cannot delete.
  - *ReadOnly:* Authorized users can only read (cannot modify or delete).
  - *Note:* Locks override RBAC permissions. Even an Owner cannot delete a locked resource until the lock is removed.
- **Resource Tags:** Name/value pairs applied to resources (e.g., `Environment: Production`, `Dept: HR`). Crucial for consolidated billing, cost allocation, and logic management.
- **Microsoft Defender for Cloud:** Provides unified security management and advanced threat protection. Gives you a **Secure Score** (a metric of your security posture).
- **Azure Key Vault:** Centralized, secure storage for secrets, encryption keys, and SSL/TLS certificates.

## 11. Azure Organizational Hierarchy
Understanding the scope is critical for RBAC and Policies:
1. **Management Groups:** Group multiple subscriptions together. Apply policies and RBAC here to cascade down to everything below.
2. **Subscriptions:** A billing boundary and an access boundary. You can have multiple subscriptions under one Entra ID tenant.
3. **Resource Groups:** A logical container for resources that share the same lifecycle (e.g., a web app, its database, and its storage account).
4. **Resources:** The actual instances (VMs, SQL Databases).

## 12. Integration & Serverless Deep Dive
- **Azure Logic Apps:** Cloud service that helps you schedule, automate, and orchestrate tasks, business processes, and workflows. *GUI-based (visual designer), no code required.*
- **Azure Event Grid:** A fully managed, highly scalable event routing service. Uses a publish-subscribe model (reacts to status changes like a blob being created).
- **Azure Service Bus:** Enterprise-grade message broker with message queues and publish-subscribe topics. Best for high-value financial transactions where messages cannot be lost.

## 13. Data & AI (Brief Overview)
- **Azure Synapse Analytics:** Enterprise analytics service that brings together big data and data warehousing.
- **Azure Databricks:** Apache Spark-based analytics platform optimized for Azure.
- **Azure Cognitive Services:** Pre-built AI models you can consume via API (Vision, Speech, Language, Decision) without needing machine learning expertise.
