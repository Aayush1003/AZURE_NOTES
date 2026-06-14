# AZ-104 Quick Cheat Sheet

## Essential PowerShell Commands

### Resource Groups
```powershell
New-AzResourceGroup -Name "RG-Name" -Location "East US"
Get-AzResourceGroup
Remove-AzResourceGroup -Name "RG-Name"
```

### Virtual Machines
```powershell
New-AzVM -ResourceGroupName "RG-Name" -Name "myVM" -Location "East US"
Get-AzVM -ResourceGroupName "RG-Name"
Start-AzVM -ResourceGroupName "RG-Name" -Name "myVM"
Stop-AzVM -ResourceGroupName "RG-Name" -Name "myVM"
Remove-AzVM -ResourceGroupName "RG-Name" -Name "myVM"
```

### Virtual Networks
```powershell
New-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "RG-Name" -Location "East US" -AddressPrefix "10.0.0.0/16"
Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "RG-Name"
```

### Storage Accounts
```powershell
New-AzStorageAccount -ResourceGroupName "RG-Name" -Name "storage" -Location "East US" -SkuName "Standard_LRS" -Kind "StorageV2"
Get-AzStorageAccountKey -ResourceGroupName "RG-Name" -Name "storage"
```

---

## RBAC Quick Reference

### Core Components
- **Security Principal**: User/Group/Service Principal
- **Role Definition**: Set of permissions
- **Scope**: Management Group → Subscription → RG → Resource

### Common Roles
| Role | What It Does |
|------|-------------|
| Owner | Everything |
| Contributor | Everything except manage access |
| Reader | View only |
| User Access Admin | Manage access only |

### Assignment Command
```powershell
New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Contributor" -Scope "/subscriptions/<sub-id>"
```

---

## Networking Essentials

### VNet Addressing
- **Subnets**: /24 = 251 usable IPs, /28 = 11 usable IPs
- **First 3 IPs**: Reserved (.0, .1, .2)
- **Last IP**: Broadcast (.255)

### NSG Rules
- **Priority**: 100 (highest) to 4096 (lowest)
- **Direction**: Inbound or Outbound
- **Access**: Allow or Deny

### Load Balancer vs Application Gateway
| Feature | LB | AppGw |
|---------|-----|-------|
| **Layer** | 4 (Transport) | 7 (Application) |
| **Routing** | Port-based | Path/Host-based |
| **Protocols** | TCP/UDP | HTTP/HTTPS |

---

## Storage Tiers & Replication

### Replication Options (High to Low Cost)
1. **GZRS** - Geo-zone redundant (highest durability)
2. **GRS** - Geo redundant
3. **ZRS** - Zone redundant
4. **LRS** - Local redundant (lowest cost)

### Blob Tiers (Hot → Cool → Archive)
| Tier | Access Speed | Retrieval Cost | Storage Cost | Typical Use |
|------|-------------|----------------|-------------|------------|
| Hot | Immediate | Low | High | Frequent access |
| Cool | Immediate | Medium | Medium | 30+ days |
| Archive | Hours/Minutes | High | Low | 90+ days |

---

## Exam Quick Facts

### Subscription Limits
- **VNets per subscription**: 50 (default, can increase)
- **NSGs per subscription**: 100
- **Public IPs per subscription**: 60
- **VMs per subscription**: 20 (default)

### VM Limits
- **Max data disks**: 64 (varies by size)
- **Max NICs**: 8 (varies by size)
- **Max IP configs per NIC**: 256

### Storage Account Limits
- **Max size**: 5 PB
- **Replication types**: LRS, ZRS, GRS, GZRS
- **Access tiers**: Hot, Cool, Archive

### Backup & Recovery
- **VM snapshots**: Stored in source region
- **Managed backup retention**: 0-35 years
- **RPO options**: 24h, 7d, 30d, 365d

---

## Monitoring & Alerts Checklist

✓ **Create Log Analytics Workspace**
✓ **Enable VM Guest Diagnostics**
✓ **Configure Metric Alerts** (CPU, Memory, Disk)
✓ **Set Up Application Insights** (for apps)
✓ **Create Action Groups** (notifications)
✓ **Configure Activity Log Alerts** (admin actions)
✓ **Enable Network Watcher** (traffic analysis)

---

## Deployment Models

