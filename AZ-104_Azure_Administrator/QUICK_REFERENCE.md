# AZ-104 Quick Reference - Commands & Key Points

## Azure CLI Quick Commands

### Authentication & Account
```bash
# Login to Azure
az login

# Set default subscription
az account set --subscription "subscription-id"

# Get current account info
az account show
```

### Resource Groups
```bash
# Create RG
az group create --name "RG-Name" --location "East US"

# List RGs
az group list

# Delete RG
az group delete --name "RG-Name"
```

### Virtual Machines
```bash
# Create VM
az vm create --resource-group "RG-Name" --name "myVM" --image "UbuntuLTS" --admin-username "azureuser"

# List VMs
az vm list --resource-group "RG-Name"

# Get VM details
az vm show --resource-group "RG-Name" --name "myVM"

# Start/Stop VM
az vm start --resource-group "RG-Name" --name "myVM"
az vm stop --resource-group "RG-Name" --name "myVM"

# Delete VM
az vm delete --resource-group "RG-Name" --name "myVM"
```

### Virtual Networks
```bash
# Create VNet
az network vnet create --resource-group "RG-Name" --name "myVNet" --address-prefix 10.0.0.0/16 --subnet-name "subnet1" --subnet-prefix 10.0.0.0/24

# List VNets
az network vnet list --resource-group "RG-Name"

# Create subnet
az network vnet subnet create --resource-group "RG-Name" --vnet-name "myVNet" --name "subnet2" --address-prefix 10.0.1.0/24
```

### Network Security Groups
```bash
# Create NSG
az network nsg create --resource-group "RG-Name" --name "myNSG"

# Add inbound rule
az network nsg rule create --resource-group "RG-Name" --nsg-name "myNSG" --name "AllowHTTP" --priority 100 --source-address-prefixes "Internet" --destination-port-ranges 80 --access Allow --protocol Tcp

# List rules
az network nsg rule list --resource-group "RG-Name" --nsg-name "myNSG"
```

### Storage Accounts
```bash
# Create storage account
az storage account create --name "storage" --resource-group "RG-Name" --location "East US" --sku "Standard_LRS"

# Get storage keys
az storage account keys list --name "storage" --resource-group "RG-Name"

# Create blob container
az storage container create --name "container" --account-name "storage"

# Upload blob
az storage blob upload --container-name "container" --name "blob" --file "local-file" --account-name "storage"
```

### RBAC
```bash
# Assign role
az role assignment create --assignee "user@domain.com" --role "Contributor" --scope "/subscriptions/subscription-id"

# List role assignments
az role assignment list --resource-group "RG-Name"

# Create custom role
az role definition create --role-definition role.json
```

### Azure Monitor
```bash
# Create metric alert
az monitor metrics alert create --name "HighCPU" --resource-group "RG-Name" --scopes "/subscriptions/sub-id/resourceGroups/RG/providers/Microsoft.Compute/virtualMachines/myVM" --condition "avg Percentage CPU > 80" --window-size 5m --evaluation-frequency 1m

# List alerts
az monitor alert list --resource-group "RG-Name"

# Get diagnostic settings
az monitor diagnostic-settings list --resource "/subscriptions/sub-id/resourceGroups/RG/providers/Microsoft.Compute/virtualMachines/myVM"
```

---

## Key Concepts Summary

### Azure AD / Entra ID
- **Tenant**: Container for users, groups, apps
- **User Types**: Cloud-only, Hybrid, Guest, B2B
- **Groups**: Security, Distribution, Microsoft 365
- **Service Principal**: App identity in Azure AD
- **OAuth 2.0/SAML**: Authentication protocols

### RBAC Structure
```
Scope (Management Group → Subscription → RG → Resource)
    ↓
Role Definition (Owner, Contributor, Reader, etc.)
    ↓
Security Principal (User, Group, Service Principal)
```

### Storage Account Design
```
Subscription
    ↓
Storage Account (provides endpoint)
    ↓
Service (Blob, File, Queue, Table)
    ↓
Container/Share/Queue/Table
    ↓
Data (Blobs, Files, Messages, Entities)
```

### Network Architecture
```
VNet (10.0.0.0/16)
    ├─ Subnet1 (10.0.1.0/24)
    │   ├─ VM1 (10.0.1.4)
    │   └─ NSG Rules (Inbound/Outbound)
    ├─ Subnet2 (10.0.2.0/24)
    │   ├─ VM2 (10.0.2.5)
    │   └─ NSG Rules
    └─ Gateway Subnet (10.0.255.0/27)
        └─ VPN/ER Gateway
```

### Monitoring Stack
```
Azure Monitor
├─ Metrics (time-series data)
├─ Logs (detailed events)
├─ Alerts (automated responses)
└─ Dashboards (visualization)
```

---

## Performance Considerations

### VM Performance Tiers
- **Standard**: General workloads, burstable
- **Premium**: High-performance, consistent
- **Ultra**: Maximum performance, specific scenarios

### Storage Performance
- **Ultra SSD**: >100,000 IOPS
- **Premium SSD**: 5,000-20,000 IOPS
- **Standard SSD**: 500-6,000 IOPS
- **Standard HDD**: 500 IOPS

