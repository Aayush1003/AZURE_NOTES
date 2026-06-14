# Domain 2: Develop for Azure Storage (15-20%)

## 1. Azure Blob Storage SDK

### Blob Client Operations
```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;

// Create blob client
var blobClient = new BlobClient(
    new Uri("https://storage.blob.core.windows.net/container/blob"),
    new DefaultAzureCredential());

// Upload blob
BlobUploadOptions uploadOptions = new BlobUploadOptions
{
    Metadata = new Dictionary<string, string> { { "key", "value" } }
};
await blobClient.UploadAsync(BinaryData.FromString("content"), overwrite: true, options: uploadOptions);

// Download blob
BlobDownloadInfo download = await blobClient.DownloadAsync();
using (StreamReader reader = new StreamReader(download.Content))
{
    Console.WriteLine(reader.ReadToEnd());
}

// List blobs
var containerClient = new BlobContainerClient(
    new Uri("https://storage.blob.core.windows.net/container"),
    new DefaultAzureCredential());

await foreach (BlobItem blob in containerClient.GetBlobsAsync())
{
    Console.WriteLine($"Blob: {blob.Name}");
}
```

### Blob Properties & Metadata
```csharp
// Get blob properties
BlobProperties properties = await blobClient.GetPropertiesAsync();
Console.WriteLine($"Size: {properties.ContentLength}");
Console.WriteLine($"Last Modified: {properties.LastModified}");

// Set blob metadata
Dictionary<string, string> metadata = new() { { "tag", "important" } };
await blobClient.SetMetadataAsync(metadata);

// Get metadata
var metadataResponse = await blobClient.GetPropertiesAsync();
foreach (var item in metadataResponse.Value.Metadata)
{
    Console.WriteLine($"{item.Key}: {item.Value}");
}
```

### Blob Tiers & Lifecycle
```csharp
// Set access tier
await blobClient.SetAccessTierAsync(AccessTier.Cool);

// Lifecycle management via portal/CLI
// Move to Cool after 30 days
// Move to Archive after 90 days
// Delete after 365 days
```

---

## 2. Azure Files (SMB/NFS)

### File Share Operations
```csharp
using Azure.Storage.Files.Shares;
using Azure.Storage.Files.Shares.Models;

// Create share client
var shareClient = new ShareClient(
    new Uri("https://storage.file.core.windows.net/share"),
    new DefaultAzureCredential());

// Upload file
using (FileStream fileStream = File.OpenRead("localfile.txt"))
{
    await shareClient.GetRootDirectoryClient()
        .GetFileClient("remotefile.txt")
        .UploadAsync(fileStream, overwrite: true);
}

// Download file
ShareFileClient fileClient = shareClient.GetRootDirectoryClient().GetFileClient("remotefile.txt");
ShareFileDownloadInfo download = await fileClient.DownloadAsync();
using (FileStream fileStream = File.Create("localfile.txt"))
{
    await download.Content.CopyToAsync(fileStream);
}

// List files
await foreach (ShareFileItem item in shareClient.GetRootDirectoryClient().GetFilesAndDirectoriesAsync())
{
    Console.WriteLine($"Name: {item.Name}");
}
```

---

## 3. Azure Table Storage

### Table Entity Operations
```csharp
using Azure.Data.Tables;

// Create table client
var tableClient = new TableClient(
    new Uri("https://storage.table.core.windows.net"),
    "TableName",
    new DefaultAzureCredential());

// Insert entity
var entity = new TableEntity("PartitionKey", "RowKey")
{
    { "Name", "John" },
    { "Age", 30 }
};
await tableClient.AddEntityAsync(entity);

// Query entities
await foreach (var entity in tableClient.QueryAsync<TableEntity>())
{
    Console.WriteLine($"{entity.RowKey}: {entity["Name"]}");
}

// Update entity
var updateEntity = new TableEntity("PartitionKey", "RowKey")
{
    { "Name", "Jane" }
};
await tableClient.UpdateEntityAsync(updateEntity, ETag.All);

// Delete entity
await tableClient.DeleteEntityAsync("PartitionKey", "RowKey");
```

---

## 4. Azure Cosmos DB

