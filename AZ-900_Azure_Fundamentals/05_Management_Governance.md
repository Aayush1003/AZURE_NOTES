# Domain 5: Management & Governance

## 1. Resource Groups

### Overview
- Container for related resources
- All resources must belong to a resource group
- Region-specific (but contains multi-region resources)

### Benefits
- Manage resources as group
- Apply policies together
- Delete all at once
- Organize by project/environment

---

## 2. Management Groups

### Hierarchy
```
Root
├── Management Group (Organization)
│   ├── Management Group (Department)
│   │   ├── Subscription 1
│   │   └── Subscription 2
│   └── Subscription 3
└── Subscription 4
```

### Benefits
✓ Hierarchical organization
✓ Apply policies at group level
✓ Inherit policies to child groups
✓ Centralized management

---

## 3. Azure Policy

### Overview
- Enforce compliance rules
- Audit and prevent non-compliant resources
- Built-in and custom policies

### Common Policies
- Require tags
- Enforce allowed locations
- Enforce SQL encryption
- Require MFA
- Disallow specific VM sizes

### Policy Effects
- **Audit**: Monitor compliance
- **Deny**: Prevent deployment
- **Append**: Add properties
- **Audit/Deny**: Conditional

---

## 4. Resource Locks

### Lock Types
- **Delete Lock**: Can modify but not delete
- **Read-Only Lock**: Cannot modify or delete

### Use Cases
✓ Prevent accidental deletion
✓ Protect production resources
✓ Enforce company policies

---

## 5. Azure Blueprints

### Overview
- Reusable environment templates
- Includes: ARM templates, RBAC, policies
- Versioning support
- Assignment tracking

### When to Use
✓ Standard environment setup
✓ Compliance templates
✓ Multi-subscription deployments

---

## 6. Tagging

### Purpose
- Organize and track resources
- Cost allocation
- Automation
- Access control

### Best Practices
- Use consistent naming
- Include: environment, owner, cost center
- Enforce via policy
- Regular cleanup

---

## 7. Azure Monitor

### Components
- **Metrics**: Numeric time-series data
- **Logs**: Detailed event data
- **Alerts**: Automated notifications
- **Dashboards**: Visualizations

### Key Features
- Application Insights (app monitoring)
- Diagnostic settings
- Log Analytics (query and analyze)
- Action groups (notifications)

---

## 8. Cost Management

### Budgets
- Set spending limits
- Alert when approaching limit
- Forecast costs

### Cost Analysis
- View actual spending
- Identify expensive resources
- Analyze trends

### Recommendations
- Right-size VMs
- Delete unused resources
- Use reserved instances
- Use Spot pricing

---

## 9. Compliance & Security

### Compliance Framework
- Industry standards: HIPAA, PCI-DSS, GDPR, SOC2
- Government: FedRAMP, CJIS
- Regional: China Cloud, German Cloud

### Azure Security Center
- Threat protection
- Vulnerability assessment
- Security recommendations
- Compliance status monitoring

### Privacy & Data Protection
- Data residency
- Data sovereignty
- GDPR compliance
- Encryption by default

---

## Key Exam Topics
✓ Resource organization (RGs, MGs)
✓ Azure Policy and enforcement
✓ Resource locks and protection
✓ Azure Blueprints
✓ Tagging strategies
✓ Azure Monitor capabilities
✓ Cost management features
✓ Budgeting and forecasting
✓ Compliance and standards
✓ Azure Security Center
✓ RBAC and access control
✓ Governance best practices

