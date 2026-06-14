# Domain 5: Connect to and Consume Azure Services (15-20%)

## 1. Azure Service Bus

### Queue Operations
```csharp
using Azure.Messaging.ServiceBus;

// Create client
var client = new ServiceBusClient("connection-string");
var sender = client.CreateSender("queueName");
var receiver = client.CreateReceiver("queueName");

// Send message
var message = new ServiceBusMessage(BinaryData.FromString("Message content"));
await sender.SendMessageAsync(message);

// Send batch
using ServiceBusMessageBatch batch = await sender.CreateMessageBatchAsync();
for (int i = 0; i < 3; i++)
{
    var msg = new ServiceBusMessage($"Message {i}");
    if (!batch.TryAddMessage(msg))
    {
        break;
    }
}
await sender.SendMessagesAsync(batch);

// Receive messages
ServiceBusReceivedMessage receivedMessage = await receiver.ReceiveMessageAsync();
Console.WriteLine($"Received: {receivedMessage.Body}");
await receiver.CompleteMessageAsync(receivedMessage);

// Process with handler
var options = new ServiceBusProcessorOptions { MaxConcurrentCalls = 10 };
var processor = client.CreateProcessor("queueName", options);
processor.ProcessMessageAsync += Handler;
processor.ProcessErrorAsync += ErrorHandler;
await processor.StartProcessingAsync();

async Task Handler(ProcessMessageEventArgs args)
{
    Console.WriteLine($"Message: {args.Message.Body}");
    await args.CompleteMessageAsync(args.CancellationToken);
}

async Task ErrorHandler(ProcessErrorEventArgs args)
{
    Console.WriteLine($"Error: {args.Exception}");
}
```

### Topic & Subscription
```csharp
// Send to topic
var topicSender = client.CreateSender("topicName");
await topicSender.SendMessageAsync(new ServiceBusMessage("Data"));

// Receive from subscription
var subscriptionReceiver = client.CreateReceiver("topicName", "subscriptionName");
var message = await subscriptionReceiver.ReceiveMessageAsync();
await subscriptionReceiver.CompleteMessageAsync(message);

// Create subscription with filter
var subscriptionClient = new ServiceBusAdministrationClient("connection-string");
var ruleProperties = new CreateRuleOptions("rule1")
{
    Filter = new SqlRuleFilter("Priority = 'High'"),
    Action = new SqlRuleAction("SET OrderId = '5'")
};
await subscriptionClient.CreateRuleAsync("topicName", "subscriptionName", ruleProperties);
```

---

## 2. Azure Event Hubs

### Event Hub Producer & Consumer
```csharp
using Azure.Messaging.EventHubs;
using Azure.Messaging.EventHubs.Producer;
using Azure.Messaging.EventHubs.Consumer;

// Create producer
var producerClient = new EventHubProducerClient("connection-string", "eventHubName");

// Send events
var batch = await producerClient.CreateBatchAsync();
for (int i = 0; i < 100; i++)
{
    var eventData = new EventData(BinaryData.FromString($"Event {i}"));
    batch.TryAdd(eventData);
}
await producerClient.SendEventsAsync(batch);

// Create consumer
var consumerClient = new EventHubConsumerClient(
    EventHubConsumerClient.DefaultConsumerGroupName,
    "connection-string",
    "eventHubName");

// Read events
EventPosition startingPosition = EventPosition.Earliest;
await foreach (var partitionEvent in consumerClient.ReadEventsAsync(startingPosition))
{
    Console.WriteLine($"Event: {partitionEvent.Data.EventBody}");
}

// Checkpointing (for resuming)
using BlobContainerClient containerClient = new BlobContainerClient(
    new Uri("https://storage.blob.core.windows.net/container"),
    new DefaultAzureCredential());

var checkpointStore = new BlobCheckpointStore(containerClient);
var processor = new EventProcessorClient(checkpointStore, "group", "connection-string", "eventHubName");

processor.ProcessEventAsync += async (args) =>
{
    Console.WriteLine($"Received: {args.Data.EventBody}");
    await args.UpdateCheckpointAsync();
};

processor.ProcessErrorAsync += async (args) =>
{
    Console.WriteLine($"Error: {args.Exception}");
};

await processor.StartProcessingAsync();
```

---

## 3. Azure Event Grid

### Event Grid Publishing
```csharp
using Azure.Messaging.EventGrid;

var client = new EventGridPublisherClient(
    new Uri("https://topicname.region.eventgrid.azure.net/api/events"),
    new AzureKeyCredential("access-key"));

// Publish event
var evt = new EventGridEvent(
    subject: "orders/order1",
    eventType: "OrderCreated",
    dataVersion: "1.0",
    data: new { OrderId = 123, Amount = 99.99 });

await client.SendEventsAsync(new[] { evt });

// Publish multiple events
var events = new[]
{
    new EventGridEvent("orders/1", "OrderCreated", "1.0", new { Amount = 10 }),
    new EventGridEvent("orders/2", "OrderCreated", "1.0", new { Amount = 20 })
};
await client.SendEventsAsync(events);
```

