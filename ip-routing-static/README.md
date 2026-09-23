Static IP Routing - CCNA Project
Overview

This project demonstrates Static Routing in Cisco networks. Data is routed manually between routers to connect multiple local networks.
R1 (1.0.0.1) ===== 1.0.0.0/24 ===== (1.0.0.2) R2
  |                                    |
  |                                    |
IOU1 --- PC1 (10.0.0.10)          IOU3 --- PC3 (12.0.0.10)
IOU2 --- PC2 (11.0.0.10)

Configuration
Router R1 - Static Routes
R1(config)#ip route 12.0.0.0 255.255.255.0 1.0.0.2

This route tells R1: "To reach network 12.0.0.0, send packets to R2 (1.0.0.2)"

Router R2 - Static Routes
R2(config)#ip route 10.0.0.0 255.255.255.0 1.0.0.1
R2(config)#ip route 11.0.0.0 255.255.255.0 1.0.0.1

These routes tell R2:

"To reach network 10.0.0.0 (PC1), send packets to R1 (1.0.0.1)"
"To reach network 11.0.0.0 (PC2), send packets to R1 (1.0.0.1)"
Test Results
✅ PC1 → PC3 (Ping Test)
PC1> ping 12.0.0.10
84 bytes from 12.0.0.10 icmp_seq=1 ttl=62 time=62.133 ms ✅
84 bytes from 12.0.0.10 icmp_seq=2 ttl=62 time=63.002 ms ✅
84 bytes from 12.0.0.10 icmp_seq=3 ttl=62 time=63.806 ms ✅
84 bytes from 12.0.0.10 icmp_seq=4 ttl=62 time=63.227 ms ✅
84 bytes from 12.0.0.10 icmp_seq=5 ttl=62 time=61.961 ms ✅

Result: All packets delivered successfully ✅

✅ PC3 → PC1 and PC2 (Return Path Test)
PC3> ping 10.0.0.10
84 bytes from 10.0.0.10 icmp_seq=1 ttl=61 time=61.180 ms ✅
84 bytes from 10.0.0.10 icmp_seq=2 ttl=61 time=61.647 ms ✅

PC3> ping 11.0.0.10
84 bytes from 11.0.0.10 icmp_seq=1 ttl=63 time=63.224 ms ✅
84 bytes from 11.0.0.10 icmp_seq=2 ttl=63 time=62.996 ms ✅

Result: Bidirectional communication works ✅

Key Commands
# View routing table
show ip route

# View interface status
show interface <interface>

# Test connectivity
ping <ip-address>

How It Works
PC1 sends a packet to PC3 (12.0.0.10)
R1 checks its routing table and finds the route to 12.0.0.0
R1 forwards the packet to R2 (1.0.0.2)
R2 receives the packet and forwards it to PC3
PC3 responds back through R2 → R1 → PC1 ✅

Summary

✅ Static routing configured on both routers
✅ All networks reachable from all PCs
✅ Bidirectional communication working
✅ Project complete
