# 💻 Compute Services

Azure Compute provides on-demand computing resources (like CPUs, memory, and OS) for running your applications.

## 1. Azure Virtual Machines (VMs)
- **What is it?** Windows or Linux VMs hosted in Azure (IaaS). Provides total control over the OS.
- **Use Cases:** Lift-and-shift migrations, legacy applications, highly customized OS configurations.
- **Virtual Machine Scale Sets (VMSS):** Allows you to create and manage a group of identical, load-balanced VMs. Supports auto-scaling.

## 2. Azure App Service
- **What is it?** A fully managed PaaS offering to build, deploy, and scale web apps and APIs quickly.
- **Languages supported:** .NET, .NET Core, Node.js, Java, Python, or PHP.
- **Features:** Auto-scaling, high availability, continuous deployment from GitHub/Azure DevOps, custom domains, SSL certificates.

## 3. Azure Container Instances (ACI)
- **What is it?** The fastest and simplest way to run a container in Azure, without having to manage any VMs and without having to adopt a higher-level service. (PaaS/CaaS).
- **Use Cases:** Simple applications, task automation, build jobs.

## 4. Azure Kubernetes Service (AKS)
- **What is it?** A managed Kubernetes service that simplifies deploying, managing, and scaling containerized applications (PaaS).
- **Use Cases:** Microservices architectures, highly scalable applications, enterprise container orchestration.

## 5. Azure Functions
- **What is it?** An event-driven, serverless compute service. You write code that responds to an event (e.g., HTTP request, blob upload, timer), and Azure automatically manages the infrastructure. (Serverless/FaaS).
- **Billing:** You only pay for the time your code is running (Consumption plan).
- **Use Cases:** Image processing, background tasks, API endpoints, IoT data processing.
