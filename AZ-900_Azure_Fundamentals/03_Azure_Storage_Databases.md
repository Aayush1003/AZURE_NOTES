# Domain 3: Azure Storage & Databases

## 1. Azure Storage Account

### Overview
- Massively scalable cloud storage
- Secure, durable, highly available

### Storage Services
| Service | Type | Durability | Use Case |
|---------|------|-----------|----------|
| **Blob Storage** | Object storage | 11 nines (LRS) | Files, media, backups |
| **File Share** | SMB/NFS | 11 nines | File sharing, mount points |
| **Queue Storage** | Messaging | 11 nines | Async messaging |
| **Table Storage** | NoSQL | 11 nines | Unstructured data |

### Replication Options
| Option | Durability | Cost | Geography |
|--------|-----------|------|-----------|
| **LRS** | 11 nines | Lowest | Single region |
| **ZRS** | 12 nines | Medium | Single region, 3 AZs |
| **GRS** | 16 nines | Medium | 2 regions |
| **GZRS** | 16 nines | Highest | 2 regions, ZRS |

### Blob Access Tiers
- **Hot**: Frequent access, highest cost access
- **Cool**: Infrequent access (30+ days), lower storage cost
- **Archive**: Rarely accessed (90+ days), lowest storage cost

---

## 2. Azure SQL Database

### Overview
- Relational database service
- Managed SQL Server
- Automatic backups and patching

### Service Tiers
- **Basic**: Development/testing
- **Standard**: General production workloads
- **Premium**: Advanced performance, PITR

### Benefits
✓ Automatic backups (35 days)
✓ Geo-replication for DR
✓ Automatic patching
✓ Advanced security
✓ Elastic pools for cost savings

---

## 3. Azure Cosmos DB

### Overview
- Globally distributed, multi-model database
- Any scale
- Millisecond latencies

### Consistency Levels
- **Strong**: Linearizable
- **Bounded staleness**: Limited lag
- **Session**: Per-session consistency
- **Consistent prefix**: Causal consistency
- **Eventual**: Highest performance

### When to Use
✓ Global scale requirements
✓ Always-on requirements
✓ Massive throughput
✓ Variable schema (flexible)
✓ Real-time analytics

---

## 4. Data Analytics Services

### Azure Synapse Analytics
- Big data and data warehouse
- SQL pools and Spark pools
- Integrated analytics

### Azure Data Lake
- Massive-scale data storage
- Structured and unstructured
- For big data analytics

### Azure HDInsight
- Managed Hadoop/Spark clusters
- Open-source analytics
- For complex processing

### Azure Databricks
- Apache Spark-based analytics
- Data engineering and ML
- Collaborative notebooks

---

## 5. Azure Database for MySQL/PostgreSQL
- Managed open-source databases
- Fully managed (backups, patching)
- High availability options

---

## Storage & Database Comparison

| Service | Type | Scale | Cost | Best For |
|---------|------|-------|------|----------|
| **Azure Storage** | Object | Massive | Low | Files, backups, unstructured |
| **Azure SQL DB** | Relational | High | Medium | Traditional applications, transactions |
| **Cosmos DB** | Multi-model | Massive | High | Global scale, real-time |
| **Data Lake** | Big Data | Massive | Low | Analytics, data processing |
| **Synapse** | Warehouse | Massive | Variable | Business intelligence, analytics |

---

## Key Exam Topics
✓ Storage account types and replication
✓ Blob tiers and lifecycle
✓ SQL Database features and service tiers
✓ Cosmos DB consistency and distribution
✓ When to use each database service
✓ Data redundancy and durability
✓ Security and encryption options
✓ Backup and recovery features
✓ Scalability characteristics
✓ Cost considerations

