# Domain 4: Configure and Manage Virtual Networking (20-25%)

## 1. Virtual Networks (VNet)

### VNet Basics
- Private network in Azure cloud
- IPv4 (and IPv6) address space
- Subnets for segmentation
- Network traffic control

### VNet Creation
```powershell
# Create VNet with subnets
$subnet1 = New-AzVirtualNetworkSubnetConfig -Name "subnet1" -AddressPrefix "10.0.1.0/24"
$subnet2 = New-AzVirtualNetworkSubnetConfig -Name "subnet2" -AddressPrefix "10.0.2.0/24"
New-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "RG-Name" -Location "East US" -AddressPrefix "10.0.0.0/16" -Subnet $subnet1, $subnet2

# Add subnet to existing VNet
$vnet = Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "RG-Name"
Add-AzVirtualNetworkSubnetConfig -Name "subnet3" -VirtualNetwork $vnet -AddressPrefix "10.0.3.0/24"
Set-AzVirtualNetwork -VirtualNetwork $vnet
```

### Peering
- Connect VNets directly
- Same region (VNet Peering) or different regions (Global VNet Peering)
- Transitive peering: NO - requires gateway

```powershell
# Create peering
Add-AzVirtualNetworkPeering -Name "peering1" -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id
```

### Peering Types
| Type | Same Region | Cross-Region | Cost |
|------|-------------|-------------|------|
| **VNet Peering** | Yes | No | Free |
| **Global VNet Peering** | No | Yes | Charges apply |
| **Gateway Transit** | Yes | Yes | Charges apply |

---

## 2. Network Security Groups (NSG)

### NSG Overview
- Firewall at network level
- Inbound/Outbound rules
- Applied to subnets or NICs
- Stateful filtering

### NSG Rules
```powershell
# Create NSG
$nsg = New-AzNetworkSecurityGroup -Name "myNSG" -ResourceGroupName "RG-Name" -Location "East US"

# Add inbound rule
$rule = New-AzNetworkSecurityRuleConfig -Name "AllowHTTP" -Direction Inbound -Priority 100 -SourceAddressPrefix "Internet" -SourcePortRange "*" -DestinationAddressPrefix "*" -DestinationPortRange "80" -Access Allow -Protocol "Tcp"
$nsg | Add-AzNetworkSecurityRuleConfig @rule
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg

# Associate NSG with subnet
$subnet = Get-AzVirtualNetworkSubnetConfig -Name "subnet1" -VirtualNetwork $vnet
Set-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "subnet1" -AddressPrefix "10.0.1.0/24" -NetworkSecurityGroup $nsg
Set-AzVirtualNetwork -VirtualNetwork $vnet
```

### Rule Priorities
- 100 = Highest priority (evaluated first)
- 4096 = Lowest priority
- 65500 = Default allow all (system rule)

### Common Rules
```
Inbound:
- Allow HTTP (80, TCP, Internet)
- Allow HTTPS (443, TCP, Internet)
- Allow RDP (3389, TCP, Admin IPs)
- Allow SSH (22, TCP, Admin IPs)

Outbound:
- Default: Allow all outbound traffic
```

---

## 3. Network Interface Cards (NIC)

### NIC Configuration
```powershell
# Create NIC
$nic = New-AzNetworkInterface -Name "myNIC" -ResourceGroupName "RG-Name" -Location "East US" -SubnetId $subnet.Id -NetworkSecurityGroupId $nsg.Id

# Add IP configuration
$ipConfig = New-AzNetworkInterfaceIpConfig -Name "ipconfig2" -Subnet $subnet -PrivateIPAddress "10.0.1.10"
$nic | Add-AzNetworkInterfaceIpConfig -IpConfig $ipConfig
Set-AzNetworkInterface -NetworkInterface $nic

# Assign public IP
$pip = New-AzPublicIpAddress -Name "myPIP" -ResourceGroupName "RG-Name" -Location "East US" -AllocationMethod Static
$nic | Set-AzNetworkInterfaceIpConfig -Name "ipconfig1" -PublicIpAddress $pip
Set-AzNetworkInterface -NetworkInterface $nic
```

