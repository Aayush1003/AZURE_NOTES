# Domain 3: Deploy and Manage Azure Compute Resources (20-25%)

## 1. Virtual Machines (VMs)

### VM Components
- **Disk**: OS disk + data disks
- **NIC**: Network interface card
- **IP Configuration**: Public/Private IP
- **Network Security Group**: Firewall rules
- **Tags**: For organization

### VM Sizes
```
Compute Optimized (F-series): High CPU ratio
Memory Optimized (E-series): High memory ratio
Storage Optimized (L-series): High disk throughput
General Purpose (D/A-series): Balanced CPU, memory, storage
GPU (N-series): Graphics processing
High Performance (H-series): High CPU, memory, fast networking
```

### VM Creation
```powershell
# Create VM
$cred = Get-Credential
$vmConfig = New-AzVMConfig -VMName "myVM" -VMSize "Standard_D2s_v3"
$vmConfig = Set-AzVMOperatingSystem -VM $vmConfig -Windows -ComputerName "myVM" -Credential $cred
$vmConfig = Set-AzVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2022-Datacenter" -Version "latest"
$vmConfig = Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id
New-AzVM -ResourceGroupName "RG-Name" -VM $vmConfig

# Stop VM
Stop-AzVM -ResourceGroupName "RG-Name" -Name "myVM"

# Start VM
Start-AzVM -ResourceGroupName "RG-Name" -Name "myVM"

# Delete VM
Remove-AzVM -ResourceGroupName "RG-Name" -Name "myVM"
```

### VM Images
- **Marketplace Images**: Publisher-provided
- **Custom Images**: Generalized VMs
- **Shared Image Gallery**: Versioned templates

### Disk Management
```powershell
# Create managed disk
$disk = New-AzDiskConfig -Location "East US" -CreateOption Empty -DiskSizeGB 128 -SkuName "Premium_LRS"
New-AzDisk -ResourceGroupName "RG-Name" -DiskName "myDisk" -Disk $disk

# Attach disk to VM
$vm = Get-AzVM -ResourceGroupName "RG-Name" -Name "myVM"
Add-AzVMDataDisk -VM $vm -Name "myDisk" -ManagedDiskId $disk.Id -Lun 0 -CreateOption Attach
Update-AzVM -ResourceGroupName "RG-Name" -VM $vm

# Snapshot
New-AzSnapshot -ResourceGroupName "RG-Name" -SnapshotName "mySnapshot" -Snapshot (New-AzSnapshotConfig -SourceUri $disk.Id -CreateOption Copy -Location "East US")
```

### Disk Types
| Type | Performance | Cost | Use Case |
|------|-------------|------|----------|
| **Ultra SSD** | Highest IOPS/throughput | Highest | High-performance DB |
| **Premium SSD** | High performance | High | Production workloads |
| **Standard SSD** | Balanced | Medium | General workloads |
| **Standard HDD** | Lower performance | Low | Dev/test, backups |

---

## 2. Virtual Machine Scale Sets (VMSS)

### Overview
- Auto-scaling group of identical VMs
- Load balancer integration
- Automatic patching
- Orchestration modes: Uniform, Flexible

### VMSS Creation
```powershell
# Create scale set
$vmssConfig = New-AzVmssConfig -Location "East US" -SkuCapacity 2 -SkuName "Standard_D2s_v3" -UpgradePolicyMode "Automatic"
Set-AzVmssOsProfile -VirtualMachineScaleSet $vmssConfig -AdminUsername "azureuser" -AdminPassword "P@ssw0rd1234"
Set-AzVmssStorageProfile -VirtualMachineScaleSet $vmssConfig -OsDiskCreateOption FromImage -ImageReferencePublisher "MicrosoftWindowsServer" -ImageReferenceOffer "WindowsServer" -ImageReferenceSku "2022-Datacenter" -ImageReferenceVersion "latest"
New-AzVmss -ResourceGroupName "RG-Name" -Name "myVMSS" -VirtualMachineScaleSet $vmssConfig
```

### Auto-Scaling Rules
```powershell
# Add autoscale setting
$rule = New-AzAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId "/subscriptions/.../vmss" -Operator GreaterThan -MetricStatistic Average -Threshold 70 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1

New-AzAutoscaleSetting -Location "East US" -Name "myScaleSetting" -ResourceGroup "RG-Name" -TargetResourceId "/subscriptions/.../vmss" -AutoscaleRule $rule -MinimumCapacity 1 -MaximumCapacity 5 -DefaultCapacity 2
```

---

## 3. Container Services

### Azure Container Instances (ACI)
- Fastest, simplest way to run containers
- Per-second billing
- No orchestration overhead

```powershell
# Deploy container
New-AzContainerGroup -ResourceGroupName "RG-Name" -Name "myContainer" -Image "nginx:latest" -OsType Linux -Port @(80,443) -DnsNameLabel "myapp"
```