### Event Grid Handler (Webhook)
```csharp
// Webhook handler in Azure Function
[Function("EventGridTrigger")]
public static async Task Run(
    [EventGridTrigger] EventGridEvent gridEvent,
    ILogger log)
{
    log.LogInformation($"Event received: {gridEvent.EventType}");
    
    switch (gridEvent.EventType)
    {
        case "OrderCreated":
            var orderData = JsonDocument.Parse(gridEvent.Data);
            log.LogInformation($"Order: {orderData.RootElement}");
            break;
    }
}
```

---

## 4. Azure Cognitive Services

### Text Analytics
```csharp
using Azure.AI.TextAnalytics;

var client = new TextAnalyticsClient(
    new Uri("https://region.api.cognitive.microsoft.com/"),
    new AzureKeyCredential("api-key"));

// Sentiment analysis
var sentiment = await client.AnalyzeSentimentAsync("I had a wonderful day!");
Console.WriteLine($"Sentiment: {sentiment.Value.Sentiment}");

// Extract entities
var entities = await client.RecognizeEntitiesAsync("John works at Microsoft in Seattle.");
foreach (var entity in entities.Value)
{
    Console.WriteLine($"Entity: {entity.Text}, Category: {entity.Category}");
}

// Key phrase extraction
var keyPhrases = await client.ExtractKeyPhrasesAsync("The quick brown fox jumps over the lazy dog");
foreach (var phrase in keyPhrases.Value)
{
    Console.WriteLine($"Key phrase: {phrase}");
}
```

### Computer Vision
```csharp
using Azure.AI.Vision.ImageAnalysis;

var client = new ImageAnalysisClient(
    new Uri("https://region.api.cognitive.microsoft.com/"),
    new AzureKeyCredential("api-key"));

// Analyze image
var imageUrl = "https://example.com/image.jpg";
var result = await client.AnalyzeAsync(
    new Uri(imageUrl),
    VisualFeatures.Caption | VisualFeatures.Objects | VisualFeatures.Tags);

Console.WriteLine($"Caption: {result.Value.Caption.Text}");
foreach (var obj in result.Value.Objects)
{
    Console.WriteLine($"Object: {obj.Tags[0].Name}");
}
```

---

## 5. REST APIs & SDKs

### HTTP Client with Retry Policy
```csharp
using Polly;
using Polly.Timeout;

// Retry policy
var retryPolicy = Policy
    .Handle<HttpRequestException>()
    .Or<TimeoutException>()
    .OrResult<HttpResponseMessage>(r => !r.IsSuccessStatusCode)
    .WaitAndRetryAsync(
        retryCount: 3,
        sleepDurationProvider: count => 
            TimeSpan.FromSeconds(Math.Pow(2, count)),
        onRetry: (outcome, timespan, count, context) =>
            Console.WriteLine($"Retry {count} after {timespan.TotalSeconds}s"));

var policy = Policy.WrapAsync(
    retryPolicy,
    Policy.TimeoutAsync<HttpResponseMessage>(TimeSpan.FromSeconds(10)));

using var client = new HttpClient();
var response = await policy.ExecuteAsync(async () => 
    await client.GetAsync("https://api.example.com/data"));

var content = await response.Content.ReadAsStringAsync();
```

### API Integration Pattern
```csharp
public interface IApiClient
{
    Task<T> GetAsync<T>(string endpoint);
    Task<T> PostAsync<T>(string endpoint, object body);
}

public class ApiClient : IApiClient
{
    private readonly HttpClient _httpClient;
    private readonly string _baseUrl;

    public ApiClient(HttpClient httpClient, string baseUrl)
    {
        _httpClient = httpClient;
        _baseUrl = baseUrl;
    }

    public async Task<T> GetAsync<T>(string endpoint)
    {
        var response = await _httpClient.GetAsync($"{_baseUrl}/{endpoint}");
        response.EnsureSuccessStatusCode();
        var json = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<T>(json);
    }

    public async Task<T> PostAsync<T>(string endpoint, object body)
    {
        var json = JsonSerializer.Serialize(body);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        var response = await _httpClient.PostAsync($"{_baseUrl}/{endpoint}", content);
        response.EnsureSuccessStatusCode();
        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<T>(responseJson);
    }
}

// Usage
services.AddScoped<IApiClient>(provider =>
{
    var httpClient = new HttpClient();
    return new ApiClient(httpClient, "https://api.example.com");
});
```

---

## Key Exam Topics
✓ Service Bus queues and topics
✓ Event Hub producer and consumer
✓ Event Grid publishing and handlers
✓ Cognitive Services integration
✓ Computer Vision and Language APIs
✓ REST API consumption
✓ SDK usage and patterns
✓ Error handling and retries
✓ Connection strings and authentication
✓ Message filtering and routing
✓ Webhooks and async processing