### IP Configurations
- Multiple IPs per NIC
- Primary and secondary
- Support for failover scenarios

---

## 4. Public IP Addresses

### IP Types
| Type | SKU | Use Case | Allocation | Status |
|------|-----|----------|-----------|--------|
| **Standard** | Standard | Production | Static/Dynamic | Current, Recommended |
| **Basic** | Basic | Legacy/Testing | Static/Dynamic | Deprecated - Migrate to Standard |

### Public IP Management
```powershell
# Create public IP
$pip = New-AzPublicIpAddress -Name "myPIP" -ResourceGroupName "RG-Name" -Location "East US" -AllocationMethod "Static" -Sku "Standard"

# Associate with NIC
$nic = Get-AzNetworkInterface -Name "myNIC" -ResourceGroupName "RG-Name"
$nic.IpConfigurations[0].PublicIpAddress = $pip
Set-AzNetworkInterface -NetworkInterface $nic

# Add DNS label
$pip.DnsSettings.DomainNameLabel = "myapp"
Set-AzPublicIpAddress -PublicIpAddress $pip
```

---

## 5. Load Balancer

### Load Balancer Types
| Type | Layer | Routing | Use Case |
|------|-------|---------|----------|
| **Application Gateway** | L7 (App) | Path-based, host-based | Web apps, APIs |
| **Load Balancer** | L4 (Transport) | Port-based | VMs, Non-HTTP |
| **Traffic Manager** | Global | Geographic, latency | Geo-distribution |
| **Front Door** | Global | Path, host, geo | Global CDN+LB |

### Azure Load Balancer
```powershell
# Create load balancer
$lb = New-AzLoadBalancer -Name "myLB" -ResourceGroupName "RG-Name" -Location "East US" -Sku "Standard"

# Add frontend IP
$feIP = New-AzLoadBalancerFrontendIpConfig -Name "feIP" -PublicIpAddress $pip
$lb | Add-AzLoadBalancerFrontendIpConfig -FrontendIpConfig $feIP

# Add backend pool
$bePool = New-AzLoadBalancerBackendAddressPoolConfig -Name "bePool"
$lb | Add-AzLoadBalancerBackendAddressPoolConfig -BackendAddressPoolConfig $bePool

# Add health probe
$probe = New-AzLoadBalancerProbeConfig -Name "healthProbe" -Protocol "Tcp" -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$lb | Add-AzLoadBalancerProbeConfig -ProbeConfig $probe

# Add load balancing rule
$rule = New-AzLoadBalancerRuleConfig -Name "lbRule" -FrontendIpConfiguration $feIP -BackendAddressPool $bePool -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80
$lb | Add-AzLoadBalancerRuleConfig -RuleConfig $rule

Set-AzLoadBalancer -LoadBalancer $lb
```

### Application Gateway
```powershell
# Create Application Gateway
$gw = New-AzApplicationGateway -Name "myAppGateway" -ResourceGroupName "RG-Name" -Location "East US" -Sku "Standard_v2" -Tier "Standard_v2" -GatewayIpConfigurationName "gwConfig" -FrontendIpConfigurationName "feIP" -FrontendPortConfigurationName "fePort" -BackendAddressPoolName "bePool" -BackendHttpSettingsCollectionName "backendSettings" -HttpListenerName "httpListener" -RequestRoutingRuleName "routingRule"
```

---

## 6. VPN Gateway

### VPN Connection Types
| Type | Connectivity | Encryption | Use Case |
|------|------------|-----------|----------|
| **Site-to-Site** | On-premises to Azure VNet | IPSec | Hybrid network |
| **Point-to-Site** | Client to Azure VNet | OpenVPN/SSTP/IKEv2 | Remote access |
| **VNet-to-VNet** | VNet to VNet | IPSec | Multi-region apps |

