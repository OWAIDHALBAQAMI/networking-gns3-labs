# Inter-VLAN Routing Project

## Overview

This project demonstrates the configuration of **Inter-VLAN Routing** using SVIs (Switched Virtual Interfaces) on IOU switches. This setup enables devices in different VLANs to communicate with each other through a trunk link connecting the switches.

## Objective

- Segment the network into separate VLANs
- Enable communication between devices in different VLANs (Inter-VLAN Communication)
- Verify successful connectivity through Ping tests

## Network Topology

```
                    IOU1 ←────→ IOU2
                   /  \          /  \
                e0/1  e0/2    e0/2  e0/3
                 /      \      /      \
               PC1      PC2   PC3     PC4
```

### Connections:
- **IOU1 e0/0** ↔ **IOU2 e0/0** (Trunk Link)
- **IOU1 e0/1** → **PC1** (Access VLAN 10)
- **IOU1 e0/2** → **PC2** (Access VLAN 20)
- **IOU2 e0/2** → **PC3** (Access VLAN 10)
- **IOU2 e0/3** → **PC4** (Access VLAN 20)

## Device Configuration

### PC1 (VLAN 10)
```
IP Address  : 10.0.0.10
Netmask     : 255.255.255.0
Gateway     : 10.0.0.1
MAC Address : 00:50:79:66:68:01
```

### PC2 (VLAN 20)
```
IP Address  : 192.168.1.10
Netmask     : 255.255.255.0
Gateway     : 192.168.1.1
MAC Address : 00:50:79:66:68:01
```

### PC3 (VLAN 10)
```
IP Address  : 10.0.0.20
Netmask     : 255.255.255.0
Gateway     : 10.0.0.1
MAC Address : 00:50:79:66:68:02
```

### PC4 (VLAN 20)
```
IP Address  : 192.168.1.20
Netmask     : 255.255.255.0
Gateway     : 192.168.1.1
MAC Address : 00:50:79:66:68:03
```

## Switch Configuration

### IOU1 - SVI Configuration

VLAN 10 SVI:
```
Interface Vlan10
 IP Address: 10.0.0.13
 Status: UP
```

VLAN 20 SVI:
```
Interface Vlan20
 IP Address: 192.168.1.13
 Status: UP
```

Interfaces:
- **e0/0**: Trunk (to IOU2)
- **e0/1**: Access - VLAN 10 (to PC1)
- **e0/2**: Access - VLAN 20 (to PC2)
- **e0/3**: Unused

### IOU2 - SVI Configuration

VLAN 10 SVI:
```
Interface Vlan10
 IP Address: 10.0.0.14
 Status: UP
```

VLAN 20 SVI:
```
Interface Vlan20
 IP Address: 192.168.1.14
 Status: UP
```

Interfaces:
- **e0/0**: Trunk (to IOU1)
- **e0/1**: Unused
- **e0/2**: Access - VLAN 10 (to PC3)
- **e0/3**: Access - VLAN 20 (to PC4)

## Test Results

### Ping Test Between Different VLANs ✅

```
PC1> ping 192.168.1.20

84 bytes from 192.168.1.20 icmp_seq=1 ttl=63 time=3.222 ms
84 bytes from 192.168.1.20 icmp_seq=2 ttl=63 time=16.089 ms
84 bytes from 192.168.1.20 icmp_seq=3 ttl=63 time=3.317 ms
84 bytes from 192.168.1.20 icmp_seq=4 ttl=63 time=2.981 ms
84 bytes from 192.168.1.20 icmp_seq=5 ttl=63 time=1.317 ms
```

**Result**: ✅ SUCCESS - Devices in different VLANs can communicate successfully

## Technical Specifications

| Item | Details |
|------|---------|
| Switch Type | IOU (IOSvL2) |
| Number of VLANs | 2 (VLAN 10, VLAN 20) |
| Routing Type | SVI-based Routing |
| Bridge Protocol | 802.1Q Trunk |
| Number of Devices | 4 PCs |
| Connection Status | ✅ Active and Documented |

## Key Features

1. **Trunk Configuration**: Port e0/0 on both switches configured as trunk for multi-VLAN traffic
2. **SVI (Switched Virtual Interface)**: Serves as Default Gateway for devices in each VLAN
3. **IP Routing**: IP routing enabled on switches to allow inter-VLAN communication
4. **TTL Value**: Response TTL value of 63 indicates the packet passed through one routing device (the switch)

## Steps to Replicate the Project

1. **Create VLANs**:
```
vlan 10
name VLAN10

vlan 20
name VLAN20
```

2. **Configure Interfaces**:
```
interface e0/0
switchport mode trunk
```

3. **Configure SVIs**:
```
interface vlan 10
ip address 10.0.0.13 255.255.255.0
no shutdown

interface vlan 20
ip address 192.168.1.13 255.255.255.0
no shutdown
```

4. **Enable IP Routing**:
```
ip routing
```

5. **Verify Connectivity**:
```
ping [IP Address]
```

## Project Status

✅ **Project Complete and Production Ready**

- All configurations successfully applied
- All tests show positive results
- Inter-VLAN connectivity working efficiently
- Full documentation provided

## Key Learning Points

- VLAN segmentation and access port configuration
- Trunk link setup using 802.1Q protocol
- SVI creation for inter-VLAN routing
- IP routing on Layer 3 switches
- Testing connectivity between isolated network segments
- Default gateway configuration for multiple VLANs

## Equipment Used

- 2x IOU Switches (IOSvL2 - Layer 3 capable)
- 4x Virtual PCs (VPCS)
- Ethernet connections

## Notes

- Both switches must have IP routing enabled
- SVI interface must be created for each VLAN that needs inter-VLAN routing
- The trunk link carries all VLAN traffic between the switches
- Access ports are configured with native VLAN settings
- TTL values help identify routing hops in packet responses
