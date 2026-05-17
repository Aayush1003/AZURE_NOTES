# 🌐 Networking Services

Networking connects your resources together and to the outside world.

## 1. Azure Virtual Network (VNet)
- **What is it?** The fundamental building block for your private network in Azure. Enables resources (like VMs) to securely communicate with each other, the internet, and on-premises networks.
- **Subnets:** You can segment your VNet into one or more subnets and allocate a portion of the VNet's address space to each subnet.
- **VNet Peering:** Connects two VNets seamlessly. Traffic between peered VNets stays on the Microsoft backbone network (doesn't go over the public internet).

## 2. Load Balancing Options
- **Azure Load Balancer:** Operates at Layer 4 (TCP/UDP). Distributes incoming network traffic across a group of backend resources (e.g., VMs).
- **Azure Application Gateway:** Operates at Layer 7 (HTTP/HTTPS). Provides web traffic load balancing, URL path-based routing, and Web Application Firewall (WAF).
- **Azure Front Door:** A global Layer 7 load balancer and Content Delivery Network (CDN) service. Excellent for highly scalable, global web applications.
- **Azure Traffic Manager:** A DNS-based traffic load balancer. Routes traffic to different regions based on rules (e.g., route to the closest geographic region).

## 3. Azure VPN Gateway
- **What is it?** Sends encrypted traffic between an Azure VNet and an on-premises location over the public internet.
- **Types:**
  - **Site-to-Site (S2S):** Connects your on-prem network to Azure.
  - **Point-to-Site (P2S):** Connects individual client computers to your Azure VNet.

## 4. Azure ExpressRoute
- **What is it?** A private connection to Azure from your on-premises infrastructure via a connectivity provider. Does NOT go over the public internet.
- **Benefits:** Higher security, faster speeds, lower latency, and more reliability compared to VPN Gateway.

## 5. Azure DNS
- **What is it?** A hosting service for DNS domains that provides name resolution by using Microsoft Azure infrastructure.
