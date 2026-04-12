# EASTCOAST_FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [ISIS CLNS interfaces](#isis-clns-interfaces)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| EASTCOAST_FABRIC | spine | SW101-SITE1-B | 172.20.20.11/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | spine | SW102-SITE1-R | 172.20.20.12/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW103-SITE1-P | 172.20.20.13/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW201-SITE2-B | 172.20.20.21/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW202-SITE2-R | 172.20.20.22/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW203-SITE2-P | 172.20.20.23/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW301-SITE3-P | 172.20.20.31/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW401-SITE4-B | 172.20.20.41/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW402-SITE4-R | 172.20.20.42/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW501-SITE5-B | 172.20.20.51/24 | cEOSLab | Provisioned | - |
| EASTCOAST_FABRIC | l3leaf | SW502-SITE5-R | 172.20.20.52/24 | cEOSLab | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| spine | SW101-SITE1-B | Ethernet43 | l3leaf | SW401-SITE4-B | Ethernet45 |
| spine | SW101-SITE1-B | Ethernet45 | l3leaf | SW301-SITE3-P | Ethernet35 |
| spine | SW101-SITE1-B | Ethernet47 | l3leaf | SW201-SITE2-B | Ethernet47 |
| spine | SW101-SITE1-B | Ethernet56/1 | l3leaf | SW103-SITE1-P | Ethernet53/1 |
| spine | SW102-SITE1-R | Ethernet43 | l3leaf | SW402-SITE4-R | Ethernet45 |
| spine | SW102-SITE1-R | Ethernet45 | l3leaf | SW301-SITE3-P | Ethernet36 |
| spine | SW102-SITE1-R | Ethernet47 | l3leaf | SW202-SITE2-R | Ethernet47 |
| spine | SW102-SITE1-R | Ethernet56/1 | l3leaf | SW103-SITE1-P | Ethernet54/1 |
| l3leaf | SW201-SITE2-B | Ethernet56/1 | l3leaf | SW203-SITE2-P | Ethernet53/1 |
| l3leaf | SW202-SITE2-R | Ethernet56/1 | l3leaf | SW203-SITE2-P | Ethernet54/1 |
| l3leaf | SW401-SITE4-B | Ethernet47 | l3leaf | SW501-SITE5-B | Ethernet39 |
| l3leaf | SW402-SITE4-R | Ethernet47 | l3leaf | SW502-SITE5-R | Ethernet39 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.11.50.0/24 | 256 | 12 | 4.69 % |
| 10.101.50.0/24 | 256 | 6 | 2.35 % |
| 10.102.50.0/24 | 256 | 6 | 2.35 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| SW101-SITE1-B | Ethernet43 | 10.101.50.6/31 | SW401-SITE4-B | Ethernet45 | 10.101.50.7/31 |
| SW101-SITE1-B | Ethernet45 | 10.11.50.16/31 | SW301-SITE3-P | Ethernet35 | 10.11.50.17/31 |
| SW101-SITE1-B | Ethernet47 | 10.101.50.4/31 | SW201-SITE2-B | Ethernet47 | 10.101.50.5/31 |
| SW101-SITE1-B | Ethernet56/1 | 10.11.50.8/31 | SW103-SITE1-P | Ethernet53/1 | 10.11.50.9/31 |
| SW102-SITE1-R | Ethernet43 | 10.102.50.6/31 | SW402-SITE4-R | Ethernet45 | 10.102.50.7/31 |
| SW102-SITE1-R | Ethernet45 | 10.11.50.18/31 | SW301-SITE3-P | Ethernet36 | 10.11.50.19/31 |
| SW102-SITE1-R | Ethernet47 | 10.102.50.4/31 | SW202-SITE2-R | Ethernet47 | 10.102.50.5/31 |
| SW102-SITE1-R | Ethernet56/1 | 10.11.50.10/31 | SW103-SITE1-P | Ethernet54/1 | 10.11.50.11/31 |
| SW201-SITE2-B | Ethernet56/1 | 10.11.50.12/31 | SW203-SITE2-P | Ethernet53/1 | 10.11.50.13/31 |
| SW202-SITE2-R | Ethernet56/1 | 10.11.50.14/31 | SW203-SITE2-P | Ethernet54/1 | 10.11.50.15/31 |
| SW401-SITE4-B | Ethernet47 | 10.101.50.8/31 | SW501-SITE5-B | Ethernet39 | 10.101.50.9/31 |
| SW402-SITE4-R | Ethernet47 | 10.102.50.8/31 | SW502-SITE5-R | Ethernet39 | 10.102.50.9/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.11.30.0/24 | 256 | 3 | 1.18 % |
| 10.101.30.0/24 | 256 | 4 | 1.57 % |
| 10.102.30.0/24 | 256 | 4 | 1.57 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| EASTCOAST_FABRIC | SW101-SITE1-B | 10.101.30.1/32 |
| EASTCOAST_FABRIC | SW102-SITE1-R | 10.102.30.1/32 |
| EASTCOAST_FABRIC | SW103-SITE1-P | 10.11.30.3/32 |
| EASTCOAST_FABRIC | SW201-SITE2-B | 10.101.30.3/32 |
| EASTCOAST_FABRIC | SW202-SITE2-R | 10.102.30.3/32 |
| EASTCOAST_FABRIC | SW203-SITE2-P | 10.11.30.4/32 |
| EASTCOAST_FABRIC | SW301-SITE3-P | 10.11.30.5/32 |
| EASTCOAST_FABRIC | SW401-SITE4-B | 10.101.30.4/32 |
| EASTCOAST_FABRIC | SW402-SITE4-R | 10.102.30.4/32 |
| EASTCOAST_FABRIC | SW501-SITE5-B | 10.101.30.5/32 |
| EASTCOAST_FABRIC | SW502-SITE5-R | 10.102.30.5/32 |

