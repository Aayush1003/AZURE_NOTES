# Domain 1: Develop Azure Compute Solutions (25-30%)

## 1. Azure App Service

### Overview
- Platform as a Service (PaaS)
- Managed hosting for web apps
- Built-in DevOps, security, compliance

### App Service Plans
| Tier | Auto-Scale | Custom Domain | Staging | Slots |
|------|-----------|--------------|---------|-------|
| **Free** | No | No | No | - |
| **Shared** | No | Yes | No | - |
| **Basic** | No | Yes | No | - |
| **Standard** | Yes | Yes | Yes | 5 |
| **Premium** | Yes | Yes | Yes | 20 |
| **Isolated** | Yes | Yes | Yes | 20 |

### Creating App Service
```csharp
// Using Azure SDK for .NET
using Azure.Identity;
using Azure.ResourceManager;
using Azure.ResourceManager.AppService;
using Azure.ResourceManager.AppService.Models;

var armClient = new ArmClient(new DefaultAzureCredential());
var subscription = await armClient.GetDefaultSubscriptionAsync();
var resourceGroupResource = await subscription.GetResourceGroupAsync("RG-Name");

var appServicePlanData = new AppServicePlanData(AzureLocation.EastUS)
{
    Sku = new BillingMeter { Name = "S1", Tier = "Standard" },
    IsLinux = false
};

var appServicePlan = await resourceGroupResource.GetAppServicePlans()
    .CreateOrUpdateAsync(WaitUntil.Completed, "myAppServicePlan", appServicePlanData);

var webAppData = new WebSiteData(AzureLocation.EastUS)
{
    AppServicePlanId = appServicePlan.Value.Id
};

var webApp = await resourceGroupResource.GetWebSites()
    .CreateOrUpdateAsync(WaitUntil.Completed, "myWebApp", webAppData);
```

### Deployment Methods
- **ZIP Deploy**: Upload ZIP file
- **Git**: Push to repo, auto-deploy
- **GitHub Actions**: CI/CD pipeline
- **Azure Pipelines**: CI/CD pipeline
- **FTP**: Traditional deployment
- **Container Registry**: Docker images

```bash
# ZIP deployment
az webapp deployment source config-zip --name "myWebApp" --resource-group "RG-Name" --src "app.zip"

# Git deployment
az webapp deployment source config-local-git --name "myWebApp" --resource-group "RG-Name"
```

### Deployment Slots
```csharp
// Swap slots
var webApp = await resourceGroupResource.GetWebSites().GetAsync("myWebApp");
var swapSlotData = new CsmSlotEntity { TargetSlot = "production" };
await webApp.Value.SwapAsync(WaitUntil.Completed, swapSlotData);
```

---

## 2. Azure Functions

### Overview
- Serverless compute
- Event-driven execution
- Pay-per-execution billing
- Supports multiple languages and runtimes

### Function Types

| Trigger | Description | Use Case |
|---------|-------------|----------|
| **HTTP** | HTTP request | REST APIs, webhooks |
| **Timer** | CRON schedule | Scheduled tasks |
| **Blob** | Storage blob event | File processing |
| **Queue** | Queue message | Background jobs |
| **Event Hub** | Event streaming | Real-time processing |
| **Service Bus** | Message queue | Async messaging |
| **Cosmos DB** | Change feed | Document changes |
| **Event Grid** | Azure event | Event routing |

### Function Development
```csharp
using Azure.Storage.Blobs;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Extensions.Logging;

public static class BlobTriggerFunction
{
    [Function("BlobTrigger")]
    public static void Run(
        [BlobTrigger("samples-workitems/{name}")] Stream stream,
        string name,
        ILogger log)
    {
        log.LogInformation($"C# Blob trigger function processed blob\n Name:{name} \n Size: {stream.Length} Bytes");
    }
}
```

### Bindings & Triggers
```csharp
[Function("QueueTriggerFunction")]
public static void Run(
    [QueueTrigger("myqueue")] string queueItem,
    [Blob("output/{queueItem}")] BlobClient outputBlob,
    ILogger log)
{
    log.LogInformation($"Processing: {queueItem}");
    outputBlob.Upload(BinaryData.FromString(queueItem), overwrite: true);
}
```

### Durable Functions
```csharp
public static async Task<string> RunOrchestrator(
    [OrchestrationTrigger] TaskOrchestrationContext context)
{
    var result1 = await context.CallActivityAsync<string>("Activity1", "input");
    var result2 = await context.CallActivityAsync<string>("Activity2", result1);
    return result2;
}

[Function(nameof(Activity1))]
public static string Activity1([ActivityTrigger] string input) => $"Activity 1: {input}";

[Function(nameof(Activity2))]
public static string Activity2([ActivityTrigger] string input) => $"Activity 2: {input}";
```

