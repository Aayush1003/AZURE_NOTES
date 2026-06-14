# Domain 2: Implement and Manage Storage (15-20%)

## 1. Azure Storage Account

### Overview
- Core storage service for cloud data
- Blob, Queue, Table, File Storage
- Geo-replication options
- Two performance tiers: Standard, Premium

### Storage Account Types
| Type | Description | Use Case |
|------|-------------|----------|
| Standard (GPv2) | General purpose, all services | Most workloads |
| Premium Block Blob | High performance, block blobs | High-throughput applications |
| Premium File Share | High performance, NFS/SMB | High-IOPS file shares |
| Legacy (GPv1) | General purpose v1 | Not recommended |
| Legacy (Blob) | Blob-only storage | Not recommended |

### Replication Options
```
LRS (Locally Redundant Storage)
├─ 3 copies in single region
├─ Lowest cost
└─ Low durability (11 nines)

ZRS (Zone Redundant Storage)
├─ 3 copies across 3 AZs
├─ Medium cost
└─ High durability (12 nines)

GRS (Geo Redundant Storage)
├─ 6 copies (3 primary + 3 secondary region)
├─ Medium cost
└─ High durability (16 nines)

GZRS (Geo Zone Redundant Storage)
├─ 6 copies (ZRS in 2 regions)
├─ Highest cost
└─ Highest durability (16 nines)
```

### Storage Account Creation
```powershell
# Create storage account
New-AzStorageAccount -ResourceGroupName "RG-Name" -Name "storageaccname" -Location "East US" -SkuName "Standard_LRS" -Kind "StorageV2"

# Get storage account keys
Get-AzStorageAccountKey -ResourceGroupName "RG-Name" -Name "storageaccname"

# Regenerate keys
New-AzStorageAccountKey -ResourceGroupName "RG-Name" -Name "storageaccname" -KeyName key1
```

---

## 2. Azure Blob Storage

### Blob Types
| Type | Description | Use Case |
|------|-------------|----------|
| **Block Blob** | 4.75 TB max | Files, documents, media |
| **Page Blob** | 8 TB max | VM disks, VHDs |
| **Append Blob** | 195 GB max | Append-only logs |

### Access Tiers
```
Hot Tier
├─ Frequently accessed data
├─ Highest retrieval cost
└─ Lowest storage cost

Cool Tier
├─ Infrequently accessed (30 days+)
├─ Medium retrieval cost
└─ Medium storage cost

Archive Tier
├─ Rarely accessed (90 days+)
├─ Highest retrieval cost
└─ Lowest storage cost
```

### Blob Management
```powershell
# Upload blob
Set-AzStorageBlobContent -Container "container-name" -File "localfile.txt" -Blob "blobname" -Context $storageContext

# Download blob
Get-AzStorageBlobContent -Container "container-name" -Blob "blobname" -Destination "localpath" -Context $storageContext

# List blobs
Get-AzStorageBlob -Container "container-name" -Context $storageContext

# Delete blob
Remove-AzStorageBlob -Container "container-name" -Blob "blobname" -Context $storageContext
```

### Lifecycle Management
```json
{
  "rules": [
    {
      "name": "archiveRule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "baseBlob": {
            "tierToCool": { "daysAfterModificationGreaterThan": 30 },
            "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
            "delete": { "daysAfterModificationGreaterThan": 365 }
          }
        }
      }
    }
  ]
}
```

---

## 3. Azure Files (File Share)

### Protocols
- **SMB 3.0**: Windows, Linux (3.0.2+), macOS, Azure VMs
- **NFS**: Linux, macOS (Azure Files premium only)

### File Share Management
```powershell
# Create file share
New-AzStorageShare -Name "fileshare" -Context $storageContext

# Upload files
Set-AzStorageFileContent -Share "fileshare" -Source "C:\localfile.txt" -Path "remotedir\" -Context $storageContext

# Mount on Windows
net use Z: \\storageaccname.file.core.windows.net\fileshare /user:Azure\storageaccname <storage-key>
```

