# Domain 4: Networking & IoT

## 1. Virtual Networks (VNets)

### Overview
- Private network in Azure cloud
- Subnets for resource organization
- Security and connectivity

### Components
- **VNet**: Address space (CIDR blocks)
- **Subnet**: Subdivides VNet
- **Network Interface**: VM connection to subnet
- **Route Table**: Controls traffic routing

### VNet Peering
- Connect VNets directly
- No internet traversal
- Transitive peering: Not automatic

---

## 2. Load Balancer

### Azure Load Balancer
- Layer 4 (Transport)
- TCP/UDP traffic distribution
- High performance

### Application Gateway
- Layer 7 (Application)
- HTTP/HTTPS routing
- Path-based and host-based routing

### When to Use Each
```
Load Balancer:      Non-HTTP protocols, extreme throughput
App Gateway:        HTTP/HTTPS, path/host-based routing
Traffic Manager:    Geographic routing, multi-region
Front Door:         Global CDN + load balancing
```

---

## 3. VPN Gateway

### Site-to-Site VPN
- Connect on-premises to Azure
- IPSec encryption
- Secure tunnel

### Point-to-Site VPN
- Individual device to VNet
- Remote access
- OpenVPN, SSTP, IKEv2

### VNet-to-VNet
- Connect Azure VNets
- Encrypted tunnel

---

## 4. Express Route

### Overview
- Private, dedicated connection to Azure
- No internet traversal
- Consistent performance
- Higher cost than VPN

### Connectivity Models
- Cloud exchange co-location
- Point-to-point Ethernet
- Any-to-any (IPVPN)

### Benefits
✓ Reliable, low-latency
✓ Private, secure
✓ Consistent bandwidth
✓ No internet congestion

---

## 5. Azure DNS

### Public DNS Zones
- Host DNS records
- Domain name resolution
- Public internet

### Private DNS Zones
- Internal name resolution
- VNet isolation
- No internet exposure

---

## 6. Azure IoT Services

### IoT Hub
- Central communication hub
- Device management
- Secure communication
- Bidirectional messaging

### IoT Central
- SaaS solution
- Pre-built templates
- No infrastructure management
- Simpler setup

### IoT Edge
- Deploy processing to edge devices
- Local processing, reduced latency
- Works with IoT Hub

### Event Hubs
- High-scale event ingestion
- Real-time data processing
- Millions of events per second

---

## Networking Services Comparison

| Service | Layer | Use Case | Cost |
|---------|-------|----------|------|
| **Load Balancer** | 4 (Transport) | Non-HTTP, high performance | Low |
| **App Gateway** | 7 (Application) | HTTP/HTTPS, path routing | Medium |
| **Traffic Manager** | DNS | Geographic routing | Low |
| **Front Door** | Global | CDN + LB | High |
| **VPN Gateway** | 3 (Network) | Site-to-site, P2S | Medium |
| **Express Route** | 3 (Network) | Private connections | High |

---

## Key Exam Topics
✓ VNet creation and subnet design
✓ Network Security Groups and firewalls
✓ Load Balancer vs Application Gateway
✓ VPN Gateway types and configurations
✓ Express Route connectivity
✓ Azure DNS (public and private)
✓ Azure IoT services
✓ When to use each networking service
✓ Routing and traffic management
✓ Security in network architecture
✓ Hybrid connectivity options

