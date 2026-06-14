# Domain 1: Manage Azure Identities and Governance (25-30%)

## 1. Azure Active Directory (Azure Entra ID)

### Overview
**Azure Entra ID** (formerly Azure AD) is Microsoft's cloud-based identity and access management service.

### Key Concepts
- **Azure Entra ID**: Identity and access management service (formerly Azure AD)
- **Tenant**: An instance of Azure Entra, represents a company
- **Directory**: Same as tenant in Azure context
- **Applications**: Registered apps in Azure Entra
- **Service Principals**: Identity for applications (use Managed Identity instead)
- **Managed Identity**: Service principal for Azure resources (recommended approach)

### User Types
| Type | Description | License Requirement |
|------|-------------|-------------------|
| Cloud-only | User only in Azure AD | Optional |
| Hybrid | Synced from on-premises AD | Optional |
| Guest | External user invited | Optional |
| B2B | Business to business user | Depends on app |

### User Management Tasks
```powershell
# Create user
New-AzADUser -DisplayName "John Doe" -UserPrincipalName "john@domain.onmicrosoft.com" -Password (ConvertTo-SecureString -AsPlainText "P@ssw0rd" -Force)

# Remove user
Remove-AzADUser -ObjectId <user-id>

# Reset password
Set-AzADUserPassword -ObjectId <user-id> -Password (ConvertTo-SecureString -AsPlainText "NewPassword" -Force) -Permanent
```

---

## 2. Groups Management

### Group Types
- **Security Groups**: Manage access to resources (O365, Applications, Cloud resources)
- **Distribution Groups**: Email distribution (via Exchange)
- **Microsoft 365 Groups**: Unified collaboration (Teams, SharePoint, Planner)

### Group Management
```powershell
# Create security group
New-AzADGroup -DisplayName "IT Team" -MailNickname "itteam"

# Add member to group
Add-AzADGroupMember -TargetGroupObjectId <group-id> -MemberObjectId <user-id>

# List group members
Get-AzADGroupMember -GroupObjectId <group-id>

# Remove member
Remove-AzADGroupMember -GroupObjectId <group-id> -MemberObjectId <user-id>
```

### Dynamic Membership
- Rules-based automatic membership
- Based on user properties
- Rules must have proper syntax

---

## 3. RBAC (Role-Based Access Control)

### Core Components
1. **Security Principal**: User, group, or service principal
2. **Role Definition**: Collection of permissions
3. **Scope**: Resource, resource group, subscription, management group

### Built-in Roles (Common)
| Role | Access Level | Use Case |
|------|-------------|----------|
| Owner | Full access | Subscription/resource management |
| Contributor | All except access management | Resource operations |
| Reader | Read-only access | Auditing and monitoring |
| User Access Administrator | Manage access only | Permission delegation |
| Subscription Admin (Classic) | Legacy role | Not recommended |

### RBAC Management
```powershell
# Assign role
New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Contributor" -Scope "/subscriptions/<subscription-id>/resourceGroups/<rg-name>"

# List role assignments
Get-AzRoleAssignment -Scope "/subscriptions/<subscription-id>"

# Remove role assignment
Remove-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Contributor" -Scope "/subscriptions/<subscription-id>"

# Create custom role
New-AzRoleDefinition -InputFile ".\customRole.json"
```

### Custom Roles
- Define permissions explicitly
- Assignable at scope level
- Use JSON for definition

---

## 4. Azure AD Conditional Access

### Components
- **Assignments**: Users/groups and cloud apps
- **Conditions**: Device, sign-in risk, client app
- **Access Controls**: Grant (MFA, compliant device) or Block

### Common Policies
1. Require MFA for risky sign-ins
2. Block access from non-compliant devices
3. Require managed devices
4. Block legacy authentication

---

## 5. Governance

### Management Groups
```
Root
├── Management Group 1
│   ├── Subscription A
│   └── Subscription B
└── Management Group 2
    └── Subscription C
```

- Organizational hierarchy
- Apply policies at group level
- Up to 6 levels deep

### Azure Policy
```powershell
# Assign policy
New-AzPolicyAssignment -Name "RequireTagPolicy" -PolicyDefinition $policyDefinition -Scope "/subscriptions/<subscription-id>"

# View policy assignments
Get-AzPolicyAssignment

# Remove policy
Remove-AzPolicyAssignment -Name "RequireTagPolicy" -Scope "/subscriptions/<subscription-id>"
```

### Azure Blueprints
- Templates for environment deployment
- Includes: ARM templates, RBAC, policies
- Versioning support
- Track deployments

### Resource Locks
```powershell
# Create lock
New-AzManagementLock -LockLevel CanNotDelete -LockName "ResourceLock" -ResourceGroupName "RG-Name"

# Delete lock
Remove-AzManagementLock -LockName "ResourceLock" -ResourceGroupName "RG-Name"
```

**Lock Types:**
- `CanNotDelete`: Can modify but not delete
- `ReadOnly`: Can view but not modify or delete

### Resource Tags
```powershell
# Apply tags
$tags = @{"Environment"="Production"; "Owner"="TeamA"}
Set-AzResource -ResourceId $resourceId -Tag $tags
```

---

## 6. Subscriptions & Billing

### Subscription Types
- **Free**: Limited services, free tier
- **Pay-As-You-Go**: Hourly billing
- **Enterprise Agreement**: Volume licensing
- **Cloud Solution Provider**: Through partners

### Cost Management
- **Budget Alerts**: Set spending limits
- **Cost Analysis**: Review spending
- **Recommendations**: Cost optimization suggestions

### Subscription Transfers
- Transfer to new Azure AD tenant
- Change billing account
- Transfer between subscriptions

---

## Key Exam Topics
✓ Azure AD user and group management
✓ RBAC implementation and troubleshooting
✓ Conditional Access policies
✓ Management Groups and policies
✓ Resource locks and tags
✓ Subscription management
✓ Cost management and budgeting
✓ Azure AD authentication methods (MFA, passwordless)