### Classic vs ARM
| Aspect | Classic | ARM |
|--------|---------|-----|
| **Deployment** | PowerShell | Templates/Portal/CLI |
| **Updates** | Limited | Full support |
| **Scalability** | Limited | Excellent |
| **RBAC** | No | Yes |
| **Status** | Deprecated | Current |

---

## Must-Know Scenarios

### Scenario: High Availability VM Setup
1. Create **Availability Set** (same VNet, different nodes)
2. Deploy 2+ VMs in Availability Set
3. Use **Load Balancer** for traffic distribution
4. Configure **Health Probes** for VM status
5. Set up **Alerts** for monitoring

### Scenario: Disaster Recovery (DR)
1. Create **Recovery Services Vault**
2. Configure **Azure Site Recovery** (ASR)
3. Set replication from primary to secondary region
4. Define **RTO/RPO** requirements
5. Test **failover** regularly

### Scenario: Secure Access
1. Create **Network Security Group** (NSG)
2. Add **inbound rules** (default deny, allow specific)
3. Apply to **subnet** or **NIC**
4. Use **Application Security Groups** (ASG) for complex rules
5. Monitor with **Network Watcher**

### Scenario: File Sharing
1. Create **Storage Account**
2. Create **File Share** (SMB/NFS)
3. Set up **Network Endpoints** (restrict access)
4. Generate **SAS Token** for temp access
5. Mount on **VMs/On-premises**

---

## Common Mistakes to Avoid

❌ Using GRS for local workloads (unnecessary cost)
❌ Forgetting gateway subnet for VPN/ER
❌ Opening NSG to "0.0.0.0" unnecessarily
❌ Not enabling backup before issues occur
❌ Assigning Owner role when Contributor suffices
❌ Missing DNS resolution for private endpoints
❌ Not testing disaster recovery plans
❌ Ignoring compliance/regulatory requirements
❌ Leaving default credentials unchanged
❌ No monitoring/alerting configured

---

## Exam Tips

✓ **Read carefully** - Understand what's being asked
✓ **Use process of elimination** - Remove obviously wrong answers
✓ **Flag difficult questions** - Come back to them later
✓ **Manage time** - ~2 minutes per question
✓ **Know the tools** - Portal, CLI, PowerShell, ARM templates
✓ **Understand concepts** - Not just memorize commands
✓ **Hands-on practice** - Use Azure free account
✓ **Review weak areas** - Focus on problem domains
✓ **Practice exams** - Identify gaps before real exam

---

## Key Acronyms

| Acronym | Meaning |
|---------|---------|
| **RBAC** | Role-Based Access Control |
| **ARM** | Azure Resource Manager |
| **NSG** | Network Security Group |
| **ASG** | Application Security Group |
| **VNet** | Virtual Network |
| **NIC** | Network Interface Card |
| **LB** | Load Balancer |
| **AppGw** | Application Gateway |
| **VPN** | Virtual Private Network |
| **ER** | Express Route |
| **VDI** | Virtual Desktop Infrastructure |
| **IaaS** | Infrastructure as a Service |
| **PaaS** | Platform as a Service |
| **SaaS** | Software as a Service |
| **LRS** | Locally Redundant Storage |
| **GRS** | Geo Redundant Storage |
| **ZRS** | Zone Redundant Storage |
| **ASR** | Azure Site Recovery |
| **RTO** | Recovery Time Objective |
| **RPO** | Recovery Point Objective |
| **MFA** | Multi-Factor Authentication |
| **SAS** | Shared Access Signature |
| **VNET** | Virtual Network |

---

## Resources for Further Study

- **Microsoft Learn**: https://learn.microsoft.com/certifications/azure-administrator/
- **Official Docs**: https://learn.microsoft.com/azure/
- **Azure Architecture Center**: https://learn.microsoft.com/azure/architecture/
- **Azure CLI Docs**: https://learn.microsoft.com/cli/azure/
- **PowerShell Docs**: https://learn.microsoft.com/powershell/azure/

---

**Last Updated**: June 2026
**Content Version**: Updated with Azure Entra ID terminology
**Difficulty Level**: Intermediate
**Exam Duration**: 120 minutes
**Passing Score**: 700/1000
**Azure Services**: Latest (Entra ID, modern cmdlets, Managed Identity)

