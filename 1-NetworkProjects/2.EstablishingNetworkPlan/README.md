# Network Design Project 📡

## Project Overview

This project focuses on designing and simulating a secure, efficient, and cost-effective network for a client relocating to a new office. The network is built to meet the client’s current needs while accommodating future growth, using **Cisco Packet Tracer** for visualization and testing.

---

### Project Goals:
1. **Secure and Efficient Network Design**: Support multiple sectors with a robust infrastructure.
2. **Resource Optimization**: Allocate IP addresses, VLANs, and devices for scalability.
3. **Security Focus**: Implement VLANs, ACLs, RADIUS authentication, and centralized services for risk mitigation.
4. **Cost-Effectiveness**: Choose components and configurations that align with a budget-conscious design.

---

## Network Design and Structure

### Network Sectors:
The network is divided into four main sectors, each with dedicated IPs, VLANs, and security configurations, designed for future scalability.

- **Management (VLAN 10)**: 5 workstations  
- **Study (VLAN 20)**: 8 workstations  
- **Production (VLAN 30)**: 10 workstations  
- **Support (VLAN 40 & 50)**: 10 workstations in each support area  

### Switches and VLANs:
- **Switch4** links all sector switches and handles VLAN 50 (servers).
- **Switch7** connects DNS and DHCP servers in access mode on VLAN 50.

### IP Addressing Scheme:

| VLAN   | Sector           | IP Range           | Subnet Mask       | Default Gateway  |
|--------|------------------|--------------------|-------------------|------------------|
| VLAN 10| Management       | 192.168.10.0/24    | 255.255.255.0     | 192.168.10.1     |
| VLAN 20| Study            | 192.168.20.0/24    | 255.255.255.0     | 192.168.20.1     |
| VLAN 30| Production       | 192.168.30.0/24    | 255.255.255.0     | 192.168.30.1     |
| VLAN 40| Support 1        | 192.168.40.0/24    | 255.255.255.0     | 192.168.40.1     |
| VLAN 41| Support 2        | 192.168.41.0/24    | 255.255.255.0     | 192.168.41.1     |
| VLAN 50| Servers          | 192.168.50.0/24    | 255.255.255.0     | 192.168.50.1     |

---

## Security Design

### VLANs and ACLs:
- **VLAN Segmentation**: Each sector is isolated with VLANs to reduce unauthorized access.
- **Access Control Lists (ACLs)**: Implemented to restrict access between VLANs, protecting sensitive areas.
- **DMZ Setup**: VLAN 50 hosts critical services (DNS, DHCP) as a pseudo-DMZ.

### RADIUS Authentication:
Configured **RADIUS** for centralized authentication, improving security by managing user access centrally.

### Firewall and Router Configurations:
Due to Cisco Packet Tracer's limitations, a layered security approach is applied:
- **Router-based firewall**: Simulated between Router0 and Router1.
- **Router policies**: Restrict unauthorized traffic from accessing sensitive sectors.

---

## Device Configuration

### Switches:
- **Switch4**: Manages inter-VLAN routing and connects to Router0.
- **Switch7**: Connects DNS, DHCP, and FTP servers, configured for VLAN 50.

### Routers:
- **Router1**: Acts as the default gateway for VLANs, handling inter-VLAN routing.
  - IP: 192.168.10.1
- **Router2**: Manages external traffic, connected to the cloud via GigabitEthernet1/1.
  - IP: 203.0.113.2

---

## DHCP and DNS Setup

### DHCP Server:
- **Pool Name**: Management
- **Start IP Address**: 192.168.10.10 (for VLAN 10)
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.10.1
- **DNS Server**: 192.168.50.48
- Each VLAN has a dedicated DHCP pool with the correct IP range and gateway.

### DNS Server:
- **DNS IP**: 192.168.50.48
- **Resource Records**: Configured for domain-to-IP mappings (e.g., `www.company.local` → `192.168.50.48`).

---

## Testing Procedures

### DHCP Testing:
1. Set PCs to **DHCP mode**.
2. Verify that PCs in VLANs receive IPs from the correct DHCP pool.
3. Test connectivity to default gateways and the DNS server.

### DNS Testing:
1. Check DNS server reachability from all VLANs (`ping 192.168.50.48`).
2. Test domain resolution by pinging configured domain names.
3. Ensure proper IP addresses are resolved.

---

## Cost Breakdown

| Item                | Quantity | Cost per Unit    | Total Cost        |
|---------------------|----------|------------------|-------------------|
| Cisco Routers       | 2        | €1,200 – €1,800  | €2,400 – €3,600   |
| Cisco Switches      | 7        | €1,000 – €1,500  | €7,000 – €10,500  |
| Firewalls           | 1        | €1,500 – €3,000  | €1,500 – €3,000   |
| Workstations        | 33       | €600 – €800      | €19,800 – €26,400 |
| **Total Estimate**  |          |                  | **€30,700 – €43,500** |

---

## Conclusion

This network design achieves a secure, scalable, and budget-friendly setup tailored to the client’s needs, with VLAN segmentation, ACLs, and RADIUS authentication supporting a secure, high-performance environment. Comprehensive documentation, including simulation and configuration files, ensures easy maintenance and adaptability.

---