---

## 3. Container Solutions

### Azure Container Registry (ACR)
```csharp
using Azure.Containers.ContainerRegistry;

// Create client
var client = new ContainerRegistryClient(
    new Uri("https://myregistry.azurecr.io"),
    new DefaultAzureCredential());

// List repositories
var repositories = client.GetRepositoriesAsync();
await foreach (var repository in repositories)
{
    Console.WriteLine($"Repository: {repository.Name}");
}
```

### Container Instances
```bash
# Deploy container
az container create --resource-group "RG-Name" \
  --name "myContainer" \
  --image "myregistry.azurecr.io/myapp:latest" \
  --registry-login-server "myregistry.azurecr.io" \
  --registry-username "username" \
  --registry-password "password" \
  --dns-name-label "myapp" \
  --ports 80 443
```

### Azure Kubernetes Service (AKS)
```bash
# Create cluster
az aks create --resource-group "RG-Name" \
  --name "myAKS" \
  --node-count 3 \
  --enable-managed-identity

# Deploy app
kubectl create deployment myapp --image=myregistry.azurecr.io/myapp:latest
kubectl expose deployment myapp --type=LoadBalancer --port=80
```

---

## 4. Virtual Machines & Scale Sets

### VM Deployment from Code
```csharp
using Azure.ResourceManager.Compute;
using Azure.ResourceManager.Compute.Models;

var vmData = new VirtualMachineData(AzureLocation.EastUS)
{
    HardwareProfile = new HardwareProfile { VmSize = "Standard_D2s_v3" },
    OSProfile = new OSProfile
    {
        ComputerName = "myVM",
        AdminUsername = "azureuser",
        AdminPassword = "P@ssw0rd1234!"
    },
    StorageProfile = new StorageProfile
    {
        ImageReference = new ImageReference
        {
            Publisher = "MicrosoftWindowsServer",
            Offer = "WindowsServer",
            Sku = "2022-Datacenter",
            Version = "latest"
        }
    }
};

var vm = await resourceGroupResource.GetVirtualMachines()
    .CreateOrUpdateAsync(WaitUntil.Completed, "myVM", vmData);
```

### Virtual Machine Scale Sets
```csharp
var vmssData = new VirtualMachineScaleSetData(AzureLocation.EastUS)
{
    Sku = new ComputeSku { Name = "Standard_D2s_v3", Capacity = 2 }
};

var vmss = await resourceGroupResource.GetVirtualMachineScaleSets()
    .CreateOrUpdateAsync(WaitUntil.Completed, "myVMSS", vmssData);
```

---

## 5. Managed Identity

### System-Assigned Identity
```csharp
// Enable system-assigned identity for App Service
var webAppData = new WebSiteData(AzureLocation.EastUS)
{
    AppServicePlanId = planId,
    Identity = new ManagedServiceIdentity { ManagedServiceIdentityType = ManagedServiceIdentityType.SystemAssigned }
};
```

### User-Assigned Identity
```csharp
// Create user-assigned identity
var identityData = new UserAssignedIdentityData(AzureLocation.EastUS);
var identity = await resourceGroupResource.GetUserAssignedIdentities()
    .CreateOrUpdateAsync(WaitUntil.Completed, "myIdentity", identityData);

// Use with App Service
var webAppData = new WebSiteData(AzureLocation.EastUS)
{
    Identity = new ManagedServiceIdentity
    {
        ManagedServiceIdentityType = ManagedServiceIdentityType.UserAssigned,
        UserAssignedIdentities = { { identity.Value.Id, new UserAssignedIdentity() } }
    }
};
```

### Using Managed Identity
```csharp
// Access Azure resources using managed identity
var credential = new DefaultAzureCredential();
var client = new BlobContainerClient(
    new Uri("https://storage.blob.core.windows.net/container"),
    credential);

var blobs = await client.GetBlobsAsync();
```

---

## Key Exam Topics
✓ App Service hosting and deployment
✓ Deployment slots and staging
✓ Azure Functions triggers and bindings
✓ Durable Functions orchestration
✓ Container Registry and deployment
✓ AKS deployment and management
✓ Managed Identity (system and user-assigned)
✓ VM and VMSS configuration
✓ CI/CD pipeline integration
✓ Scaling and performance optimization

