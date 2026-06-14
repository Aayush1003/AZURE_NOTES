# Domain 4: Monitor, Troubleshoot, and Optimize Solutions (15-20%)

## 1. Application Insights

### Integration with Applications
```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

// In ASP.NET Core Startup
services.AddApplicationInsightsTelemetry("instrumentation-key");

// Inject and use in controller
private TelemetryClient _telemetryClient;

public MyController(TelemetryClient telemetryClient)
{
    _telemetryClient = telemetryClient;
}

public IActionResult MyAction()
{
    // Track event
    _telemetryClient.TrackEvent("ButtonClicked", 
        new Dictionary<string, string> { { "page", "home" } },
        new Dictionary<string, double> { { "duration", 100 } });

    // Track exception
    try
    {
        // Code that might throw
    }
    catch (Exception ex)
    {
        _telemetryClient.TrackException(ex);
    }

    // Track metric
    _telemetryClient.GetMetric("Items").TrackValue(42);

    return Ok();
}
```

### Custom Metrics & Logs
```csharp
// Log custom metric
var metric = _telemetryClient.GetMetric("OrderValue");
metric.TrackValue(order.Total);

// Track trace
_telemetryClient.TrackTrace("Processing order", 
    SeverityLevel.Information,
    new Dictionary<string, string> { { "orderId", "123" } });

// Track dependency (external service call)
_telemetryClient.TrackDependency("SQL", "Database.Query", 
    DateTime.UtcNow, TimeSpan.FromSeconds(2), success: true);
```

### Analytics Queries
```kusto
// Get request count by endpoint
requests
| summarize Count = count() by name
| top 10 by Count

// Track exceptions over time
exceptions
| where type == "MyException"
| summarize count() by bin(timestamp, 1h)

// Performance analysis
requests
| where duration > 1000
| summarize AvgDuration = avg(duration), MaxDuration = max(duration) by name

// Failed requests
requests
| where success == false
| summarize Count = count() by resultCode, name
```

---

## 2. Logging & Diagnostics

### Application Logging
```csharp
using Microsoft.Extensions.Logging;

// In service
public class MyService
{
    private readonly ILogger<MyService> _logger;

    public MyService(ILogger<MyService> logger)
    {
        _logger = logger;
    }

    public void ProcessOrder(Order order)
    {
        _logger.LogInformation("Processing order {OrderId}", order.Id);
        
        try
        {
            // Process logic
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to process order {OrderId}", order.Id);
        }
    }
}

// In Startup
services.AddLogging(logging =>
{
    logging.AddApplicationInsights();
    logging.AddConsole();
    logging.SetMinimumLevel(LogLevel.Information);
});
```

### Diagnostic Logs
```bash
# Enable diagnostic logs for App Service
az webapp log config --name "myWebApp" --resource-group "RG-Name" \
  --application-logging filesystem \
  --detailed-error-messages \
  --failed-request-tracing

# Stream logs
az webapp log tail --name "myWebApp" --resource-group "RG-Name"
```

---

## 3. Performance Optimization

### Async/Await Patterns
```csharp
// Bad: Blocking
public ActionResult GetUsers()
{
    var users = _userService.GetUsersAsync().Result; // BLOCKS!
    return Ok(users);
}

// Good: Async all the way
public async Task<ActionResult> GetUsers()
{
    var users = await _userService.GetUsersAsync();
    return Ok(users);
}

// Parallel async operations
public async Task<(Users users, Orders orders)> GetUserDataAsync(int userId)
{
    var usersTask = _userService.GetUserAsync(userId);
    var ordersTask = _orderService.GetUserOrdersAsync(userId);
    
    await Task.WhenAll(usersTask, ordersTask);
    
    return (await usersTask, await ordersTask);
}
```

