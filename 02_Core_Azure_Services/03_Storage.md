# 💾 Storage Services

Azure offers highly scalable and secure storage solutions for various data types.

## 1. Azure Blob Storage
- **What is it?** Object storage solution for the cloud. Optimized for storing massive amounts of unstructured data (text or binary data).
- **Use Cases:**
  - Serving images or documents directly to a browser.
  - Storing files for distributed access.
  - Streaming video and audio.
  - Backups and archiving.
- **Access Tiers:**
  - **Hot:** Frequently accessed data. Highest storage cost, lowest access cost.
  - **Cool:** Infrequently accessed data (stored for at least 30 days). Lower storage cost, higher access cost.
  - **Archive:** Rarely accessed data (stored for at least 180 days). Lowest storage cost, highest access cost (takes hours to retrieve).

## 2. Azure Files
- **What is it?** Fully managed file shares in the cloud that are accessible via the industry standard Server Message Block (SMB) protocol or Network File System (NFS) protocol.
- **Use Cases:** Lift-and-shift on-premises file servers, replacing traditional NAS. Multiple VMs can mount the file share simultaneously.

## 3. Azure Queue Storage
- **What is it?** A service for storing large numbers of messages. You access messages from anywhere in the world via authenticated calls using HTTP or HTTPS.
- **Use Cases:** Creating a backlog of work to process asynchronously (decoupling components in a microservice architecture).

## 4. Azure Table Storage
- **What is it?** Stores large amounts of structured NoSQL data in the cloud (key/attribute store with a schemaless design). *Note: Azure Cosmos DB for Table is the premium offering now, but Table Storage still exists.*

## Redundancy Options (Data Protection)
- **LRS (Locally Redundant Storage):** 3 copies within a single datacenter in the primary region. Cheapest, protects against server rack failures.
- **ZRS (Zone-Redundant Storage):** 3 copies across 3 different Availability Zones in the primary region. Protects against datacenter failures.
- **GRS (Geo-Redundant Storage):** 3 copies in primary region (LRS) + asynchronous copy to a secondary paired region (LRS). Total 6 copies. Protects against regional outages.
- **GZRS (Geo-Zone-Redundant Storage):** 3 copies across AZs in primary (ZRS) + asynchronous copy to secondary region (LRS). Total 6 copies. Maximum protection.
