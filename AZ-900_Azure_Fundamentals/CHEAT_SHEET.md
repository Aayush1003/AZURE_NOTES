# AZ-900 Quick Cheat Sheet

## Cloud Service Models

```
IaaS (Infrastructure as a Service)
├─ Examples: Azure VMs, App Service (compute)
├─ You manage: Applications, Data, Runtime, OS
└─ Provider manages: Virtualization, Servers, Storage, Networking

PaaS (Platform as a Service)
├─ Examples: App Service, Azure SQL DB, Functions
├─ You manage: Applications, Data
└─ Provider manages: Runtime, OS, Middleware, Infrastructure

SaaS (Software as a Service)
├─ Examples: Microsoft 365, Salesforce, Teams
├─ You manage: Users, Data
└─ Provider manages: Everything else
```

## Cloud Deployment Models

```
Public Cloud: Cloud provider owns everything
├─ Cost-effective
├─ No capital investment
└─ Scalable

Private Cloud: Organization owns infrastructure
├─ Full control
├─ Higher cost
└─ Complex management

Hybrid Cloud: Mix of public + private
├─ Flexibility
├─ Cost optimization
└─ Complex architecture
```

---

## Azure Services by Category

### Compute
- VMs: Full OS control
- App Service: Web apps and APIs
- Functions: Serverless, event-driven
- ACI: Simple containers
- AKS: Kubernetes orchestration
- Batch: Parallel processing

### Storage
- Blob: Object storage (files)
- File: SMB file shares
- Queue: Async messaging
- Table: NoSQL key-value
- Disk: VM disks (managed)

### Databases
- SQL Database: Relational
- Cosmos DB: Global NoSQL
- MySQL/PostgreSQL: Open-source
- Azure Cache: Redis caching

### Networking
- VNet: Virtual network
- Load Balancer: Layer 4 LB
- App Gateway: Layer 7 LB
- VPN Gateway: Encrypted tunnel
- Express Route: Dedicated connection
- DNS: Domain name service

### Management
- Resource Groups: Logical containers
- Management Groups: Hierarchy
- Azure Policy: Compliance
- Azure Monitor: Monitoring
- Cost Management: Budget control

---

## Replication & Redundancy

```
LRS (Locally Redundant Storage)
├─ 3 copies in one region
├─ Cheapest
└─ Lowest durability (11 nines)

ZRS (Zone Redundant Storage)
├─ 3 copies across 3 availability zones
├─ Medium cost
└─ Higher durability (12 nines)

GRS (Geo Redundant Storage)
├─ 6 copies (3 in primary + 3 in secondary region)
├─ Medium cost
└─ High durability (16 nines)

GZRS (Geo-Zone Redundant Storage)
├─ 6 copies (ZRS in 2 regions)
├─ Most expensive
└─ Highest durability (16 nines)
```

---

## SLA Examples

```
Service               SLA      Implications
VMs (single)          99.9%    ~8.76 hrs downtime/year
VMs (availability set) 99.95%  ~4.38 hrs downtime/year
VMs (availability zones) 99.99% ~52.6 mins downtime/year
App Service           99.95%   ~4.38 hrs downtime/year
Azure SQL             99.99%   ~52.6 mins downtime/year
```

---

## Pricing Models Quick Reference

```
PAY-AS-YOU-GO
├─ No upfront cost
├─ Hourly billing
└─ Higher long-term cost

RESERVED INSTANCES
├─ 1-year: ~30% savings
├─ 3-year: ~40-50% savings
└─ Upfront commitment

SPOT PRICING
├─ Up to 90% discount
├─ Can be interrupted
└─ For flexible workloads
```

---

## Compliance Quick Check

| Requirement | Azure Support |
|-------------|---------------|
| HIPAA | ✓ Yes |
| GDPR | ✓ Yes |
| PCI-DSS | ✓ Yes |
| SOC 2 | ✓ Yes |
| CJIS | ✓ Yes |
| FedRAMP | ✓ Yes |
| ISO 27001 | ✓ Yes |

---

## Common Exam Scenarios

**Scenario 1: Need to store files occasionally**
→ Answer: Cool tier or Archive tier

**Scenario 2: Need global scale and real-time**
→ Answer: Cosmos DB

**Scenario 3: Need traditional relational DB**
→ Answer: Azure SQL Database

**Scenario 4: Need serverless event-driven**
→ Answer: Azure Functions

**Scenario 5: Need containers with orchestration**
→ Answer: AKS

**Scenario 6: Need to connect on-premises**
→ Answer: VPN Gateway or Express Route

**Scenario 7: Need to enforce compliance**
→ Answer: Azure Policy

**Scenario 8: Need 99.99% SLA for VMs**
→ Answer: Availability Zones (2+ VMs)

---

## Important Dates & Numbers

- **Free account**: 12 months free + always-free services
- **Policy retention**: 6 years (audit logs)
- **SQL backup retention**: 35 days (default)
- **Cosmos DB consistency options**: 5 levels
- **App Service plans**: 6 tiers
- **Reserve commitment**: 1 or 3 years
- **Functions timeout**: 5 minutes (consumption)
- **Storage durability**: 11-16 nines (depends on replication)

---

## Exam Day Tips

✓ Read questions carefully - watch for negative keywords
✓ Understand the scenario before choosing
✓ Know service purposes, not just features
✓ Think about cost implications
✓ Consider compliance requirements
✓ Use elimination - remove obviously wrong answers
✓ Manage time - don't spend >2 min per question
✓ Flag difficult questions - come back to them
✓ Review flagged questions before submitting

---

## Must-Know Comparisons

```
App Service vs Azure Functions
→ Continuous vs event-driven, always-on vs pay-per-use

Load Balancer vs Application Gateway
→ Layer 4 (TCP/UDP) vs Layer 7 (HTTP/HTTPS)

VMs vs App Service
→ Full control vs managed platform

SQL Database vs Cosmos DB
→ Relational vs NoSQL, traditional vs global scale

Blob vs File Share
→ Object storage vs SMB/NFS share

VPN vs Express Route
→ Internet-based vs private dedicated connection
```

