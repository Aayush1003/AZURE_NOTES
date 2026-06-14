# AZ-204 Quick Cheat Sheet

## Essential .NET & SDK Commands

### Azure SDK Initialization
```csharp
// Default credential (uses environment, managed identity, or CLI)
var credential = new DefaultAzureCredential();

// Specific credential types
var servicePrincipalCredential = new ClientSecretCredential(tenantId, clientId, clientSecret);
var managedIdentityCredential = new DefaultAzureCredential(
    new DefaultAzureCredentialOptions { ManagedIdentityClientId = "client-id" });
```

### Storage Operations Quick Ref
```csharp
// Blob
var blobClient = new BlobClient(uri, credential);
await blobClient.UploadAsync(stream, overwrite: true);
var download = await blobClient.DownloadAsync();

// Queue
var queueClient = new QueueClient(uri, credential);
await queueClient.SendMessageAsync("message");
var message = await queueClient.ReceiveMessageAsync();

// Table
var tableClient = new TableClient(uri, "tableName", credential);
await tableClient.AddEntityAsync(entity);
await foreach (var entity in tableClient.QueryAsync<TableEntity>()) { }
```

---

## Core Concepts

### Trigger & Binding Types
| Trigger | Binding | Common Use |
|---------|---------|-----------|
| HTTP | Input/Output | REST APIs |
| Timer | - | Scheduled tasks |
| Blob | Input/Output | File processing |
| Queue | Input/Output | Background jobs |
| Service Bus | Input/Output | Async messaging |
| Event Hub | - | Event streaming |
| Event Grid | - | Event routing |
| Cosmos DB | Input/Output | Document DB |
| Managed Identity | - | Secure auth |

### Authentication Patterns
```
1. Client Credentials: Service-to-service (no user)
2. Auth Code: User login with browser
3. ROPC: Username/password (not recommended)
4. Managed Identity: Azure service to Azure resource
5. Certificate: App authentication via cert
```

### Async Patterns
```csharp
// Good
async Task<IActionResult> GetAsync() 
{
    var data = await _service.GetDataAsync();
    return Ok(data);
}

// Bad (blocks)
IActionResult Get() 
{
    var data = _service.GetDataAsync().Result;
    return Ok(data);
}
```

---

## Caching Strategy
```csharp
// 1. Check cache
var cached = cache.Get(key);
if (cached != null) return cached;

// 2. Get from source
var data = await GetFromSourceAsync();

// 3. Store in cache
cache.Set(key, data, expiration);
return data;
```

---

## Error Handling
```csharp
try 
{
    // API call
}
catch (Azure.RequestFailedException ex) when (ex.Status == 404)
{
    // Not found
}
catch (Azure.RequestFailedException ex) when (ex.Status == 429)
{
    // Rate limited - retry with backoff
}
catch (OperationCanceledException)
{
    // Timeout
}
```

---

## Service Bus Patterns
```
Queue: Point-to-point (1 sender → 1 receiver)
Topic: Pub/Sub (1 sender → N subscribers)
DLQ: Dead Letter Queue (failed messages)
```

---

## Database Integration
```csharp
// Cosmos DB
CosmosClient client = new("endpoint", "key");
Container container = client.GetContainer("db", "container");
var item = await container.ReadItemAsync<T>(id, new PartitionKey(id));

// SQL Database (EF Core)
using var context = new MyDbContext();
var users = await context.Users.Where(u => u.Age > 25).ToListAsync();
```

---

## Key Exam Facts

✓ Azure Functions: 5 min timeout (consumption plan)
✓ Service Bus: 14-day retention (default)
✓ Event Hub: 24-hour retention (default)
✓ Cosmos DB: Automatic indexing by default
✓ Key Vault: 7-day soft delete retention
✓ API Management: Supports OpenAPI, GraphQL
✓ App Service: IIS on Windows, custom Linux
✓ Managed Identity: No secrets to manage

---

## Performance Checklist
- [ ] Use async/await throughout
- [ ] Implement caching for frequently accessed data
- [ ] Use connection pooling for databases
- [ ] Batch operations when possible
- [ ] Use compression for large payloads
- [ ] Implement request throttling/rate limiting
- [ ] Monitor with Application Insights
- [ ] Use CDN for static content
- [ ] Enable query result caching
- [ ] Optimize LINQ queries

---

## Security Checklist
- [ ] Use Managed Identity instead of connection strings
- [ ] Store secrets in Key Vault
- [ ] Validate and sanitize all inputs
- [ ] Use HTTPS/TLS everywhere
- [ ] Implement CORS properly
- [ ] Use JWT for API authentication
- [ ] Implement rate limiting
- [ ] Log security events
- [ ] Use parameterized queries (prevent SQL injection)
- [ ] Encrypt sensitive data at rest and in transit

---

## Testing Patterns
```csharp
// Unit test with mock
[Fact]
public async Task GetUser_ReturnsUser_WhenFound()
{
    var mockRepository = new Mock<IUserRepository>();
    mockRepository.Setup(r => r.GetUserAsync(1))
        .ReturnsAsync(new User { Id = 1, Name = "John" });

    var service = new UserService(mockRepository.Object);
    var user = await service.GetUserAsync(1);

    Assert.NotNull(user);
    Assert.Equal("John", user.Name);
}

// Integration test
[Fact]
public async Task GetUser_ReturnsUser_FromDatabase()
{
    using var context = new TestDbContext();
    context.Users.Add(new User { Id = 1, Name = "John" });
    await context.SaveChangesAsync();

    var service = new UserService(context);
    var user = await service.GetUserAsync(1);

    Assert.Equal("John", user.Name);
}
```

---

## Deployment

### Using Azure CLI
```bash
# Deploy App Service
az webapp up --name myapp --resource-group mygroup --plan myplan

# Deploy Function App
func azure functionapp publish myFunctionApp

# Deploy Container
az container create --image myregistry.azurecr.io/myapp:latest
```

### Using ARM Templates
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2021-01-15",
      "name": "[parameters('appName')]"
    }
  ]
}
```

---

## Troubleshooting Commands
```bash
# Check app logs
az webapp log tail --name myapp --resource-group mygroup

# Get deployment history
az webapp deployment list --name myapp --resource-group mygroup

# Test connection
Test-NetConnection -ComputerName api.example.com -Port 443
```