### Caching Strategy
```csharp
using Microsoft.Extensions.Caching.Distributed;

public class UserService
{
    private readonly IDistributedCache _cache;
    private readonly IUserRepository _repository;

    public async Task<User> GetUserAsync(int id)
    {
        var cacheKey = $"user:{id}";
        
        // Try get from cache
        var cachedUser = await _cache.GetAsync(cacheKey);
        if (cachedUser != null)
        {
            return JsonSerializer.Deserialize<User>(cachedUser);
        }

        // Get from database
        var user = await _repository.GetUserAsync(id);

        // Store in cache (5 minutes)
        var options = new DistributedCacheEntryOptions
            .SetAbsoluteExpiration(TimeSpan.FromMinutes(5));
        
        await _cache.SetAsync(cacheKey, JsonSerializer.SerializeToUtf8Bytes(user), options);

        return user;
    }
}

// In Startup
services.AddStackExchangeRedisCache(options =>
{
    options.Configuration = "localhost:6379";
});
```

### Connection Pooling
```csharp
// SQL Connection pooling (automatic, but configurable)
var connectionString = "Server=server.database.windows.net;Database=db;Max Pool Size=100;Min Pool Size=5;";

// HTTP Client reuse (NOT new HttpClient per request)
private static readonly HttpClient Client = new HttpClient();

// Avoid creating new DbContext per request (ASP.NET DI handles this)
services.AddScoped<UserContext>();
```

---

## 4. Azure Monitor Integration

### Metrics & Alerts
```csharp
using Azure.Monitor.Query;
using Azure.Monitor.Query.Models;

// Query metrics
var metricsQueryClient = new MetricsQueryClient(new DefaultAzureCredential());

var response = await metricsQueryClient.QueryResourceAsync(
    new ResourceIdentifier("/subscriptions/sub-id/resourceGroups/rg/providers/Microsoft.Compute/virtualMachines/vm"),
    new[] { "Percentage CPU" },
    new MetricsQueryOptions { Granularity = TimeSpan.FromMinutes(1) });

foreach (var metric in response.Metrics)
{
    foreach (var timeseries in metric.TimeSeries)
    {
        foreach (var data in timeseries.Data)
        {
            Console.WriteLine($"Time: {data.TimeStamp}, Value: {data.Total}");
        }
    }
}
```

### Log Analytics Queries
```kusto
// Query logs from Application Insights
AzureActivity
| where TimeGenerated > ago(24h)
| where OperationName == "Microsoft.Compute/virtualMachines/write"
| summarize Count = count() by Caller

// Performance counters
Perf
| where CounterName == "% Processor Time"
| where Computer == "myVM"
| summarize AvgCPU = avg(CounterValue) by bin(TimeGenerated, 5m)
```

---

## 5. Debugging & Troubleshooting

### Remote Debugging
```csharp
// Enable remote debugging in App Service
// Via Azure Portal: Configuration → General Settings → Remote Debugging

// Visual Studio Remote Debugger
// Cloud Explorer → App Service → Attach Debugger

// Breakpoints work across the network
```

### Exception Handling
```csharp
// Global exception handler
public class GlobalExceptionHandlerMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionHandlerMiddleware> _logger;

    public async Task InvokeAsync(HttpContext context, ILogger<GlobalExceptionHandlerMiddleware> logger)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Unhandled exception");
            context.Response.StatusCode = StatusCodes.Status500InternalServerError;
            await context.Response.WriteAsJsonAsync(new { error = ex.Message });
        }
    }
}

// Register middleware
app.UseMiddleware<GlobalExceptionHandlerMiddleware>();
```

---

## Key Exam Topics
✓ Application Insights integration
✓ Custom metrics and events
✓ KQL queries for analytics
✓ Application logging setup
✓ Diagnostic logs configuration
✓ Performance optimization techniques
✓ Async/await patterns
✓ Caching strategies
✓ Connection pooling
✓ Azure Monitor metric queries
✓ Log Analytics queries
✓ Remote debugging
✓ Exception handling and logging

