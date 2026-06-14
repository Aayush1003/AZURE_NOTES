# Domain 3: Implement Azure Security (20-25%)

## 1. Authentication & Authorization

### Azure AD Authentication
```csharp
using Azure.Identity;
using Microsoft.Identity.Client;

// Client Credentials Flow (App to Azure)
var credential = new ClientSecretCredential(
    tenantId: "tenant-id",
    clientId: "client-id",
    clientSecret: "client-secret");

// Authorization Code Flow (User login)
var publicClientApplication = PublicClientApplicationBuilder
    .Create("client-id")
    .WithAuthority("https://login.microsoftonline.com/tenant-id")
    .WithRedirectUri("http://localhost:3000")
    .Build();

var result = await publicClientApplication.AcquireTokenInteractive(
    new[] { "https://graph.microsoft.com/.default" })
    .ExecuteAsync();

Console.WriteLine($"Access Token: {result.AccessToken}");

// Refresh Token
var refreshResult = await publicClientApplication.AcquireTokenByRefreshToken(
    result.RefreshToken,
    new[] { "https://graph.microsoft.com/.default" })
    .ExecuteAsync();
```

### Managed Identity
```csharp
// System-Assigned Identity (automatic)
var credential = new DefaultAzureCredential();
var blobClient = new BlobClient(
    new Uri("https://storage.blob.core.windows.net/container/blob"),
    credential);

// User-Assigned Identity (explicit)
var credential = new DefaultAzureCredential(
    new DefaultAzureCredentialOptions
    {
        ManagedIdentityClientId = "user-assigned-identity-id"
    });
```

---

## 2. Azure Key Vault

### Key Vault Operations
```csharp
using Azure.Security.KeyVault.Secrets;
using Azure.Security.KeyVault.Keys;
using Azure.Security.KeyVault.Certificates;

// Create clients
var secretClient = new SecretClient(
    new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential());

var keyClient = new KeyClient(
    new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential());

var certificateClient = new CertificateClient(
    new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential());

// Set secret
KeyVaultSecret secret = await secretClient.SetSecretAsync("mySecret", "secretValue");

// Get secret
KeyVaultSecret retrieved = await secretClient.GetSecretAsync("mySecret");
Console.WriteLine($"Secret: {retrieved.Value}");

// Create key
KeyVaultKey key = await keyClient.CreateKeyAsync("myKey", KeyType.Rsa);

// Import certificate
var policy = new CertificatePolicy("Self", "CN=myapp.com");
CertificateOperation operation = await certificateClient.StartCreateCertificateAsync("myCert", policy);
```

---

## 3. RBAC for Applications

### Application-Level RBAC
```csharp
using Microsoft.Graph;
using Azure.Identity;

// Use Microsoft Graph to check roles
var graphClient = new GraphServiceClient(new DefaultAzureCredential());

// Get user's assigned roles
var directoryObjects = await graphClient.Me.MemberOf
    .GetAsync((requestConfiguration) =>
    {
        requestConfiguration.QueryParameters.Filter = "isOf('microsoft.graph.directoryRole')";
    });

foreach (var directoryRole in directoryObjects.Value)
{
    Console.WriteLine($"Role: {directoryRole.Id}");
}

// Custom app roles
var appRoles = new List<AppRole>
{
    new AppRole
    {
        AllowedMemberTypes = new List<string> { "User" },
        DisplayName = "Admin",
        Id = Guid.NewGuid().ToString(),
        IsEnabled = true,
        Value = "admin"
    }
};
```

---

## 4. API Security

### API Key Authentication
```csharp
// Add API key header in HTTP client
using HttpClient client = new HttpClient();
client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "api-key");
var response = await client.GetAsync("https://api.example.com/data");
```

### OAuth 2.0 / OpenID Connect
```csharp
// Protected API endpoint with JWT validation
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;

services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.Authority = "https://login.microsoftonline.com/tenant-id";
        options.Audience = "api-resource-id";
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = true
        };
    });

// In controller
[Authorize]
[HttpGet("api/protected")]
public IActionResult GetProtected()
{
    var claims = User.Claims;
    return Ok("Authorized!");
}
```

---

## 5. Data Encryption

### Encryption at Rest
```csharp
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;

// Azure handles encryption automatically with Microsoft-managed keys
var blobClient = new BlobClient(
    new Uri("https://storage.blob.core.windows.net/container/blob"),
    new DefaultAzureCredential());

// Data is encrypted at rest by default
await blobClient.UploadAsync(BinaryData.FromString("sensitive data"), overwrite: true);

// Use customer-managed keys via Key Vault
var encryptionScopeOptions = new BlobUploadOptions
{
    EncryptionScope = "scope-name"
};
await blobClient.UploadAsync(BinaryData.FromString("data"), options: encryptionScopeOptions);
```

### TLS/HTTPS Encryption
```csharp
// Force HTTPS in ASP.NET Core
public void Configure(IApplicationBuilder app)
{
    app.UseHsts(); // HTTP Strict Transport Security
    app.UseHttpsRedirection();
}

// .NET configuration
var httpHandler = new HttpClientHandler();
httpHandler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
{
    return errors == System.Net.Security.SslPolicyErrors.None;
};
var client = new HttpClient(httpHandler);
```

---

## 6. API Management (APIM)

### API Management Setup
```csharp
// Using Azure API Management SDK
using Azure.ResourceManager.ApiManagement;

var apiManagementClient = new ApiManagementClient(
    new Uri("https://myapim.management.azure-api.net"),
    new DefaultAzureCredential());

// Add API
var apiData = new ApiCreateOrUpdateProperties
{
    DisplayName = "My API",
    ServiceUrl = "https://myapi.example.com",
    Path = "v1"
};

// Add operation
var operationData = new OperationCreateOrUpdateProperties
{
    DisplayName = "Get Users",
    Method = "GET",
    UrlTemplate = "/users"
};
```

### API Policy
```xml
<!-- Rate limiting policy -->
<policies>
    <inbound>
        <rate-limit calls="100" renewal-period="60" />
        <base />
    </inbound>
</policies>

<!-- JWT validation policy -->
<policies>
    <inbound>
        <validate-jwt header-name="Authorization" failed-validation-httpcode="401">
            <openid-config url="https://login.microsoftonline.com/tenant-id/.well-known/openid-configuration" />
            <audiences>
                <audience>api-resource-id</audience>
            </audiences>
        </validate-jwt>
    </inbound>
</policies>
```

---

## Key Exam Topics
✓ Azure AD authentication flows (ROPC, client credentials, auth code)
✓ Managed Identity (system and user-assigned)
✓ Key Vault integration and secrets management
✓ RBAC and application roles
✓ API Key authentication
✓ OAuth 2.0 and OpenID Connect
✓ JWT token validation
✓ Data encryption (at rest and in transit)
✓ TLS/HTTPS configuration
✓ API Management policies and security