### Cosmos DB Client
```csharp
using Microsoft.Azure.Cosmos;

// Create Cosmos client
CosmosClient client = new CosmosClient(
    endpoint: "https://cosmosaccount.documents.azure.com:443/",
    authKeyOrResourceToken: "accountKey");

// Get database and container
Database database = await client.CreateDatabaseIfNotExistsAsync("myDatabase");
Container container = await database.CreateContainerIfNotExistsAsync("myContainer", "/id");

// Insert item
dynamic item = new { id = Guid.NewGuid().ToString(), name = "John", age = 30 };
ItemResponse<dynamic> response = await container.CreateItemAsync(item);

// Query items
string query = "SELECT * FROM c WHERE c.age > 25";
FeedIterator<dynamic> iterator = container.GetItemQueryIterator<dynamic>(query);
while (iterator.HasMoreResults)
{
    FeedResponse<dynamic> response = await iterator.ReadNextAsync();
    foreach (var item in response)
    {
        Console.WriteLine($"ID: {item.id}");
    }
}

// Update item
item.name = "Jane";
await container.UpsertItemAsync(item);

// Delete item
await container.DeleteItemAsync<dynamic>(item.id.ToString(), new PartitionKey(item.id.ToString()));
```

### Cosmos DB SQL Query
```sql
-- Query with filter
SELECT c.id, c.name, c.age 
FROM c 
WHERE c.age > 25 
ORDER BY c.age

-- Query with joins
SELECT c.id, c.name, a.city
FROM customers c
JOIN addresses a ON c.id = a.customerId

-- Aggregate functions
SELECT COUNT(*), AVG(c.age), MAX(c.age)
FROM c

-- Pagination
SELECT * FROM c OFFSET 10 LIMIT 10
```

---

## 5. Azure SQL Database

### SQL Database Connection
```csharp
using Microsoft.Data.SqlClient;

// Connection string (managed identity)
string connectionString = "Server=tcp:myserver.database.windows.net,1433;Initial Catalog=mydb;";

using (SqlConnection connection = new SqlConnection(connectionString))
{
    // Use DefaultAzureCredential with SQL
    var token = new DefaultAzureCredential().GetTokenAsync(
        new Azure.Core.TokenRequestContext(new[] { "https://database.windows.net/.default" })).Result;
    
    connection.AccessToken = token.Token;
    await connection.OpenAsync();

    using (SqlCommand command = new SqlCommand("SELECT * FROM Users WHERE Age > @age", connection))
    {
        command.Parameters.AddWithValue("@age", 25);
        using (SqlDataReader reader = await command.ExecuteReaderAsync())
        {
            while (await reader.ReadAsync())
            {
                Console.WriteLine($"ID: {reader["Id"]}, Name: {reader["Name"]}");
            }
        }
    }
}
```

### Entity Framework Core
```csharp
using Microsoft.EntityFrameworkCore;

public class UserContext : DbContext
{
    public DbSet<User> Users { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
        var credential = new DefaultAzureCredential();
        var token = credential.GetTokenAsync(
            new TokenRequestContext(new[] { "https://database.windows.net/.default" })).Result;
        
        options.UseSqlServer(
            "Server=tcp:myserver.database.windows.net,1433;Initial Catalog=mydb;",
            sqlOptions => sqlOptions.AccessToken = token.Token);
    }
}

// Usage
using (var context = new UserContext())
{
    var users = await context.Users.Where(u => u.Age > 25).ToListAsync();
}
```

---

## 6. Azure Cache for Redis

### Redis Client
```csharp
using StackExchange.Redis;

// Connect to Redis
ConnectionMultiplexer redis = await ConnectionMultiplexer.ConnectAsync(
    "myredis.redis.cache.windows.net:6380?ssl=true",
    options => options.Password = "accessKey");

IDatabase db = redis.GetDatabase();

// Set and get
await db.StringSetAsync("key", "value");
var value = await db.StringGetAsync("key");

// Expiration
await db.StringSetAsync("sessionId", "sessionData", TimeSpan.FromHours(1));

// Lists
await db.ListPushAsync("queue", "item1");
await db.ListPushAsync("queue", "item2");
var item = await db.ListPopAsync("queue");

// Hashes (for objects)
await db.HashSetAsync("user:1", new HashEntry[] {
    new("name", "John"),
    new("email", "john@example.com"),
    new("age", "30")
});

var hashEntries = await db.HashGetAllAsync("user:1");
foreach (var entry in hashEntries)
{
    Console.WriteLine($"{entry.Name}: {entry.Value}");
}
```

---

## Key Exam Topics
✓ Azure Storage SDK operations
✓ Blob storage uploads, downloads, and metadata
✓ Azure Files SMB/NFS protocols
✓ Table Storage entities and queries
✓ Cosmos DB SQL queries and CRUD operations
✓ SQL Database connections and EF Core
✓ Managed identity for authentication
✓ Caching strategies with Redis
✓ Data encryption and security
✓ Data migration and backup

