# Domain 5: Monitor and Maintain Azure Resources (10-15%)

## 1. Azure Monitor

### Core Components
- **Metrics**: Numerical data (CPU %, Memory usage)
- **Logs**: Detailed event data
- **Alerts**: Automated responses to conditions
- **Dashboards**: Visualization of metrics

### Metrics
```powershell
# Get metrics
$metrics = Get-AzMetric -ResourceId $vm.Id -MetricName "Percentage CPU" -TimeGrain (New-TimeSpan -Minutes 1) -StartTime (Get-Date).AddHours(-1) -EndTime (Get-Date)

# Get metric definitions
Get-AzMetricDefinition -ResourceId $vm.Id

# Create metric alert
Add-AzMetricAlertRule -Name "HighCPU" -Location "East US" -ResourceGroup "RG-Name" -TargetResourceId $vm.Id -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 80 -WindowSize 00:05:00 -TimeAggregationOperator Average
```

### Logs & Log Analytics
```powershell
# Create Log Analytics workspace
New-AzOperationalInsightsWorkspace -Name "myWorkspace" -ResourceGroupName "RG-Name" -Location "East US"

# Run KQL query
$query = "Perf | where ObjectName == 'Processor' | summarize AvgCPU = avg(CounterValue) by Computer"
Invoke-AzOperationalInsightsQuery -Workspace $workspace -Query $query -Timespan (New-TimeSpan -Days 1)
```

### Common Metrics
| Resource | Metric | Threshold |
|----------|--------|-----------|
| VM | % Processor Time | > 80% |
| VM | Available Memory | < 20% |
| App Service | Response Time | > 2000ms |
| SQL DB | CPU Percentage | > 90% |
| Storage | Available Capacity | < 10% |

---

## 2. Application Insights

### Overview
- Application performance monitoring
- Web app analytics
- Exception tracking
- Dependency mapping

### Setup
```powershell
# Create Application Insights resource
New-AzApplicationInsights -ResourceGroupName "RG-Name" -Name "myAppInsights" -Location "East US"

# Get instrumentation key
(Get-AzApplicationInsights -ResourceGroupName "RG-Name" -Name "myAppInsights").InstrumentationKey
```

### Monitoring Aspects
- **Availability Tests**: Uptime monitoring
- **Performance Counters**: System metrics
- **Trace Logging**: Custom application logs
- **Exception Tracking**: Error analytics
- **Dependency Map**: Service dependencies

---

## 3. Alerts

### Alert Types
```
Metric Alerts
├─ Static threshold
├─ Dynamic threshold (ML-based)
└─ Composite rules

Log Alerts
├─ Scheduled query
└─ Custom log search

Activity Log Alerts
└─ Administrative actions

Resource Health Alerts
└─ Service health notifications
```

### Alert Rule Creation
```powershell
# Create metric alert
$criterion = New-AzMetricAlertRuleV2CriterionObject -Name "Percentage CPU" -ComparisonOperator GreaterThan -Threshold 80 -TimeAggregationType Average
Add-AzMetricAlertRuleV2 -Name "HighCPUAlert" -ResourceGroupName "RG-Name" -TargetResourceScope $vm.Id -WindowSize (New-TimeSpan -Minutes 5) -Frequency (New-TimeSpan -Minutes 1) -Criterion $criterion -Description "Alert when CPU > 80%"

# Create action group
$actionGroup = New-AzActionGroup -Name "myActionGroup" -ResourceGroupName "RG-Name" -ShortName "mg"

# Add email recipient
Add-AzActionGroupEmailReceiver -ActionGroup $actionGroup -Name "EmailAdmin" -EmailReceiver "admin@example.com"
Set-AzActionGroup -InputObject $actionGroup
```

### Notification Types
- Email
- SMS
- Push notifications
- Webhooks
- Logic Apps
- Automation Runbooks

---

## 4. Activity Log

### What It Tracks
- Resource creation/modification/deletion
- RBAC changes
- Policy changes
- Service health events
- Operations on resources

### Query Activity Log
```powershell
# Get activity log entries
Get-AzActivityLog -ResourceGroupName "RG-Name" -StartTime (Get-Date).AddDays(-7) -EndTime (Get-Date)

# Filter by caller
Get-AzActivityLog -Caller "user@example.com" -StartTime (Get-Date).AddDays(-1)

# Export to CSV
Get-AzActivityLog -ResourceGroupName "RG-Name" | Export-Csv -Path "activity.csv"
```

### Activity Log Alerts
```powershell
# Create activity log alert
$condition = New-AzActivityLogAlertCondition -Status "Succeeded" -OperationName "Microsoft.Compute/virtualMachines/write"
New-AzActivityLogAlert -Name "VMCreation" -ResourceGroupName "RG-Name" -Scope "/subscriptions/$sub/resourceGroups/RG-Name" -Condition $condition -Action (New-AzActivityLogAlertActionGroup -ActionGroupId $actionGroup.Id)
```

---

## 5. Log Analytics