### Azure Container Registry (ACR)
- Private Docker registry
- Geo-replication
- Build tasks
- Webhook support

```powershell
# Create registry
New-AzContainerRegistry -ResourceGroupName "RG-Name" -Name "myregistry" -Sku "Standard" -Location "East US"

# Push image
Push-AzContainerImage -ImageUri "myregistry.azurecr.io/myapp:1.0" -Tag "latest"
```

### Azure Kubernetes Service (AKS)
- Managed Kubernetes cluster
- Auto-scaling nodes
- Integrated monitoring
- Azure Policy support

```powershell
# Create AKS cluster
New-AzAksCluster -ResourceGroupName "RG-Name" -Name "myAKS" -NodeCount 3 -ServicePrincipalIdAndSecret $spIdAndSecret -KubernetesVersion "1.27.0"

# Get credentials
Import-AzAksCredential -ResourceGroupName "RG-Name" -Name "myAKS" -Force
```

---

## 4. App Service

### App Service Plans
| Tier | Auto-scale | Custom Domain | SSL | Staging |
|------|-----------|--------------|-----|---------|
| **Free** | No | No | No | No |
| **Shared** | No | Yes | Yes | No |
| **Basic** | No | Yes | HTTPS | No |
| **Standard** | Yes | Yes | HTTPS | Yes |
| **Premium** | Yes | Yes | HTTPS | Yes |
| **Isolated** | Yes | Yes | HTTPS | Yes |

### App Service Creation
```powershell
# Create App Service plan
New-AzAppServicePlan -Name "myPlan" -ResourceGroupName "RG-Name" -Location "East US" -Tier "Standard" -NumberOfWorkers 2

# Create web app
New-AzWebApp -Name "myWebApp" -ResourceGroupName "RG-Name" -AppServicePlan "myPlan"

# Deploy code
Publish-AzWebapp -ResourceGroupName "RG-Name" -Name "myWebApp" -ArchivePath "C:\path\to\app.zip"
```

### Deployment Slots
```powershell
# Create slot
New-AzWebAppSlot -ResourceGroupName "RG-Name" -Name "myWebApp" -Slot "staging"

# Swap slots
Switch-AzWebAppSlot -ResourceGroupName "RG-Name" -Name "myWebApp" -SourceSlotName "staging" -DestinationSlotName "production"
```

### Monitoring
- Application Insights
- Log Analytics
- Metrics and Alerts

---

## 5. Batch Processing

### Azure Batch
- Large-scale parallel processing
- Auto-scaling compute nodes
- Task scheduling
- Output persistence

```powershell
# Create batch account
New-AzBatchAccount -AccountName "mybatchaccount" -ResourceGroupName "RG-Name" -Location "East US" -StorageAccountId "/subscriptions/.../storageAccounts/storage"

# Get batch context
$context = Get-AzBatchAccount -AccountName "mybatchaccount" -ResourceGroupName "RG-Name"
```

---

## 6. Function Apps

### Azure Functions
- Serverless compute
- Event-driven
- Pay-per-execution
- Multiple languages (C#, Python, JavaScript, Java)

```powershell
# Create function app
New-AzFunctionApp -Name "myFunctionApp" -ResourceGroupName "RG-Name" -StorageAccountName "storage" -Runtime "dotnet" -RuntimeVersion "6.0" -FunctionsVersion 4 -Location "East US"

# Publish function
Publish-AzFunctionApp -Name "myFunctionApp" -ResourceGroupName "RG-Name" -FilePath "C:\path\to\function.zip"
```

### Triggers & Bindings
- HTTP trigger
- Timer trigger
- Event Hub trigger
- Blob storage trigger
- Queue trigger
- Cosmos DB trigger
- Table storage binding
- SendGrid binding

---

## 7. VM Extensions

### Common Extensions
- **CustomScriptExtension**: Run scripts on VM
- **VMAccessExtension**: Reset credentials
- **DependencyAgentLinux/Windows**: Monitoring
- **DiagnosticsExtension**: Diagnostics data

```powershell
# Add custom script extension
Set-AzVMExtension -ResourceGroupName "RG-Name" -VMName "myVM" -Name "customScript" -Publisher "Microsoft.Compute" -Type "CustomScriptExtension" -TypeHandlerVersion "1.10" -Settings '{"fileUris":["https://url/script.ps1"],"commandToExecute":"powershell -ExecutionPolicy Unrestricted -File script.ps1"}'
```

---

## Key Exam Topics
✓ VM creation, sizing, and management
✓ Disk types and managed disks
✓ VM Scale Sets and auto-scaling
✓ Container services (ACI, ACR, AKS)
✓ App Service and deployment slots
✓ Azure Functions and serverless
✓ Batch processing
✓ VM extensions and customization
✓ Marketplace images vs. custom images
✓ Snapshots and images for disaster recovery

