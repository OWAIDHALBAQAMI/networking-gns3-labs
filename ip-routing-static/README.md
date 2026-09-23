# 🌐 Cisco Static Routing Configuration

Laboratory implementation of **IPv4 Static Routing** using Cisco Routers to enable inter-network communication across R1 and R2.

---

## 📌 Network Topology

![Network Topology](topology.png)

---

## ⚙️ Static Route Commands

### 🔹 Router 1 (R1) Configuration
Adding a static route to reach network `12.0.0.0/24` via R2 (`1.1.1.2`):

![R1 Static Route](R1-static-route.png)

```cisco
R1# configure terminal
R1(config)# ip route 12.0.0.0 255.255.255.0 1.1.1.2

🔹 Router 2 (R2) Configuration
Adding static routes to reach networks 10.0.0.0/24 and 11.0.0.0/24 via R1 (1.1.1.1):
R2# configure terminal
R2(config)# ip route 10.0.0.0 255.255.255.0 1.1.1.1
R2(config)# ip route 11.0.0.0 255.255.255.0 1.1.1.1
✅ Connectivity Verification (Ping Tests)
1️⃣ PC1 (10.0.0.10) ➡️ PC3 (12.0.0.10)
2️⃣ PC3 (12.0.0.10) ➡️ PC1 (10.0.0.10) & PC2 (11.0.0.10)

