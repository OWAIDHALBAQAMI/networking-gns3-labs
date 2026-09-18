# Inter-VLAN Routing with L3 Switch SVIs & Trunking

## 📌 Project Overview
This repository demonstrates a complete GNS3 lab implementation of **Inter-VLAN Routing** using **Switch Virtual Interfaces (SVIs)** configured across Cisco Layer 3 Switches (`IOU1` and `IOU2`). The switches are interconnected via an **IEEE 802.1Q Trunk Link** to seamlessly extend VLANs across the infrastructure.

---

## 📐 Topology Architecture
![Network Topology](Inter-VLAN-Routing/topology.png)

```text
                  +-----------------------------------+
                  |        IOU1 (L3 Core Switch)      |
                  |  SVIs: Vlan10 (.13) | Vlan20 (.13) |
                  +-----------------------------------+
                       /            |            \
           (Access V10)             |             (Access V20)
                  /          (Trunk e0/0)          \
             [PC1]                  |             [PC2]
         10.0.0.10/24               |          192.168.1.10/24
         GW: 10.0.0.13              |          GW: 192.168.1.13
                                    |
                  +-----------------------------------+
                  |        IOU2 (L3 Access Switch)    |
                  |  SVIs: Vlan10 (.14) | Vlan20 (.14) |
                  +-----------------------------------+
                       /                         \
           (Access V10)                           (Access V20)
                 /                                 \
            [PC3]                                 [PC4]
        10.0.0.20/24                           192.168.1.20/24
        GW: 10.0.0.14                          GW: 192.168.1.14


💻 End Hosts IP Configurations
PC1 Configuration
PC2 Configuration
PC3 Configuration
PC4 Configuration
⚙️ Switch Configurations
1. IOU1 (Layer 3 Core Switch)

configure terminal
vlan 10
 name HR_VLAN
vlan 20
 name IT_VLAN
exit

interface Ethernet0/1
 switchport mode access
 switchport access vlan 10
 no shutdown

interface Ethernet0/2
 switchport mode access
 switchport access vlan 20
 no shutdown

interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

interface vlan 10
 ip address 10.0.0.13 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.1.13 255.255.255.0
 no shutdown

ip routing

2. IOU2 (Layer 3 Access Switch)
configure terminal
vlan 10
 name HR_VLAN
vlan 20
 name IT_VLAN
exit

interface Ethernet0/2
 switchport mode access
 switchport access vlan 10
 no shutdown

interface Ethernet0/3
 switchport mode access
 switchport access vlan 20
 no shutdown

interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

interface vlan 10
 ip address 10.0.0.14 255.255.255.0
 no shutdown

interface vlan 20
 ip address 192.168.1.14 255.255.255.0
 no shutdown

ip routing

🔍 Troubleshooting & ResolutionDuring initial testing, inter-VLAN connectivity failed (ICMP Timeout).   Root Cause: The end host gateways were pointing to an unassigned IP address (10.0.0.1) instead of matching the active SVI IP address on their directly connected switch.   Fix: Re-configured PC1's gateway to 10.0.0.13 and PC4's gateway to 192.168.1.14, successfully resolving return-path routing.   End-to-End Ping Test Result (PC1 to PC4)


