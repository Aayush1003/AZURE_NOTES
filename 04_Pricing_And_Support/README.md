# 💰 Pricing & Support

Understanding how to manage costs and get help is essential for any Azure administrator.

## 1. Cost Factors
The cost of an Azure resource generally depends on three things:
- **Resource Type:** A VM costs more than an App Service; SSD storage costs more than HDD.
- **Consumption:** How long you run the service or how much data you store/transfer.
- **Location:** Prices vary between regions (e.g., East US vs. West Europe) based on local operational costs.
- *Note:* Inbound data transfer (ingress) is usually FREE. Outbound data transfer (egress) is usually CHARGED.

## 2. Cost Management Tools
- **Pricing Calculator:** Used to estimate costs *before* provisioning resources.
- **TCO (Total Cost of Ownership) Calculator:** Used to compare the cost of running an on-premises infrastructure vs. migrating it to Azure over time.
- **Azure Cost Management & Billing:** A tool within the Azure Portal to monitor *current* spending, track against budgets, and find ways to optimize (reduce) costs.

## 3. Azure Blueprints (Note: Being deprecated in favor of Template Specs/Deployment Stacks)
- **What is it?** Used to define a repeatable set of Azure resources that implement and adhere to an organization's standards, patterns, and requirements (combines ARM templates, RBAC roles, and Policies).

## 4. Azure Support Plans
- **Basic:** Free, included for all customers. Includes billing/subscription support and access to Azure Advisor.
- **Developer:** For trial and non-production environments. Support during business hours.
- **Standard:** For production workload environments. 24x7 tech support.
- **Professional Direct:** For business-critical workloads. Faster response times, architectural guidance.
- **Premier:** For organizations with substantial dependence on Microsoft technologies across multiple products.

## 5. Service Level Agreements (SLAs)
- **What is it?** Microsoft's commitment for uptime and connectivity (e.g., 99.9% uptime).
- **Composite SLA:** When you combine multiple services, the overall SLA is lower. You multiply the SLAs together (e.g., 99.9% VM * 99.9% SQL DB = 99.8% Composite SLA).
