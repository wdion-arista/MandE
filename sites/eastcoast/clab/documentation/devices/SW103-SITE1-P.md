# SW103-SITE1-P

## Table of Contents

- [Management](#management)
  - [Banner](#banner)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management SSH](#management-ssh)
  - [Management Console](#management-console)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [RADIUS Server](#radius-server)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
  - [AAA Accounting](#aaa-accounting)
- [Aliases Device Configuration](#aliases-device-configuration)
- [DHCP Server](#dhcp-server)
  - [DHCP Servers Summary](#dhcp-servers-summary)
  - [DHCP Server Configuration](#dhcp-server-configuration)
  - [DHCP Server Interfaces](#dhcp-server-interfaces)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Flow Tracking](#flow-tracking)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router ISIS](#router-isis)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [802.1X Port Security](#8021x-port-security)
  - [802.1X Summary](#8021x-summary)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [IP DHCP Snooping](#ip-dhcp-snooping)
  - [IP DHCP Snooping Device Configuration](#ip-dhcp-snooping-device-configuration)
- [Errdisable](#errdisable)
  - [Errdisable Summary](#errdisable-summary)

## Management

### Banner

#### Login Banner

```text
***********************************************
Go away!
EOF
```

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | OOB_MANAGEMENT | oob | default | 172.20.20.13/24 | 172.20.20.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | OOB_MANAGEMENT | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description OOB_MANAGEMENT
   no shutdown
   ip address 172.20.20.13/24
```

### DNS Domain

DNS domain: lab.example.com

#### DNS Domain Device Configuration

```eos
dns domain lab.example.com
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 8.8.8.8 | default | - |
| 4.2.2.1 | default | - |
| 8.8.8.8 | Production | - |
| 4.2.2.1 | Production | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf default 4.2.2.1
ip name-server vrf Production 4.2.2.1
ip name-server vrf default 8.8.8.8
ip name-server vrf Production 8.8.8.8
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **Canada/Eastern**.

#### Clock Device Configuration

```eos
!
clock timezone Canada/Eastern
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 23.133.168.244 | default | - | - | - | - | - | - | - | - |
| 216.232.132.95 | default | - | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server 23.133.168.244
ntp server 216.232.132.95
```

### Management SSH

#### VRFs

| VRF | Enabled | IPv4 ACL | IPv6 ACL |
| --- | ------- | -------- | -------- |
| default | True | - | - |

#### Other SSH Settings

| Idle Timeout | Connection Limit | Max from a single Host | Ciphers | Key-exchange methods | MAC algorithms | Hostkey server algorithms |
| ------------ | ---------------- | ---------------------- | ------- | -------------------- | -------------- | ------------------------- |
| 60 | 5 | - | default | default | default | default |

#### Management SSH Device Configuration

```eos
!
management ssh
   idle-timeout 60
   connection limit 5
```

### Management Console

#### Management Console Timeout

Management Console Timeout is set to **300** minutes.

#### Management Console Device Configuration

```eos
!
management console
   idle-timeout 300
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no protocol http
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| cvpadmin | 15 | network-admin | False | - |
| pcapper | - | - | False | /bin/bash |
| service | - | - | False | /bin/bash |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username admin ssh-key ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJF7OT/UyUNugQBV/6sn/7FPB5glZfb2KzjBmlkxyWdx westley.dion@Westleys-MacBook-Pro-2.local
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
username cvpadmin ssh-key ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJF7OT/UyUNugQBV/6sn/7FPB5glZfb2KzjBmlkxyWdx westley.dion@Westleys-MacBook-Pro-2.local
username pcapper shell /bin/bash secret sha512 <removed>
username pcapper ssh-key ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJF7OT/UyUNugQBV/6sn/7FPB5glZfb2KzjBmlkxyWdx westley.dion@Westleys-MacBook-Pro-2.local
username service shell /bin/bash secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

### RADIUS Server

#### RADIUS Server Hosts

| VRF | RADIUS Servers | TLS | SSL Profile | Timeout | Retransmit |
| --- | -------------- | --- | ----------- | ------- | ---------- |
| default | radsec.beta.agni.arista.io | True | agni-server | - | - |

#### RADIUS Server Device Configuration

```eos
!
radius-server host radsec.beta.agni.arista.io tls ssl-profile agni-server
```

### AAA Server Groups

#### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| agni-server-group | radius | default | radsec.beta.agni.arista.io tls |

#### AAA Server Groups Device Configuration

```eos
!
aaa group server radius agni-server-group
   server radsec.beta.agni.arista.io tls
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |

#### AAA Authentication Device Configuration

```eos
aaa authentication dot1x default group agni-server-group
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

### AAA Accounting

#### AAA Accounting Summary

| Type | Commands | Record type | Groups | Logging |
| ---- | -------- | ----------- | ------ | ------- |
| Dot1x - Default | - | start-stop | agni-server-group | - |

#### AAA Accounting Device Configuration

```eos
aaa accounting dot1x default start-stop group agni-server-group
```

## Aliases Device Configuration

```eos
alias cc clear counters
alias cp clear platform trident counters
alias senz show interface counter error | nz
alias shmc show int | awk '/^[A-Z]/ { intf = ck216 } /, address is/ { print intf, }'
alias snz show interface counter | nz
alias sqnz show interface counter queue | nz
alias srnz show interface counter rate | nz
alias ps_show_bess bash ps -ef | grep -i Bess
alias check_terminattr show agent TerminAttr log | tail 

!
```

## DHCP Server

### DHCP Servers Summary

| DHCP Server Enabled | VRF | IPv4 DNS Domain | IPv4 DNS Servers | IPv4 Bootfile | IPv4 Lease Time | IPv6 DNS Domain | IPv6 DNS Servers | IPv6 Bootfile | IPv6 Lease Time |
| ------------------- | --- | --------------- | ---------------- | ------------- | --------------- | --------------- | ---------------- | ------------- | --------------- |
| True | Production | - | 4.2.2.1, 8.8.8.8 | - | - | - | - | - | - |

#### VRF Production DHCP Server

##### Subnets

| Subnet | Name | DNS Servers | Default Gateway | Lease Time | Ranges |
| ------ | ---- | ----------- | --------------- | ---------- | ------ |
| 10.1.12.0/24 | Network Management | - | 10.1.12.1 | - | 10.1.12.10-10.1.12.250 |
| 10.1.13.0/24 | Operations | - | 10.1.13.1 | - | 10.1.13.10-10.1.13.250 |
| 10.1.20.0/24 | Intercom | - | 10.1.20.1 | - | 10.1.20.10-10.1.20.250 |
| 10.1.35.0/24 | SRT Blue | - | 10.1.35.1 | - | 10.1.35.10-10.1.35.250 |
| 10.1.36.0/24 | SRT Red | - | 10.1.36.1 | - | 10.1.36.10-10.1.36.250 |
| 10.1.40.0/24 | Data Transfer - EVS | - | 10.1.40.1 | - | 10.1.40.10-10.1.40.250 |
| 10.1.84.0/24 | WAN_1G | - | 10.1.84.1 | - | 10.1.84.10-10.1.84.250 |

##### IPv4 Vendor Options

| Vendor ID | Sub-option Code | Sub-option Type | Sub-option Data |
| --------- | ----------------| --------------- | --------------- |
| NTP | 42 | array ipv4-address | 216.232.132.95 23.133.168.244 173.183.146.26 208.73.56.29 |

### DHCP Server Configuration

```eos
!
dhcp server vrf Production
   dns server ipv4 4.2.2.1 8.8.8.8
   !
   subnet 10.1.12.0/24
      !
      range 10.1.12.10 10.1.12.250
      name Network Management
      default-gateway 10.1.12.1
   !
   subnet 10.1.13.0/24
      !
      range 10.1.13.10 10.1.13.250
      name Operations
      default-gateway 10.1.13.1
   !
   subnet 10.1.20.0/24
      !
      range 10.1.20.10 10.1.20.250
      name Intercom
      default-gateway 10.1.20.1
   !
   subnet 10.1.35.0/24
      !
      range 10.1.35.10 10.1.35.250
      name SRT Blue
      default-gateway 10.1.35.1
   !
   subnet 10.1.36.0/24
      !
      range 10.1.36.10 10.1.36.250
      name SRT Red
      default-gateway 10.1.36.1
   !
   subnet 10.1.40.0/24
      !
      range 10.1.40.10 10.1.40.250
      name Data Transfer - EVS
      default-gateway 10.1.40.1
   !
   subnet 10.1.84.0/24
      !
      range 10.1.84.10 10.1.84.250
      name WAN_1G
      default-gateway 10.1.84.1
   !
   vendor-option ipv4 NTP
      sub-option 42 type array ipv4-address data 216.232.132.95 23.133.168.244 173.183.146.26 208.73.56.29
```

### DHCP Server Interfaces

| Interface name | DHCP IPv4 | DHCP IPv6 |
| -------------- | --------- | --------- |
| Vlan112 | True | - |
| Vlan113 | True | - |
| Vlan120 | True | - |
| Vlan135 | True | - |
| Vlan136 | True | - |
| Vlan140 | True | - |
| Vlan184 | True | - |

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | default | token-secure,/tmp/cv-onboarding-token | ale,flexCounter,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |
| gzip | apiserver.arista.io:443 | default | token-secure,/tmp/cv-onboarding-token-wrdion | ale,flexCounter,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvopt CVaaS.addr=apiserver.arista.io:443 -cvopt CVaaS.auth=token-secure,/tmp/cv-onboarding-token -cvopt CVaaS.vrf=default -cvopt CVaaS.sourceintf=Management0 -cvopt CVaaS-wrdion.addr=apiserver.arista.io:443 -cvopt CVaaS-wrdion.auth=token-secure,/tmp/cv-onboarding-token-wrdion -cvopt CVaaS-wrdion.vrf=default -cvopt CVaaS-wrdion.sourceintf=Vlan11 -disableaaa -smashexcludes=ale,flexCounter,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### Flow Tracking

#### Flow Tracking Sampled

| Sample Size | Minimum Sample Size | Hardware Offload for IPv4 | Hardware Offload for IPv6 | Encapsulations |
| ----------- | ------------------- | ------------------------- | ------------------------- | -------------- |
| 10000 | default | disabled | disabled | ipv4, ipv6, mpls |

##### Trackers Summary

| Tracker Name | Record Export On Inactive Timeout | Record Export On Interval | MPLS | Number of Exporters | Applied On | Table Size |
| ------------ | --------------------------------- | ------------------------- | ---- | ------------------- | ---------- | ---------- |
| FLOW-TRACKER | 70000 | 300000 | - | 1 | Ethernet53/1<br>Ethernet54/1<br>Ethernet50 | - |

##### Exporters Summary

| Tracker Name | Exporter Name | Collector IP/Host | Collector Port | Local Interface |
| ------------ | ------------- | ----------------- | -------------- | --------------- |
| FLOW-TRACKER | CV-TELEMETRY | 127.0.0.1 | - | Loopback0 |

#### Flow Tracking Device Configuration

```eos
!
flow tracking sampled
   encapsulation ipv4 ipv6 mpls
   sample 10000
   tracker FLOW-TRACKER
      record export on inactive timeout 70000
      record export on interval 300000
      exporter CV-TELEMETRY
         collector 127.0.0.1
         local interface Loopback0
         template interval 3600000
   no shutdown
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
spanning-tree mst 0 priority 32768
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 110 | SITE5akiMgmt | - |
| 112 | NetworkMgmtSite1 | - |
| 113 | EdgeDeviceControlSite1 | - |
| 120 | IntercomSite1 | - |
| 135 | SRTTrafficBlue | - |
| 136 | SRTTrafficRed | - |
| 140 | DataTransferSite1 | - |
| 150 | ILD1_10mbps_client_site1 | - |
| 184 | WAN_1000mbps_prod_site1 | - |
| 999 | MGMT | - |

### VLANs Device Configuration

```eos
!
vlan 110
   name SITE5akiMgmt
!
vlan 112
   name NetworkMgmtSite1
!
vlan 113
   name EdgeDeviceControlSite1
!
vlan 120
   name IntercomSite1
!
vlan 135
   name SRTTrafficBlue
!
vlan 136
   name SRTTrafficRed
!
vlan 140
   name DataTransferSite1
!
vlan 150
   name ILD1_10mbps_client_site1
!
vlan 184
   name WAN_1000mbps_prod_site1
!
vlan 999
   name MGMT
```

## Interfaces

### Interface Defaults

#### Interface Defaults Summary

- Default Ethernet Interface Shutdown: True

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   ethernet
      shutdown
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet50 | SERVER_site1-server1_Ethernet3 | access | 135 | - | - | - |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet53/1 | P2P_SW101-SITE1-B_Ethernet56/1 | - | 10.11.50.9/31 | default | 1500 | False | - | - |
| Ethernet54/1 | P2P_SW102-SITE1-R_Ethernet56/1 | - | 10.11.50.11/31 | default | 1500 | False | - | - |

##### ISIS

| Interface | Channel Group | ISIS Instance | ISIS BFD | ISIS Metric | Mode | ISIS Circuit Type | Hello Padding | ISIS Authentication Mode |
| --------- | ------------- | ------------- | -------- | ----------- | ---- | ----------------- | ------------- | ------------------------ |
| Ethernet53/1 | - | EVPN_UNDERLAY | - | 50 | point-to-point | level-2 | - | - |
| Ethernet54/1 | - | EVPN_UNDERLAY | - | 50 | point-to-point | level-2 | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet50
   description SERVER_site1-server1_Ethernet3
   no shutdown
   switchport access vlan 135
   switchport mode access
   switchport
   flow tracker sampled FLOW-TRACKER
   spanning-tree portfast
   spanning-tree bpduguard enable
!
interface Ethernet53/1
   description P2P_SW101-SITE1-B_Ethernet56/1
   no shutdown
   mtu 1500
   no switchport
   flow tracker sampled FLOW-TRACKER
   ip address 10.11.50.9/31
   isis enable EVPN_UNDERLAY
   isis circuit-type level-2
   isis metric 50
   isis network point-to-point
!
interface Ethernet54/1
   description P2P_SW102-SITE1-R_Ethernet56/1
   no shutdown
   mtu 1500
   no switchport
   flow tracker sampled FLOW-TRACKER
   ip address 10.11.50.11/31
   isis enable EVPN_UNDERLAY
   isis circuit-type level-2
   isis metric 50
   isis network point-to-point
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.11.30.3/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.11.40.3/32 |
| Loopback10 | INBAND_MGMT | Production | 10.0.10.13/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback10 | INBAND_MGMT | Production | - |

##### ISIS

| Interface | ISIS instance | ISIS metric | Interface mode |
| --------- | ------------- | ----------- | -------------- |
| Loopback0 | EVPN_UNDERLAY | - | passive |
| Loopback1 | EVPN_UNDERLAY | - | passive |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.11.30.3/32
   isis enable EVPN_UNDERLAY
   isis passive
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.11.40.3/32
   isis enable EVPN_UNDERLAY
   isis passive
!
interface Loopback10
   description INBAND_MGMT
   vrf Production
   ip address 10.0.10.13/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan112 | NetworkMgmtSite1 | Production | - | False |
| Vlan113 | EdgeDeviceControlSite1 | Production | - | False |
| Vlan120 | IntercomSite1 | Production | - | False |
| Vlan135 | SRTTrafficBlue | Production | - | False |
| Vlan136 | SRTTrafficRed | Production | - | False |
| Vlan140 | DataTransferSite1 | Production | - | False |
| Vlan184 | WAN_1000mbps_prod_site1 | Production | - | False |
| Vlan999 | MGMT | MGMT | - | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan112 |  Production  |  10.1.12.1/24  |  -  |  -  |  -  |  -  |
| Vlan113 |  Production  |  10.1.13.1/24  |  -  |  -  |  -  |  -  |
| Vlan120 |  Production  |  10.1.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan135 |  Production  |  10.1.35.1/24  |  -  |  -  |  -  |  -  |
| Vlan136 |  Production  |  10.1.36.1/24  |  -  |  -  |  -  |  -  |
| Vlan140 |  Production  |  10.1.40.1/24  |  -  |  -  |  -  |  -  |
| Vlan184 |  Production  |  10.1.84.1/24  |  -  |  -  |  -  |  -  |
| Vlan999 |  MGMT  |  -  |  10.1.99.1/24  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan112
   description NetworkMgmtSite1
   no shutdown
   vrf Production
   ip address 10.1.12.1/24
   dhcp server ipv4
!
interface Vlan113
   description EdgeDeviceControlSite1
   no shutdown
   vrf Production
   ip address 10.1.13.1/24
   dhcp server ipv4
!
interface Vlan120
   description IntercomSite1
   no shutdown
   vrf Production
   ip address 10.1.20.1/24
   dhcp server ipv4
!
interface Vlan135
   description SRTTrafficBlue
   no shutdown
   vrf Production
   ip address 10.1.35.1/24
   dhcp server ipv4
!
interface Vlan136
   description SRTTrafficRed
   no shutdown
   vrf Production
   ip address 10.1.36.1/24
   dhcp server ipv4
!
interface Vlan140
   description DataTransferSite1
   no shutdown
   vrf Production
   ip address 10.1.40.1/24
   dhcp server ipv4
!
interface Vlan184
   description WAN_1000mbps_prod_site1
   no shutdown
   vrf Production
   ip address 10.1.84.1/24
   dhcp server ipv4
!
interface Vlan999
   description MGMT
   no shutdown
   vrf MGMT
   ip address virtual 10.1.99.1/24
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 110 | 10110 | - | - |
| 112 | 10112 | - | - |
| 113 | 10113 | - | - |
| 120 | 10120 | - | - |
| 135 | 10135 | - | - |
| 136 | 10136 | - | - |
| 140 | 10140 | - | - |
| 150 | 10150 | - | - |
| 184 | 10184 | - | - |
| 999 | 10999 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Overlay Multicast Group to Encap Mappings |
| --- | --- | ----------------------------------------- |
| MGMT | 11999 | - |
| Production | 11111 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description SW103-SITE1-P_VTEP
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 110 vni 10110
   vxlan vlan 112 vni 10112
   vxlan vlan 113 vni 10113
   vxlan vlan 120 vni 10120
   vxlan vlan 135 vni 10135
   vxlan vlan 136 vni 10136
   vxlan vlan 140 vni 10140
   vxlan vlan 150 vni 10150
   vxlan vlan 184 vni 10184
   vxlan vlan 999 vni 10999
   vxlan vrf MGMT vni 11999
   vxlan vrf Production vni 11111
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:dc:13

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:13
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | True |
| Production | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf MGMT
ip routing vrf Production
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |
| MGMT | false |
| Production | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| default | 0.0.0.0/0 | 172.20.20.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 172.20.20.1
```

### Router ISIS

#### Router ISIS Summary

| Settings | Value |
| -------- | ----- |
| Instance | EVPN_UNDERLAY |
| Net-ID | 49.0001.0100.1103.0003.00 |
| Type | level-2 |
| Router-ID | 10.11.30.3 |
| Log Adjacency Changes | True |

#### ISIS Interfaces Summary

| Interface | ISIS Instance | ISIS Metric | Interface Mode |
| --------- | ------------- | ----------- | -------------- |
| Ethernet53/1 | EVPN_UNDERLAY | 50 | point-to-point |
| Ethernet54/1 | EVPN_UNDERLAY | 50 | point-to-point |
| Loopback0 | EVPN_UNDERLAY | - | passive |
| Loopback1 | EVPN_UNDERLAY | - | passive |

#### ISIS IPv4 Address Family Summary

| Settings | Value |
| -------- | ----- |
| IPv4 Address-family Enabled | True |
| Maximum-paths | 4 |

#### Router ISIS Device Configuration

```eos
!
router isis EVPN_UNDERLAY
   net 49.0001.0100.1103.0003.00
   router-id ipv4 10.11.30.3
   is-type level-2
   log-adjacency-changes
   !
   address-family ipv4 unicast
      maximum-paths 4
   !
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65103 | 10.11.30.3 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.101.30.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.102.30.1 | 65200 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Peer-tag In | Peer-tag Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ----------- | ------------ | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True | - | - | - | - | default | - |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 110 | 10.11.30.3:10110 | 10110:10110 | - | - | learned |
| 112 | 10.11.30.3:10112 | 10112:10112 | - | - | learned |
| 113 | 10.11.30.3:10113 | 10113:10113 | - | - | learned |
| 120 | 10.11.30.3:10120 | 10120:10120 | - | - | learned |
| 135 | 10.11.30.3:10135 | 10135:10135 | - | - | learned |
| 136 | 10.11.30.3:10136 | 10136:10136 | - | - | learned |
| 140 | 10.11.30.3:10140 | 10140:10140 | - | - | learned |
| 150 | 10.11.30.3:10150 | 10150:10150 | - | - | learned |
| 184 | 10.11.30.3:10184 | 10184:10184 | - | - | learned |
| 999 | 10.11.30.3:10999 | 10999:10999 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| MGMT | 10.11.30.3:11999 | connected | - |
| Production | 10.11.30.3:11111 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65103
   router-id 10.11.30.3
   no bgp default ipv4-unicast
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor 10.101.30.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.101.30.1 remote-as 65100
   neighbor 10.101.30.1 description SW101-SITE1-B_Loopback0
   neighbor 10.102.30.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.102.30.1 remote-as 65200
   neighbor 10.102.30.1 description SW102-SITE1-R_Loopback0
   !
   vlan 110
      rd 10.11.30.3:10110
      route-target both 10110:10110
      redistribute learned
   !
   vlan 112
      rd 10.11.30.3:10112
      route-target both 10112:10112
      redistribute learned
   !
   vlan 113
      rd 10.11.30.3:10113
      route-target both 10113:10113
      redistribute learned
   !
   vlan 120
      rd 10.11.30.3:10120
      route-target both 10120:10120
      redistribute learned
   !
   vlan 135
      rd 10.11.30.3:10135
      route-target both 10135:10135
      redistribute learned
   !
   vlan 136
      rd 10.11.30.3:10136
      route-target both 10136:10136
      redistribute learned
   !
   vlan 140
      rd 10.11.30.3:10140
      route-target both 10140:10140
      redistribute learned
   !
   vlan 150
      rd 10.11.30.3:10150
      route-target both 10150:10150
      redistribute learned
   !
   vlan 184
      rd 10.11.30.3:10184
      route-target both 10184:10184
      redistribute learned
   !
   vlan 999
      rd 10.11.30.3:10999
      route-target both 10999:10999
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
   !
   vrf MGMT
      rd 10.11.30.3:11999
      route-target import evpn 11999:11999
      route-target export evpn 11999:11999
      router-id 10.11.30.3
      redistribute connected
   !
   vrf Production
      rd 10.11.30.3:11111
      route-target import evpn 11111:11111
      route-target export evpn 11111:11111
      router-id 10.11.30.3
      redistribute connected
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## 802.1X Port Security

### 802.1X Summary

#### 802.1X Global

| System Auth Control | Protocol LLDP Bypass | Dynamic Authorization | Dropped Packets Statistics |
| ------------------- | -------------------- | ----------------------| -------------------------- |
| True | True | True | - |

#### Dot1x Configuration

```eos
!
dot1x system-auth-control
dot1x protocol lldp bypass
dot1x dynamic-authorization
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | enabled |
| Production | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance Production
```

## IP DHCP Snooping

IP DHCP Snooping is enabled

### IP DHCP Snooping Device Configuration

```eos
!
ip dhcp snooping
```

## Errdisable

### Errdisable Summary

Errdisable recovery timer interval: 30 seconds

|  Cause | Detection Enabled | Recovery Enabled |
| ------ | ----------------- | ---------------- |
| bpduguard | - | True |
| link-flap | - | True |

```eos
!
errdisable recovery cause bpduguard
errdisable recovery cause link-flap
errdisable recovery interval 30
```