### SMB Multichannel (Premium only)
- Multiple TCP connections
- Improved throughput
- Enable on premium file shares

---

## 4. Azure Queue Storage

### Queue Basics
```powershell
# Create queue
New-AzStorageQueue -Name "myqueue" -Context $storageContext

# Add message
$storageContext | Add-AzQueueMessage -QueueName "myqueue" -MessageText "Hello Queue"

# Get message
$storageContext | Get-AzQueueMessage -QueueName "myqueue"

# Delete message
$msg = $storageContext | Get-AzQueueMessage -QueueName "myqueue"
$storageContext | Remove-AzQueueMessage -QueueName "myqueue" -Id $msg.Id -PopReceipt $msg.PopReceipt
```

### Queue Properties
- FIFO order
- Retention: 7 days default
- Visibility timeout: 30 seconds default
- Message size: Max 64 KB

---

## 5. Azure Table Storage

### Table Basics
- NoSQL data store
- Structured data with key-value pairs
- Partition and row key for querying
- Highly scalable

### Partition & Row Keys
- **Partition Key**: Determines distribution
- **Row Key**: Unique within partition
- Combined = unique entity identifier

```powershell
# Create table entity
$entity = New-Object -TypeName Microsoft.Azure.Cosmos.Table.DynamicTableEntity -ArgumentList "PartitionKey", "RowKey"
$entity.Properties.Add("Name", "John")
$entity.Properties.Add("Age", 30)

# Insert entity
$table.Execute([Microsoft.Azure.Cosmos.Table.TableOperation]::Insert($entity))
```

---

## 6. Storage Account Security

### Network Security
```powershell
# Add storage firewall
Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "RG" -Name "storage" -DefaultAction Deny

# Add virtual network rule
Add-AzStorageAccountNetworkRule -ResourceGroupName "RG" -Name "storage" -VirtualNetworkResourceId "/subscriptions/.../subnets/subnet1"

# Allow Azure services
Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "RG" -Name "storage" -Bypass AzureServices
```

### Shared Access Signatures (SAS)
```powershell
# Generate blob SAS
New-AzStorageBlobSASToken -Container "container" -Blob "blob" -Permission "racwd" -ExpiryTime (Get-Date).AddHours(1) -Context $storageContext
```

**SAS Permissions:**
- `r` = Read
- `a` = Add
- `c` = Create
- `w` = Write
- `d` = Delete
- `l` = List

### Encryption
- **At Rest**: AES-256 (default, automatic)
- **In Transit**: HTTPS/TLS 1.2
- **Customer-Managed Keys**: Via Key Vault

### Role-Based Access
```powershell
# Assign role
New-AzRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Storage Blob Data Reader" -Scope "/subscriptions/.../storageAccounts/storage/blobServices/default/containers/container"
```

---

## 7. Storage Account Diagnostics

### Logging
```powershell
# Enable storage analytics
Set-AzStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations All -RetentionDays 7 -Context $storageContext
```

### Monitoring
- Azure Monitor metrics
- Storage Analytics logs
- Alerting rules

---

## 8. Migration Tools

| Tool | Source | Destination | Use Case |
|------|--------|-------------|----------|
| AzCopy | Various | Azure Storage | Large-scale transfers |
| Storage Explorer | Desktop UI | Azure Storage | GUI management |
| Data Box | On-premises/datacenter | Azure | Offline bulk transfer |
| Azure File Sync | On-premises files | Azure Files | Hybrid file sync |

---

## Key Exam Topics
✓ Storage account creation and configuration
✓ Blob storage tiers and lifecycle policies
✓ File shares (SMB/NFS) and mounting
✓ Queue and Table storage basics
✓ SAS tokens and authentication
✓ Storage account security and firewalls
✓ Replication and redundancy options
✓ Monitoring and diagnostics
✓ Data migration and AzCopy

