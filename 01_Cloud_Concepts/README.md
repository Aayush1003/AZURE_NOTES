# ☁️ Cloud Concepts

Understanding these fundamental concepts is crucial for any Azure certification or cloud interview.

## 1. High Availability vs Scalability vs Elasticity
- **High Availability (HA):** Ensuring a service is operational and accessible for a long period without interruptions (measured in 'nines' like 99.99%).
- **Scalability:** The ability to increase or decrease resources to meet demand.
  - **Vertical Scaling (Scale Up):** Adding more power (CPU, RAM) to an existing machine.
  - **Horizontal Scaling (Scale Out):** Adding more instances of a machine (e.g., adding more VMs).
- **Elasticity:** The system's ability to scale resources automatically and dynamically in response to workload changes (e.g., auto-scaling).

## 2. Agility and Fault Tolerance
- **Agility:** The speed at which you can deploy and provision cloud resources.
- **Fault Tolerance:** The ability of a system to remain up and running even if a component fails.

## 3. Capex vs Opex
- **Capital Expenditure (CapEx):** Spending money upfront on physical infrastructure (Servers, Networking). You deduct the expense over time (depreciation).
- **Operational Expenditure (OpEx):** Spending money on services or products as you use them (Pay-as-you-go). Cloud computing falls under OpEx.

## 4. Cloud Deployment Models
- **Public Cloud:** Services offered over the public internet to anyone who wants to purchase them (e.g., Azure, AWS).
- **Private Cloud:** Cloud resources used exclusively by one business or organization. Can be physically located on-site or hosted by a third-party.
- **Hybrid Cloud:** Combines public and private clouds, allowing data and applications to be shared between them. Provides greater flexibility.

## 5. Cloud Service Models
- **IaaS (Infrastructure as a Service):** You rent IT infrastructure (servers, VMs, storage, networks) on a pay-as-you-go basis. E.g., Azure Virtual Machines. You manage the OS, runtime, and apps.
- **PaaS (Platform as a Service):** Provides a managed environment for developing, testing, delivering, and managing software applications. You manage the apps and data. E.g., Azure App Service, Azure SQL.
- **SaaS (Software as a Service):** Software hosted and managed for the end user. You only manage your data and access. E.g., Microsoft 365, GitHub, Salesforce.

### Responsibility Matrix:
- **On-Premises:** You manage EVERYTHING (Networking to Applications).
- **IaaS:** Cloud provider manages Physical infrastructure. You manage OS to Apps.
- **PaaS:** Cloud provider manages Physical + OS + Runtime. You manage Apps and Data.
- **SaaS:** Cloud provider manages ALMOST EVERYTHING. You just use the software.