### ISIS CLNS interfaces

| POD | Node | CLNS Address |
| --- | ---- | ------------ |
| EASTCOAST_FABRIC | SW101-SITE1-B | 49.0001.0101.0103.0001.00 |
| EASTCOAST_FABRIC | SW102-SITE1-R | 49.0001.0101.0203.0001.00 |
| EASTCOAST_FABRIC | SW103-SITE1-P | 49.0001.0100.1103.0003.00 |
| EASTCOAST_FABRIC | SW201-SITE2-B | 49.0001.0101.0103.0003.00 |
| EASTCOAST_FABRIC | SW202-SITE2-R | 49.0001.0101.0203.0003.00 |
| EASTCOAST_FABRIC | SW203-SITE2-P | 49.0001.0100.1103.0004.00 |
| EASTCOAST_FABRIC | SW301-SITE3-P | 49.0001.0100.1103.0005.00 |
| EASTCOAST_FABRIC | SW401-SITE4-B | 49.0001.0101.0103.0004.00 |
| EASTCOAST_FABRIC | SW402-SITE4-R | 49.0001.0101.0203.0004.00 |
| EASTCOAST_FABRIC | SW501-SITE5-B | 49.0001.0101.0103.0005.00 |
| EASTCOAST_FABRIC | SW502-SITE5-R | 49.0001.0101.0203.0005.00 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 10.11.40.0/24 | 256 | 3 | 1.18 % |
| 10.101.40.0/24 | 256 | 4 | 1.57 % |
| 10.102.40.0/24 | 256 | 4 | 1.57 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| EASTCOAST_FABRIC | SW101-SITE1-B | 10.101.40.1/32 |
| EASTCOAST_FABRIC | SW102-SITE1-R | 10.102.40.1/32 |
| EASTCOAST_FABRIC | SW103-SITE1-P | 10.11.40.3/32 |
| EASTCOAST_FABRIC | SW201-SITE2-B | 10.101.40.3/32 |
| EASTCOAST_FABRIC | SW202-SITE2-R | 10.102.40.3/32 |
| EASTCOAST_FABRIC | SW203-SITE2-P | 10.11.40.4/32 |
| EASTCOAST_FABRIC | SW301-SITE3-P | 10.11.40.5/32 |
| EASTCOAST_FABRIC | SW401-SITE4-B | 10.101.40.4/32 |
| EASTCOAST_FABRIC | SW402-SITE4-R | 10.102.40.4/32 |
| EASTCOAST_FABRIC | SW501-SITE5-B | 10.101.40.5/32 |
| EASTCOAST_FABRIC | SW502-SITE5-R | 10.102.40.5/32 |
