# Domain 2: Azure Compute Services

## 1. Virtual Machines

### Overview
- Infrastructure as a Service (IaaS)
- Full control over OS and software
- Pay for compute, storage, networking

### When to Use VMs
✓ Need full OS control
✓ Running custom applications
✓ Need specific software versions
✓ Performance-critical workloads
✓ Lift-and-shift migrations

### VM Sizes
- **General Purpose (B, D)**: Balanced CPU, memory, storage
- **Compute Optimized (F)**: High CPU-to-memory ratio
- **Memory Optimized (E)**: High memory-to-CPU ratio
- **Storage Optimized (L)**: High disk throughput
- **GPU (N)**: Graphics processing, ML
- **High Performance (H)**: HPC workloads

---

## 2. Azure App Service

### Overview
- Platform as a Service (PaaS)
- Managed hosting for web apps
- Built-in DevOps, auto-scaling, security

### When to Use App Service
✓ Web applications (ASP.NET, Node.js, Python, Java)
✓ REST APIs
✓ Mobile app backends
✓ Need auto-scaling
✓ Want managed platform

### App Service Plans
- **Free/Shared**: Development only
- **Basic**: For testing
- **Standard**: Production workloads with scaling
- **Premium**: Advanced features, dedicated hardware
- **Isolated**: Maximum scalability and control

---

## 3. Azure Functions

### Overview
- Serverless compute
- Event-driven execution
- Pay per execution

### When to Use Functions
✓ Scheduled tasks
✓ Responding to events
✓ Short-lived operations
✓ Microservices
✓ Data processing pipelines

### Supported Triggers
- HTTP (APIs)
- Timer (CRON)
- Blob Storage
- Queue Storage
- Service Bus
- Event Hub
- Event Grid
- Cosmos DB

---

## 4. Container Services

### Azure Container Instances (ACI)
- Fastest way to run containers
- No orchestration overhead
- Per-second billing

### Azure Container Registry (ACR)
- Private Docker registry
- Geo-replication
- Build tasks

### Azure Kubernetes Service (AKS)
- Managed Kubernetes
- Auto-scaling
- Self-healing pods
- Network policies

### When to Use Each
```
ACI:  Quick testing, simple containerized apps
ACR:  Container image storage and distribution
AKS:  Complex microservices, production workloads
```

---

## 5. Batch Processing

### Azure Batch
- Large-scale parallel processing
- Cost-effective for compute-intensive tasks
- Auto-scaling compute nodes

### When to Use
✓ Media encoding
✓ Scientific simulations
✓ Financial modeling
✓ Data processing

---

## Azure Compute Comparison

| Service | Model | Use Case | Scaling | Cost |
|---------|-------|----------|---------|------|
| **VMs** | IaaS | Full control, custom apps | Manual/Auto | Hourly |
| **App Service** | PaaS | Web apps, APIs | Auto | Hourly |
| **Functions** | Serverless | Event-driven, short tasks | Auto | Per execution |
| **ACI** | Containers | Simple containers | Manual | Per second |
| **AKS** | Containers | Microservices | Auto | Compute + management |

---

## Key Exam Topics
✓ VM capabilities and sizing
✓ App Service plans and hosting
✓ Azure Functions triggers and bindings
✓ Container services comparison
✓ When to use each compute service
✓ Scaling options for each service
✓ Cost implications of each option
✓ Deployment methods
✓ High availability features

