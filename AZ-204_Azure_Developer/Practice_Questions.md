# AZ-204 Practice Questions & Sample Answers

## Domain 1: Compute Solutions (6 Questions)

### Question 1
**Scenario**: You're building a REST API that receives requests from mobile apps. Traffic is unpredictable and spikes occur during peak hours. You need automatic scaling and minimal management overhead.

**Which Azure service should you use?**
A) Azure Virtual Machines
B) Azure App Service with Standard Plan
C) Azure Functions
D) Azure Container Instances

**Answer**: B) Azure App Service with Standard Plan

**Explanation**: App Service Standard tier provides auto-scaling for web APIs. While Functions could work, App Service is better for continuous APIs. VMs require manual management. ACI doesn't auto-scale.

---

### Question 2
**Scenario**: You need to process files uploaded to Azure Blob Storage. Processing is triggered by file upload and may take 5-10 minutes per file.

**What's the best solution?**
A) Azure Functions with Blob trigger (5-min timeout)
B) Azure Functions with 60-minute timeout
C) Durable Functions
D) App Service background job

**Answer**: C) Durable Functions

**Explanation**: Durable Functions handle long-running operations. Regular Functions have 5-min timeout (not enough). App Service works but Durable Functions are more scalable and serverless.

---

### Question 3
**Scenario**: Your company runs containerized microservices that need automatic scaling, self-healing, and load balancing.

**Which service is best?**
A) Azure Container Instances
B) Azure Container Registry
C) Azure Kubernetes Service (AKS)
D) App Service Containers

**Answer**: C) Azure Kubernetes Service (AKS)

**Explanation**: AKS provides orchestration, auto-scaling, and self-healing. ACI lacks orchestration. ACR is just a registry. App Service containers don't have k8s features.

---

### Question 4
**Scenario**: Your App Service app needs to authenticate users with Azure Entra ID. You want to avoid storing user credentials.

**What should you implement?**
A) OAuth 2.0 Authorization Code flow
B) Resource Owner Password Credentials (ROPC) flow
C) Client Credentials flow
D) Device code flow

**Answer**: A) OAuth 2.0 Authorization Code flow

**Explanation**: Auth Code flow is for user login without storing passwords. ROPC exposes passwords. Client Credentials is for service-to-service. Device code is for CLI/IoT devices.

---

### Question 5
**Scenario**: You have an Azure Function triggered by Service Bus messages. Sometimes the function fails and you want automatic retries with exponential backoff.

**How should you implement this?**
A) Handle retries inside the function code
B) Configure Service Bus dead letter queue
C) Enable function retry policy in host.json
D) Use Durable Functions with retry logic

**Answer**: D) Use Durable Functions with retry logic

**Explanation**: Durable Functions have built-in retry and exponential backoff. Regular functions don't have built-in retry (you must code it). DLQ is for failed messages, not retries.

---

### Question 6
**Scenario**: Your App Service needs to run background jobs on a schedule (every 6 hours). What's the best solution?

A) Azure Functions with Timer trigger
B) Azure Scheduler
C) App Service WebJob (scheduled)
D) System.Timers.Timer in application code

**Answer**: A) Azure Functions with Timer trigger

**Explanation**: Timer-triggered Functions are serverless and scalable. Scheduler is deprecated. WebJobs require App Service running. In-app timers aren't reliable/scalable.

---

## Domain 2: Storage Solutions (5 Questions)

### Question 7
**Scenario**: You need to store 100 TB of JSON documents with flexible schema. Queries must execute globally with <10ms latency. Budget is significant.

**Which database is best?**
A) Azure SQL Database
B) Azure Cosmos DB
C) Azure Blob Storage
D) Azure Table Storage

**Answer**: B) Azure Cosmos DB

**Explanation**: Cosmos DB provides global distribution with low latency. SQL DB is regional. Blob/Table Storage aren't databases. Cosmos DB is expensive but meets requirements.

---

### Question 8
**Scenario**: You're uploading a 1 GB file to Azure Blob Storage. The operation might fail. What approach should you use?

A) Single PUT operation
B) Multi-part upload with SDK
C) Azure Data Box for large files
D) AzCopy command-line tool

**Answer**: B) Multi-part upload with SDK

**Explanation**: SDK handles chunking and retries. Single PUT fails for large files. Data Box is for offline transfers. AzCopy works but SDK is more programmatic.

---

### Question 9
**Scenario**: You need to authenticate blob access without sharing storage account keys. The access should expire after 1 hour.

