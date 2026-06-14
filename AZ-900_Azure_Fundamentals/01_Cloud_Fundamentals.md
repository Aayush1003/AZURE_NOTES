# Domain 1: Cloud Fundamentals (25-30%)

## 1. Cloud Computing Concepts

### Cloud Characteristics
- **On-demand self-service**: Use resources as needed
- **Broad network access**: Access from any device
- **Resource pooling**: Shared resources for multiple users
- **Rapid elasticity**: Scale up/down quickly
- **Measured service**: Pay for what you use

### Cloud Deployment Models

| Model | Infrastructure | Management | Best For |
|-------|-----------------|-----------|----------|
| **Public Cloud** | Cloud provider owns/manages | Provider manages | Cost-effective, scalability |
| **Private Cloud** | Organization owns | Organization manages | Control, security, compliance |
| **Hybrid Cloud** | Mix of public/private | Mixed management | Flexibility, cost optimization |

### Cloud Service Models

| Model | Who Manages | Responsibility | Examples |
|-------|-------------|-----------------|----------|
| **IaaS** | Provider | Apps, data, runtime, OS | Azure VMs, App Service |
| **PaaS** | Provider | Apps, data, runtime | App Service, Azure SQL DB |
| **SaaS** | Provider | Everything | Microsoft 365, Salesforce |

### Responsibility Matrix
```
On-Premises:   You manage everything (OS, software, data)
IaaS:          Provider manages infrastructure; you manage OS and above
PaaS:          Provider manages platform; you manage applications
SaaS:          Provider manages everything
```

---

## 2. Azure Benefits

### Cost Savings
- **No capital expenditure**: No upfront hardware investment
- **Operating expenditure model**: Pay only for what you use
- **Economy of scale**: Provider's costs distributed across users
- **Flexible pricing**: Various pricing models available

### Scalability & Elasticity
- **Scale up**: Increase resources for single resource
- **Scale out**: Add more instances
- **Automatic**: Scale based on demand
- **Global reach**: Deploy anywhere in the world

### Reliability & Availability
- **Redundancy**: Multiple copies across regions
- **High availability**: Uptime guarantees (SLAs)
- **Disaster recovery**: Automatic failover
- **99.99% SLA**: Common for Azure services

### Security & Compliance
- **Data encryption**: At rest and in transit
- **Identity management**: Azure AD integration
- **Compliance**: GDPR, HIPAA, PCI-DSS, SOC2
- **Security updates**: Automatic patching

### Global Infrastructure
- **60+ regions worldwide**: Choose location for latency
- **Data residency**: Keep data in specific region
- **Redundancy**: Cross-region replication
- **Azure Stack**: On-premises Azure

---

## 3. CapEx vs OpEx

### Capital Expenditure (CapEx)
- Upfront investment in physical infrastructure
- Hardware, data center, networking equipment
- Long-term asset ownership
- Depreciation over time

### Operational Expenditure (OpEx)
- Pay-as-you-go model
- Monthly/hourly billing
- No upfront investment
- Flexible scaling

### Cloud Advantage
```
Traditional (CapEx)          Azure Cloud (OpEx)
├─ Large upfront cost        ├─ No upfront cost
├─ Fixed resources           ├─ Flexible resources
├─ Slow to scale            ├─ Scale instantly
└─ Difficult to expand      └─ Easy to expand
```

---

## 4. Consumption-Based Pricing

### Azure Pricing Models
- **Pay-as-you-go**: Pay hourly for resources used
- **Reserved instances**: Commit 1-3 years, save 25-72%
- **Spot pricing**: Use spare capacity, save 90%
- **Free tier**: Limited free usage for new accounts

### Factors Affecting Cost
- **Region**: Pricing varies by geography
- **Service tier**: Standard vs Premium
- **Usage**: Data transfer, storage amount
- **Reserved capacity**: Advance commitment

---

## Key Exam Topics
✓ Cloud service models (IaaS, PaaS, SaaS)
✓ Cloud deployment models (Public, Private, Hybrid)
✓ Azure benefits (cost, scalability, reliability)
✓ CapEx vs OpEx comparison
✓ Shared responsibility model
✓ Azure regions and availability zones
✓ Global infrastructure benefits
✓ Consumption-based pricing
✓ Cost optimization strategies

