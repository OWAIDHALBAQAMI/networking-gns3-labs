# Cisco IOS Router as DHCP Server Implementation Lab

## Project Overview
This project documents the implementation and validation of a **Cisco IOS Router acting as a DHCP Server** within a GNS3 enterprise simulation environment. The objective is to automate dynamic IP allocation across local hosts while enforcing **IP Conflict Prevention** using DHCP address exclusions.

---

## Network Topology
The architecture features a Cisco Router (`R1`) serving as the central DHCP gateway, connected via a FastEthernet interface through a Layer 2 Switch (`SW`) to three end-host devices (`PC1`, `PC2`, `PC3`).

![DHCP Network Topology](dhcp_topology.png)

### Device & Interface Details
* **DHCP Server**: Cisco Router `R1` (`FastEthernet0/0` — `10.0.0.1/24`)
* **Switch**: Generic L2 Switch (`SW`)
* **Clients**: `PC1` (port `e0/0`), `PC2` (port `e0/1`), `PC3` (port `e0/2`)

---

## Configuration

### Cisco IOS DHCP Server Setup
The DHCP pool `owiadh` is configured to service the `10.0.0.0/24` network. To protect critical enterprise resources (gateways, servers, static hosts) from dynamic lease conflicts, the address range **`10.0.0.4` through `10.0.0.10`** was explicitly excluded.

![R1 DHCP Configuration](r1_dhcp_full_config.png)

### CLI Commands Executed
```cisconet
! Interface Configuration
interface FastEthernet0/0
 ip address 10.0.0.1 255.255.255.0
 no shutdown

! DHCP Pool & Exclusion Rules
ip dhcp pool owiadh
 network 10.0.0.0 255.255.255.0
 default-router 10.0.0.1
 dns-server 8.8.8.8
 ip dhcp excluded-address 10.0.0.4 10.0.0.10