### KQL Basics
```kusto
// Simple query
Perf
| where Computer == "myVM"
| where CounterName == "% Processor Time"

// Aggregation
Perf
| summarize AvgCPU = avg(CounterValue) by Computer, bin(TimeGenerated, 5m)

// Join
Perf
| join (Event | where EventID == 1000) on Computer
| project Computer, CounterValue, RenderedDescription

// Time series
Perf
| make-series AvgCPU = avg(CounterValue) default=0 on TimeGenerated from ago(7d) to now() step 1h by Computer
```

### Common Log Sources
- **Perf**: Performance counter data
- **Event**: Windows event logs
- **SecurityEvent**: Security events
- **Syslog**: Linux logs
- **AzureActivity**: Azure audit logs
- **AppTraces**: Application traces
- **AppRequests**: HTTP requests

---

## 6. Diagnostics Settings

### Diagnostic Settings Configuration
```powershell
# Enable diagnostics for VM
$diagnostic = New-AzDiagnosticSettingOperation -LogAnalyticsDestinationType "Dedicated" -LogAnalyticsWorkspaceResourceId $workspace.ResourceId

# Create diagnostic setting
New-AzDiagnosticSetting -Name "vmDiagnostics" -ResourceId $vm.Id -WorkspaceId $workspace.ResourceId -Enabled $true

# Log retention
Set-AzDiagnosticSetting -Name "vmDiagnostics" -ResourceId $vm.Id -RetentionEnabled $true -RetentionInDays 30
```

### Metrics to Collect
- VM guest diagnostics
- Event logs
- Performance counters
- Application logs
- IIS logs
- ETW traces

---

## 7. Azure Backup

### Backup Services
```
Azure Backup
├─ Azure Backup (Vault)
├─ Azure Site Recovery
└─ Managed Disks snapshots
```

### VM Backup
```powershell
# Create recovery services vault
$vault = New-AzRecoveryServicesVault -Name "myVault" -ResourceGroupName "RG-Name" -Location "East US"

# Set backup policy
$policy = Get-AzRecoveryServicesBackupProtectionPolicy -Name "DefaultPolicy" -VaultId $vault.ID
Enable-AzRecoveryServicesBackupProtection -ResourceId $vm.Id -Policy $policy -VaultId $vault.ID

# Trigger backup
$backupJob = Backup-AzRecoveryServicesBackupItem -Item $item -VaultId $vault.ID

# Restore VM
Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp -VaultId $vault.ID -StorageAccountName $storage -StorageAccountResourceGroupName "RG-Name"
```

### Backup Types
| Type | RPO | Retention |
|------|-----|-----------|
| **Daily** | 24 hours | 0-35 years |
| **Weekly** | 7 days | 0-104 weeks |
| **Monthly** | 30 days | 0-1188 months |
| **Yearly** | 365 days | 0-99 years |

---

## 8. Azure Site Recovery (ASR)

### Replication Scenarios
- Azure to Azure
- VMware to Azure
- Hyper-V to Azure
- Physical servers to Azure

### ASR Setup
```powershell
# Create recovery services vault
$vault = New-AzRecoveryServicesVault -Name "myVault" -ResourceGroupName "RG-Name" -Location "East US"

# Enable replication
$replicationPolicy = New-AzRecoveryServicesAsrPolicy -AzureToAzure -Name "myPolicy" -RecoveryPointRetentionInDays 7 -ApplicationConsistentSnapshotFrequencyInHours 4

# Start replication
New-AzRecoveryServicesAsrReplicationProtectedItem -AzureToAzure -AzureVmId $vm.Id -Name "myVM" -Policy $replicationPolicy -RecoveryResourceGroupId $rg.Id -RecoveryVirtualNetworkId $vnet.Id -RecoverySubnetName "subnet1" -ProtectionContainerName "container1"
```

### RTO & RPO
- **RTO**: Recovery Time Objective (max downtime)
- **RPO**: Recovery Point Objective (max data loss)

---

## 9. Update Management

### Patch Management
```powershell
# Enable Update Management
New-AzAutomationAccount -ResourceGroupName "RG-Name" -Name "myAutomation" -Location "East US"

# Create update deployment
New-AzAutomationSoftwareUpdateConfiguration -AzureModuleClass "AzModule" -Schedule $schedule -WindowDuration 02:00:00 -AzureQuery $query -IncludedUpdateClassification "Critical", "Security"
```

### Patching Options
- **Manual**: Apply patches manually
- **Automatic**: Deploy on a schedule
- **Just-in-time**: Apply when VMs start

---

## 10. Performance Tuning

### VM Performance
- **Disk**: Use premium disks for production
- **Network**: Accelerated Networking
- **CPU**: Right-size VM for workload
- **Memory**: Monitor and adjust allocation

### Optimization Tools
```powershell
# Get resource recommendations
Get-AzAdvisor -Category Performance

# Check VM extensions
Get-AzVMExtension -ResourceGroupName "RG-Name" -VMName "myVM"
```

---

## Key Exam Topics
✓ Azure Monitor metrics and logs
✓ Application Insights setup and usage
✓ Alert rules and action groups
✓ Activity log analysis
✓ Log Analytics and KQL queries
✓ Diagnostic settings
✓ Azure Backup and recovery
✓ Azure Site Recovery for DR
✓ Update Management
✓ Resource health and service health
✓ Troubleshooting common issues
✓ Performance optimization
✓ Cost analysis and optimization