### Network Performance
- **Accelerated Networking**: Up to 100 Gbps
- **ExpressRoute**: 50 Mbps to 10 Gbps
- **VPN Gateway**: 100-200 Mbps
- **Standard Bandwidth**: 1 Gbps (app limited)

---

## High Availability Patterns

### Availability Set
```
Availability Set
├─ Update Domain 1
│   ├─ VM1
│   └─ VM3
├─ Update Domain 2
│   ├─ VM2
│   └─ VM4
└─ Fault Domain 1-3
    (Rack-level redundancy)
```

### Availability Zones
```
Region (East US)
├─ Zone 1 (Data center 1)
│   └─ VM1, Storage
├─ Zone 2 (Data center 2)
│   └─ VM2, Storage
└─ Zone 3 (Data center 3)
    └─ VM3, Storage
```

---

## Cost Optimization Tips

✓ **Right-size VMs** - Monitor actual usage
✓ **Use Reserved Instances** - Save 25-72%
✓ **Spot VMs** - Save up to 90% (interruptible)
✓ **LRS instead of GRS** - If local redundancy sufficient
✓ **Cool/Archive tiers** - For infrequent data
✓ **Idle resources** - Delete unused resources
✓ **Autoscaling** - Scale based on demand
✓ **Stop VMs** - Don't delete if might reuse
✓ **Hybrid benefits** - If you have on-premises licenses

---

## Troubleshooting Checklist

### VM Connectivity Issues
- [ ] Check NSG inbound rules
- [ ] Verify network interface IP configuration
- [ ] Check routing tables
- [ ] Verify DNS resolution
- [ ] Check firewall rules inside VM

### Storage Access Issues
- [ ] Verify storage account firewall rules
- [ ] Check service endpoints vs private endpoints
- [ ] Verify SAS token expiration
- [ ] Check storage account keys
- [ ] Verify container/share permissions

### Performance Issues
- [ ] Check CPU/Memory/Disk metrics
- [ ] Review diagnostic logs
- [ ] Check network throughput
- [ ] Analyze slow queries (if DB)
- [ ] Review Application Insights traces

### Authentication Issues
- [ ] Verify RBAC assignments
- [ ] Check Azure AD group membership
- [ ] Verify service principal credentials
- [ ] Check access token expiration
- [ ] Verify tenant/subscription context

---

## Exam Scenario Examples

### Scenario 1: Secure Multi-Tier Application
**Requirement**: Web tier accessible from internet, app tier private, database tier very restricted

**Solution**:
1. Create VNet with 3 subnets (Web, App, DB)
2. Create 3 NSGs with appropriate rules
3. Web NSG: Allow 80, 443 from Internet
4. App NSG: Allow 8080 from Web NSG only
5. DB NSG: Allow 3306 from App NSG only
6. Use Application Gateway for Layer 7 routing

### Scenario 2: Hybrid Connectivity
**Requirement**: Connect on-premises network to Azure

**Solution**:
1. Option A: Site-to-Site VPN
   - Create VPN Gateway in Azure
   - Configure on-premises VPN device
   - Create connection
   
2. Option B: Express Route
   - Order circuit from provider
   - Create private peering
   - Route traffic through circuit

### Scenario 3: High-Availability Web App
**Requirement**: 99.95% SLA, automatic recovery from failures

**Solution**:
1. Create 2+ VMs in different Availability Zones
2. Place behind Azure Load Balancer
3. Configure health probes (TCP/HTTP)
4. Set up auto-scaling rules
5. Enable Azure Backup
6. Configure Azure Site Recovery

### Scenario 4: Data Migration
**Requirement**: Move large dataset (10 TB) to Azure Storage

**Solution**:
1. Create storage account with appropriate tier
2. Option A: Use AzCopy for smaller data (<1 TB)
3. Option B: Use Azure Data Box for large data
4. Verify data integrity with checksums
5. Delete source data after verification

---

## Interview/Exam Preparation Questions

1. **Describe RBAC hierarchy** - Scope, roles, principals
2. **Explain VNet peering vs. VPN** - Transitive, encryption, cost
3. **Compare storage account types** - Use cases, limitations
4. **Design HA/DR architecture** - RPO, RTO, recovery strategy
5. **NSG rule processing order** - Inbound vs outbound, priority
6. **Backup recovery scenarios** - Data retention, RPO options
7. **Load balancer vs. App Gateway** - Layer, routing capabilities
8. **VPN Gateway connection types** - S2S, P2S, VNet-to-VNet
9. **Blob storage lifecycle** - Transitions between tiers
10. **Monitoring strategy** - Metrics vs. logs, alerting

---

## Last-Minute Review Points

✓ Know subnet math (CIDR notation)
✓ Understand NSG rule priority (lower number = higher priority)
✓ Remember storage replication options (LRS < ZRS < GRS < GZRS)
✓ Blade terminology in Azure Portal
✓ Difference between managed and unmanaged disks
✓ Service Principal vs. Managed Identity
✓ Virtual Network Gateway vs. VPN Gateway
✓ Soft delete vs. hard delete for resources
✓ Public vs. Private IP addresses
✓ Default routes in Azure routing tables

---

**Study Time**: 40-60 hours recommended
**Hands-on Lab Time**: 20-30 hours minimum
**Practice Exams**: 3-5 full length tests

