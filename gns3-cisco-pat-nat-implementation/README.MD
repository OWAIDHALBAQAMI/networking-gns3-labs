# Enterprise NAT & PAT Implementation in Cisco IOS: Simulating Private-to-Public Internet Translation

## 📌 Project Overview
This project demonstrates the implementation and verification of Network Address Translation (NAT) with Port Address Translation (PAT / NAT Overload) using Cisco IOS in GNS3. The setup simulates a real-world enterprise environment connecting an internal private network (`10.0.0.0/24`) to an external ISP network (`20.0.0.0/24`) over a WAN link (`40.0.0.0/24`).

---

## 📐 Network Topology
The topology includes R1 acting as the Internal Edge Gateway, R2 representing the Service Provider (ISP), and client PCs on both internal and external subnets.

![NAT Network Topology](nat_topology.png)

---

## ⚙️ Initial Setup & Routing Baseline

### 1. R1 Gateway Configuration
Configuring LAN/WAN interface IP addresses on R1 and establishing a default static route pointing toward the ISP edge router (R2).

![R1 Initial Config](r1_initial_config.png)

### 2. R2 ISP Router Configuration
Configuring ISP interface IP addresses on R2. R2 deliberately maintains no routing knowledge of the internal private subnet (`10.0.0.0/24`), accurately reflecting public Internet routing behavior.

![R2 Initial Config](r2_initial_config.png)

---

## 🚀 PAT (NAT Overload) Configuration & Traffic Flow

### 1. PAT Configuration on R1
Enforcing NAT inside/outside interface designations, defining an Access Control List (ACL) to match internal private traffic, and enabling PAT (Overload) on interface FastEthernet0/0.

![PAT Config](pat_config.png)

### 2. Connectivity Verification
Testing outbound reachability from internal host PC2 (`10.0.0.20`) across the simulated ISP network to external target `20.0.0.10`.

![Ping Success Proof](ping_success.png)

### 3. Active Translation Table Verification
Inspecting the active NAT translation table on R1. The output confirms that private source addresses (`10.0.0.10` and `10.0.0.20`) are dynamically mapped to R1's single public IP address (`40.0.0.1`) using unique port identifiers.

![NAT Translation Proof](nat_translations_proof.png)

---

## 🔑 Key Technical Takeaways
1. **Private Address Hiding:** PAT enables internal hosts with private IPv4 addresses (RFC 1918) to communicate across public networks using a single public IPv4 address.
2. **Port-Level Multiplexing:** Layer 4 port numbers allow the gateway to distinguish and route returning traffic back to the correct originating internal host.
3. **ISP Routing Realism:** ISP core routers do not route non-routable private IP addresses; proper NAT translation at the enterprise edge is essential for Internet communication.
