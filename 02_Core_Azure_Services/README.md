# 🏗️ Core Azure Services

This section details the primary services offered by Azure, categorised into Compute, Networking, Storage, and Databases.

## Sub-Modules
1. [Compute](./01_Compute.md) - VMs, App Services, AKS, Functions.
2. [Networking](./02_Networking.md) - VNets, Load Balancers, VPN Gateway, Application Gateway.
3. [Storage](./03_Storage.md) - Blob, File, Queue, Table storage.
4. [Databases](./04_Databases.md) - Cosmos DB, Azure SQL, PostgreSQL.

## Azure Global Infrastructure
- **Regions:** A set of datacenters deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network.
- **Availability Zones (AZs):** Physically separate datacenters within an Azure region. Each zone consists of one or more datacenters equipped with independent power, cooling, and networking. Protects against datacenter failures.
- **Region Pairs:** Each Azure region is paired with another region within the same geography (at least 300 miles away) for disaster recovery. If a region fails, services automatically failover to the paired region.
