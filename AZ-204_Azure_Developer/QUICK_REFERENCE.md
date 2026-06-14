# AZ-204 Quick Reference - Commands & Patterns

## Azure SDK Setup Checklist

```csharp
// 1. Install NuGet packages
// Install-Package Azure.Storage.Blobs
// Install-Package Azure.Messaging.ServiceBus
// Install-Package Azure.Security.KeyVault.Secrets
// Install-Package Microsoft.Azure.Cosmos
// Install-Package Microsoft.ApplicationInsights

// 2. Authenticate
var credential = new DefaultAzureCredential();
// OR
var credential = new ClientSecretCredential(tenantId, clientId, clientSecret);
// OR
var credential = new ManagedIdentityCredential(clientId); // For Managed Identity
```

---

## Common Code Patterns

### Async/Await Pattern
```csharp
// Good pattern
public async Task<DataModel> GetDataAsync()
{
    var data = await _client.GetDataAsync();
    return data;
}

// Bad - blocking pattern (avoid)
public DataModel GetData()
{
    var data = _client.GetDataAsync().Result; // BLOCKS - AVOID
    return data;
}

// Parallel async operations
var task1 = _service1.GetDataAsync();
var task2 = _service2.GetDataAsync();
await Task.WhenAll(task1, task2);
var (data1, data2) = (await task1, await task2);
```

### Error Handling Pattern
```csharp
try
{
    // Azure operation
    var result = await client.GetAsync(...);
}
catch (Azure.RequestFailedException ex) when (ex.Status == 404)
{
    // Handle not found
    _logger.LogWarning($"Resource not found: {ex.Message}");
}
catch (Azure.RequestFailedException ex) when (ex.Status == 429)
{
    // Handle throttling - implement retry with backoff
    await Task.Delay(TimeSpan.FromSeconds(5));
}
catch (OperationCanceledException)
{
    // Handle timeout
    _logger.LogError("Operation timed out");
}
catch (Exception ex)
{
    // Log unexpected errors
    _logger.LogError(ex, "Unexpected error occurred");
    throw;
}
```

### Caching Pattern
```csharp
public async Task<User> GetUserAsync(int id)
{
    var cacheKey = $"user:{id}";
    
    // Try cache first
    if (_cache.TryGetValue(cacheKey, out User cachedUser))
    {
        return cachedUser;
    }
    
    // Get from source
    var user = await _repository.GetUserAsync(id);
    
    // Cache with expiration
    var cacheOptions = new MemoryCacheEntryOptions()
        .SetAbsoluteExpiration(TimeSpan.FromMinutes(5));
    _cache.Set(cacheKey, user, cacheOptions);
    
    return user;
}
```

---

## Service Specific Commands

### Azure Functions Local Testing
```bash
# Install tools
npm install -g azure-functions-core-tools@4 --unsafe-perm true

# Create function project
func init myFunctionApp --worker-runtime dotnet
cd myFunctionApp

# Create new function
func new --name MyFunction --template "HTTP trigger"

# Run locally
func start

# Deploy to Azure
func azure functionapp publish myFunctionApp
```

### Docker & Container Commands
```bash
# Build Docker image
docker build -t myapp:latest .

# Tag for registry
docker tag myapp:latest myregistry.azurecr.io/myapp:latest

# Push to ACR
docker push myregistry.azurecr.io/myapp:latest

# Deploy to ACI
az container create --resource-group mygroup \
  --name mycontainer \
  --image myregistry.azurecr.io/myapp:latest \
  --registry-login-server myregistry.azurecr.io \
  --registry-username username \
  --registry-password password \
  --dns-name-label myapp \
  --ports 80 443
```

### Kubernetes (AKS) Commands
```bash
# Get credentials
az aks get-credentials --resource-group mygroup --name myaks

# Deploy application
kubectl apply -f deployment.yaml

# Check deployment status
kubectl get deployment
kubectl get pods

# Scale deployment
kubectl scale deployment myapp --replicas 3

# View logs
kubectl logs <pod-name>

# Port forward
kubectl port-forward svc/myapp 8080:80
```

---

## Connection Strings & Endpoints

### Storage Account
```csharp
// Full connection string
"DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=key;EndpointSuffix=core.windows.net"

// URI + Credential (recommended)
new BlobContainerClient(
    new Uri("https://myaccount.blob.core.windows.net/container"),
    new DefaultAzureCredential());
```

### Service Bus
```csharp
// Connection string
"Endpoint=sb://mynamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=key"

// Modern approach
new ServiceBusClient(
    "mynamespace.servicebus.windows.net",
    new DefaultAzureCredential());
```

### Cosmos DB
```csharp
// Connection string
"AccountEndpoint=https://myaccount.documents.azure.com:443/;AccountKey=key;;"

// Modern approach
new CosmosClient(
    "https://myaccount.documents.azure.com:443/",
    new DefaultAzureCredential());
```

### SQL Database
```csharp
// Connection string
"Server=tcp:myserver.database.windows.net,1433;Initial Catalog=mydb;Persist Security Info=False;User ID=username;Password=password;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

// With Azure Entra ID
"Server=tcp:myserver.database.windows.net,1433;Initial Catalog=mydb;Authentication=Active Directory Default;"
```

---

## Testing Patterns

### Unit Test with Mocks
```csharp
[Fact]
public async Task GetUser_ReturnsUser_WhenFound()
{
    // Arrange
    var mockRepository = new Mock<IUserRepository>();
    mockRepository
        .Setup(r => r.GetUserAsync(1))
        .ReturnsAsync(new User { Id = 1, Name = "John" });
    
    var service = new UserService(mockRepository.Object);
    
    // Act
    var user = await service.GetUserAsync(1);
    
    // Assert
    Assert.NotNull(user);
    Assert.Equal("John", user.Name);
    mockRepository.Verify(r => r.GetUserAsync(1), Times.Once);
}
```

### Integration Test
```csharp
[Fact]
public async Task GetUser_ReturnsUser_FromDatabase()
{
    // Arrange
    using var context = new TestDbContext();
    context.Users.Add(new User { Id = 1, Name = "John" });
    await context.SaveChangesAsync();
    
    var service = new UserService(context);
    
    // Act
    var user = await service.GetUserAsync(1);
    
    // Assert
    Assert.Equal("John", user.Name);
}
```

---

## Performance Tips

✓ Use async/await throughout code
✓ Implement connection pooling for databases
✓ Use caching for frequently accessed data
✓ Batch operations when possible
✓ Lazy load large objects
✓ Use compression for large payloads
✓ Implement proper logging without logging sensitive data
✓ Use Application Insights for monitoring
✓ Profile code to find bottlenecks
✓ Use CDN for static content

---

## Security Checklist

✓ Never hardcode secrets - use Key Vault
✓ Use Managed Identity instead of connection strings
✓ Enable HTTPS/TLS everywhere
✓ Validate and sanitize all inputs
✓ Use parameterized queries (prevent SQL injection)
✓ Implement rate limiting
✓ Log security events
✓ Use strong authentication (OAuth 2.0, OpenID Connect)
✓ Implement CORS properly
✓ Encrypt sensitive data at rest and in transit
✓ Regular security updates and patching