### VPN Gateway Creation
```powershell
# Create gateway subnet
$gwsubnet = Add-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet -AddressPrefix "10.0.255.0/27"
Set-AzVirtualNetwork -VirtualNetwork $vnet

# Create VPN gateway
$gw = New-AzVirtualNetworkGateway -Name "myVPNGateway" -ResourceGroupName "RG-Name" -Location "East US" -IpConfigurations $ipconfig -GatewayType "Vpn" -VpnType "RouteBased" -GatewaySku "VpnGw1"

# Create site-to-site connection
New-AzVirtualNetworkGatewayConnection -Name "s2sConnection" -ResourceGroupName "RG-Name" -VirtualNetworkGateway1 $gw -LocalNetworkGateway2 $localGw -ConnectionType "IPsec" -SharedKey "shared-secret"
```

---

## 7. Express Route

### Express Route Overview
- Dedicated, private connection to Azure
- No internet traversal
- Higher bandwidth and lower latency
- 3 connectivity models:
  - Cloud exchange co-location
  - Point-to-point Ethernet
  - Any-to-any (IPVPN)

### Circuit Creation
```powershell
# Create Express Route circuit
New-AzExpressRouteCircuit -Name "myCircuit" -ResourceGroupName "RG-Name" -Location "West US" -SkuTier "Premium" -SkuFamily "MeteredData" -ServiceProviderName "Equinix" -PeeringLocation "Seattle" -BandwidthInGbps 10
```

---

## 8. DNS

### Azure DNS
- Host DNS zones
- Name resolution
- Private and public zones

```powershell
# Create DNS zone
New-AzDnsZone -Name "example.com" -ResourceGroupName "RG-Name"

# Add DNS record
New-AzDnsRecordSet -Name "www" -RecordType "A" -ZoneName "example.com" -ResourceGroupName "RG-Name" -Ttl 300 -DnsRecords (New-AzDnsRecordConfig -IPv4Address "1.2.3.4")

# Create private DNS zone
New-AzPrivateDnsZone -Name "internal.example.com" -ResourceGroupName "RG-Name"

# Link VNet to private zone
New-AzPrivateDnsVirtualNetworkLink -Name "vnetLink" -ResourceGroupName "RG-Name" -ZoneName "internal.example.com" -VirtualNetworkId $vnet.Id
```

### Record Types
- **A**: IPv4 address
- **AAAA**: IPv6 address
- **CNAME**: Alias
- **MX**: Mail exchange
- **TXT**: Text records
- **SRV**: Service
- **PTR**: Pointer (reverse DNS)

---

## 9. Network Watcher

### Features
- Topology visualization
- Connection monitor
- Packet capture
- Flow logs
- Diagnostic tools

```powershell
# Enable NSG flow logs
Set-AzNetworkWatcherConfigFlowLog -NetworkWatcher $nw -TargetResourceId $nsg.Id -StorageId $storage.Id -Enabled $true

# Create packet capture
New-AzNetworkWatcherPacketCapture -NetworkWatcher $nw -TargetVirtualMachineId $vm.Id -PacketCaptureName "capture" -LocalFilePath "C:\captures\capture.pcap"

# Check connectivity
Test-AzNetworkWatcherConnectivity -NetworkWatcher $nw -SourceId $vm1.Id -DestinationAddress "10.0.2.4" -DestinationPort 80
```

---

## Key Exam Topics
✓ VNet creation and subnets
✓ VNet peering and gateway transit
✓ NSG rules and best practices
✓ Public and private IP addresses
✓ Load balancer and Application Gateway
✓ VPN Gateway (Site-to-Site, Point-to-Site)
✓ Express Route connectivity
✓ Azure DNS (public and private)
✓ Network Watcher for monitoring
✓ Service endpoints and private endpoints
✓ NAT Gateway for outbound connectivity