**What should you use?**
A) Storage account key
B) Connection string with shared key
C) Shared Access Signature (SAS) token
D) Azure Entra ID with role assignment

**Answer**: C) Shared Access Signature (SAS) token

**Explanation**: SAS tokens provide temporary, scoped access with expiration. Storage keys don't expire. Connection strings expose keys. Entra ID doesn't have expiration for roles.

---

### Question 10
**Scenario**: You're querying a Cosmos DB container with 1 million items. The query will scan all items. How do you optimize?

A) Increase RU (Request Units)
B) Add index on queried fields
C) Use stored procedures
D) Partition the container

**Answer**: B) Add index on queried fields

**Explanation**: Indexes reduce RUs needed. Increasing RUs costs more but doesn't improve efficiency. Stored procedures don't help full scans. Container partition is already done.

---

### Question 11
**Scenario**: Your application caches frequently-accessed data in Redis. Cache sometimes expires or is flushed. You need fallback data access.

**What's the pattern?**
A) Always rely on cache; if miss, get from source
B) Cache-aside: check cache, miss → get source and cache
C) Write-through: write to both cache and source simultaneously
D) Cache is optional, only use sometimes

**Answer**: B) Cache-aside: check cache, miss → get source and cache

**Explanation**: Cache-aside (lazy loading) handles cache misses by fetching from source. Write-through writes to both. Option A doesn't handle cache misses. Option D isn't a pattern.

---

## Domain 3: Security (4 Questions)

### Question 12
**Scenario**: Your function app needs to access multiple Azure resources (Storage, Key Vault, Cosmos DB) securely without hardcoding credentials.

**What's the best approach?**
A) Store connection strings in application settings
B) Use Managed Identity with RBAC
C) Use service principal with app registration
D) Embed credentials in code

**Answer**: B) Use Managed Identity with RBAC

**Explanation**: Managed Identity is simplest (no secrets to manage). Service Principal requires managing credentials. App settings aren't secure. Code embedding is dangerous.

---

### Question 13
**Scenario**: You need to store API keys, database passwords, and certificates securely. These values change frequently and need versioning.

**Where should you store them?**
A) Application configuration files
B) Environment variables
C) Azure Key Vault
D) Encrypted database table

**Answer**: C) Azure Key Vault

**Explanation**: Key Vault provides secure storage, versioning, and rotation. Config files aren't secure. Env vars can be exposed. Database isn't designed for secret management.

---

### Question 14
**Scenario**: Your REST API needs to validate JWT tokens from Azure Entra ID. Tokens should be verified to prevent unauthorized access.

**How should you implement this?**
A) Skip validation; tokens are from trusted source
B) Validate JWT signature using Entra public keys
C) Store token in database and check on each request
D) Trust claims without signature validation

**Answer**: B) Validate JWT signature using Entra public keys

**Explanation**: Always validate signatures using public keys from provider. Never skip validation. Database check is slower. Claims without signatures are insecure.

---

### Question 15
**Scenario**: Your application uploads files to Blob Storage. You want to ensure data is encrypted at rest and in transit.

**Which is correct?**
A) Only encryption in transit is possible
B) Only encryption at rest is possible
C) Both are possible; encryption at rest is automatic
D) Encryption must be manually configured for both

**Answer**: C) Both are possible; encryption at rest is automatic

**Explanation**: Azure Storage encrypts at rest by default (AES-256). HTTPS provides in-transit encryption. Both happen automatically with zero config.

---

## Answer Key Summary

| Q# | Answer | Concept |
|----|--------|---------|
| 1 | B | App Service auto-scaling |
| 2 | C | Durable Functions for long-running ops |
| 3 | C | AKS for orchestration |
| 4 | A | OAuth 2.0 for user auth |
| 5 | D | Durable Functions for retry logic |
| 6 | A | Timer-triggered Functions |
| 7 | B | Cosmos DB for global scale |
| 8 | B | Multi-part upload for large files |
| 9 | C | SAS tokens for temporary access |
| 10 | B | Indexing for query optimization |
| 11 | B | Cache-aside pattern |
| 12 | B | Managed Identity for secure access |
| 13 | C | Key Vault for secrets |
| 14 | B | JWT signature validation |
| 15 | C | Automatic encryption at rest |

---

## Scoring Guide

**Score 60-70%**: Review weak domains thoroughly
**Score 71-85%**: Practice more scenarios; focus on edge cases
**Score 86-95%**: Ready for exam; final review recommended
**Score 95%+**: Take full practice exam to confirm readiness

