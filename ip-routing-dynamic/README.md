# Multi-Area OSPF Enterprise Routing Architecture Lab

## Overview
This laboratory project demonstrates the design, deployment, and verification of a **Multi-Area OSPF (Open Shortest Path First)** dynamic routing protocol in an enterprise environment. The lab simulates multi-site WAN interconnections, isolating network domains, reducing SPF algorithm overhead, and maintaining scalable routing topology through an **Area Border Router (ABR)**.

---

## Network Topology & Addressing Table

![Multi-Area OSPF Topology](ip-routing-dynamic/multi-area-ospf-topology.png)

### Topology Architecture
* **Area 0 (Backbone Area):** Connects `R1` and `R2` (`FastEthernet0/0`).
* **Area Border Router (ABR):** `R2` acts as the ABR connecting **Area 0** to **Area 20**.
* **Area 20 (Non-Backbone Area):** Connects `R2` (`FastEthernet0/1`), `R3`, and `R4`.

### Addressing Details
| Device | Interface | IP Address / Mask | OSPF Area | Description |
| :--- | :--- | :--- | :--- | :--- |
| **R1** | `Fa0/0` | `10.0.0.1/24` | Area 0 | Physical Link to R2 |
| **R1** | `Loopback1` | `1.1.1.1/24` | Area 0 | Stable Test Endpoint / Router ID |
| **R2** | `Fa0/0` | `10.0.0.2/24` | Area 0 | Backbone Link to R1 |
| **R2** | `Fa0/1` | `11.0.0.1/24` | Area 20 | Area Border Link to R3 |
| **R3** | `Fa0/1` | `11.0.0.2/24` | Area 20 | Link to R2 (ABR) |
| **R3** | `Fa0/0` | `12.0.0.1/24` | Area 20 | Link to R4 |
| **R4** | `Fa0/0` | `12.0.0.2/24` | Area 20 | Link to R3 |
| **R4** | `Loopback1` | `4.4.4.4/24` | Area 20 | Target Endpoint / Router ID |

---

## Step-by-step Configuration

### Step 1: Interface IP & Loopback Configuration

#### Router 1 (R1)
![R1 IP Configuration](ip-routing-dynamic/R1-ip-config.png)

```cisco
R1# configure terminal
R1(config)# interface fastEthernet 0/0
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# interface loopback 1
R1(config-if)# ip address 1.1.1.1 255.255.255.0

Router 2 (R2 - ABR)
R2# configure terminal
R2(config)# interface fastEthernet 0/0
R2(config-if)# ip address 10.0.0.2 255.255.255.0
R2(config-if)# no shutdown
R2(config-if)# interface fastEthernet 0/1
R2(config-if)# ip address 11.0.0.1 255.255.255.0
R2(config-if)# no shutdown

Router 3 (R3)
R3# configure terminal
R3(config)# interface fastEthernet 0/1
R3(config-if)# ip address 11.0.0.2 255.255.255.0
R3(config-if)# no shutdown
R3(config-if)# interface fastEthernet 0/0
R3(config-if)# ip address 12.0.0.1 255.255.255.0
R3(config-if)# no shutdown

Router 4 (R4)
R4# configure terminal
R4(config)# interface fastEthernet 0/0
R4(config-if)# ip address 12.0.0.2 255.255.255.0
R4(config-if)# no shutdown
R4(config-if)# interface loopback 1
R4(config-if)# ip address 4.4.4.4 255.255.255.0

Step 2: Multi-Area OSPF Configuration
Router 1 (R1 - Area 0)
R1(config)# router ospf 1
R1(config-router)# network 10.0.0.0 0.0.0.255 area 0
R1(config-router)# network 1.1.1.0 0.0.0.255 area 0

Router 2 (R2 - Area Border Router / ABR)
R2(config)# router ospf 1
R2(config-router)# network 10.0.0.0 0.0.0.255 area 0
R2(config-router)# network 11.0.0.0 0.0.0.255 area 20

Router 3 (R3 - Area 20)
R3(config)# router ospf 1
R3(config-router)# network 11.0.0.0 0.0.0.255 area 20
R3(config-router)# network 12.0.0.0 0.0.0.255 area 20

Router 4 (R4 - Area 20)
R4(config)# router ospf 1
R4(config-router)# network 12.0.0.0 0.0.0.255 area 20
R4(config-router)# network 4.4.4.0 0.0.0.255 area 20


Verification & Database Inspection
OSPF Database Verification
Router 1 OSPF Database
Area Border Router (R2) OSPF Multi-Area Databases
Testing & End-to-End Verification
Forward Reachability & Routing Table (R1 to R4 Loopback)
Reverse Reachability Test (R4 to R1 Loopback)
Key Observations:
Inter-Area Routes (O IA): R1 successfully learned remote networks from Area 20 (4.4.4.4, 11.0.0.0/24, and 12.0.0.0/24) marked with the O IA prefix, indicating proper LSA Type 3 summarization via the ABR (R2).

Full Bidirectional Reachability: Both ICMP Ping tests (R1 to R4 and R4 to R1) achieved a 100% success rate (5/5) across areas.

Key Takeaways
Fault Isolation: Topology changes in Area 20 do not trigger SPF recalculations inside Area 0, maintaining network stability.

Scalability: Employing an ABR (R2) isolates Link-State Advertisements (LSAs) and optimizes CPU and memory resource consumption across all participating routers.

Loopback Testing: Eliminates physical host dependencies while acting as stable, deterministic OSPF Router IDs.
