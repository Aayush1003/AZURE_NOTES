# Domain 6: Pricing, SLAs, and Support

## 1. Pricing Models

### Pay-As-You-Go
- Hourly or per-minute billing
- No upfront cost
- Most flexible
- Higher cost long-term

### Reserved Instances
- Commit 1-3 years
- Save 25-72% vs pay-as-you-go
- Non-refundable commitment
- Good for stable workloads

### Spot Pricing
- Use spare capacity
- Save up to 90%
- Can be interrupted
- Good for flexible workloads

### Free Tier
- 12 months free
- Always-free services
- Limited usage amounts
- For new accounts

---

## 2. Cost Factors

### What Affects Cost

| Factor | Impact | Example |
|--------|--------|---------|
| **Region** | High | US East cheaper than Europe |
| **Data Transfer** | Medium | Outbound costs more |
| **Reserved Capacity** | High | Commit = lower cost |
| **VM Size** | High | Standard_D4 > Standard_B2s |
| **Storage Type** | Medium | Premium SSD > Standard HDD |
| **Service Tier** | High | Premium > Standard > Free |

### Cost Optimization
- Right-size resources
- Use reserved instances
- Turn off unused resources
- Monitor with Cost Management
- Use spot pricing
- Implement autoscaling
- Archive old data
- Use zones for cheaper pricing

---

## 3. SLA (Service Level Agreement)

### Understanding SLAs

| Uptime % | Downtime/Year | Downtime/Month |
|----------|---------------|----------------|
| 99% | 3.65 days | 43 min |
| 99.9% | 8.76 hours | 4.3 min |
| 99.95% | 4.38 hours | 2.16 min |
| 99.99% | 52.6 min | 4.3 sec |

### Azure Service SLAs
- **VMs (Single)**: 99.9%
- **VMs (2+ with Availability Set)**: 99.95%
- **VMs (2+ with Availability Zones)**: 99.99%
- **App Service (Standard+)**: 99.95%
- **Azure SQL DB (Standard)**: 99.99%
- **Azure Storage**: 99.9%-99.99%

### Composite SLA
```
SLA1 × SLA2 × SLA3
99.95% × 99.9% × 99.99% = 99.84%
```

---

## 4. Azure Support Plans

| Plan | Cost | Response Time | Use Case |
|------|------|---------------|----------|
| **Basic** | Free | None | Dev/test |
| **Developer** | $29/mo | <24h | Non-production |
| **Standard** | $300/mo | <1h | Production |
| **Professional Direct** | $1000/mo | <15min | Mission-critical |

### Support Includes
- Technical support
- Proactive guidance
- Training and resources
- Planning assistance

---

## 5. Azure Lifecycle

### Preview Stages
1. **Private Preview**: Invitation only
2. **Public Preview**: All users, feedback welcome
3. **General Availability**: Production-ready
4. **Deprecation**: Moving to retire
5. **Retirement**: Service ends

### Product Lifecycle Policy
- Minimum 12 months notice before retirement
- Extended support available
- Migration path provided

---

## 6. Compliance & Standards

### Compliance Offerings
- **HIPAA**: Healthcare compliance
- **GDPR**: EU data protection
- **PCI-DSS**: Payment card security
- **SOC 2**: Security and controls
- **ISO 27001**: Information security
- **FedRAMP**: US government

### Data Residency
- Keep data in specific region
- Required for some countries
- Impacts latency and cost
- GDPR: EU data in EU

### Privacy Commitments
- Data encryption by default
- No data mining
- Transparent practices
- User data ownership

---

## 7. Service Health

### Resource Health
- Specific resource status
- Maintenance notifications
- Incident information

### Service Health
- Service-wide status
- Planned maintenance
- Health advisories

### Azure Status Page
- Public status dashboard
- Historical data
- Subscription alerts

---

## Pricing Calculator

### Azure Pricing Calculator
- Estimate costs before deployment
- Adjust configurations
- Compare pricing tiers
- Save estimates

### Total Cost of Ownership (TCO)
- Compare cloud vs on-premises
- Include infrastructure, labor, maintenance
- Highlight cloud advantages

---

## Key Exam Topics
✓ Pay-as-you-go vs reserved vs spot pricing
✓ Cost factors and optimization
✓ SLA percentages and implications
✓ Composite SLA calculation
✓ Support plan selection
✓ Lifecycle stages
✓ Compliance and standards
✓ Data residency requirements
✓ Service health monitoring
✓ Cost estimation
✓ Service agreements
✓ Support response times
✓ Planning and migration

