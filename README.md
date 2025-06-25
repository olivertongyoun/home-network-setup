#  Home Network Setup and Optimization

Designed and configured a secure home network using a Cisco router and switch. Implemented VLANs, configured DHCP and static IPs, and enabled wireless security protocols.

---

##  Overview

- Simulated a home network using Cisco Packet Tracer
- Segmented traffic using VLANs (Admin, Guest, IoT)
- Enabled inter-VLAN routing using Router-on-a-Stick
- Configured WRT300N wireless router for IoT WiFi access

---

##  Network Topology Diagram

![Network Diagram](home-network.png)

> Basic layout includes: Cisco router, switch, admin PC, guest PC, WRT300N wireless router, and IoT devices

---

## Device Summary

| Device       | Role                | IP Address          | Notes                                   |
|--------------|---------------------|---------------------|------------------------------------------|
| Router       | Gateway & Routing   | Subinterfaces used  | Inter-VLAN routing (g0/0.10, .20, .30)   |
| Switch SW1   | VLAN-enabled Switch | N/A                 | VLAN tagging & access port configuration |
| PC1          | Admin PC            | 192.168.10.2        | VLAN 10, static or DHCP                  |
| PC2          | Guest PC            | 192.168.20.2        | VLAN 20, DHCP assigned                   |
| WRT300N      | Wireless Gateway    | 192.168.30.2 (WAN)  | VLAN 30 uplink, provides WiFi/NAT        |
| Laptop       | IoT Device          | 192.168.20.x        | Connected to WRT300N via WiFi            |
| Smartphone   | IoT Device          | 192.168.20.x        | Connected to WRT300N via WiFi            |

---

##  VLAN & Subnet Details

| VLAN | Purpose         | IP Range         | Gateway IP       |
|------|------------------|------------------|------------------|
| 10   | Admin Devices    | 192.168.10.0/24  | 192.168.10.1     |
| 20   | Guest Devices    | 192.168.20.0/24  | 192.168.20.1     |
| 30   | IoT Devices      | 192.168.30.0/24  | 192.168.30.1     |

---

##  Router Configuration (Sample CLI)

enable
configure terminal

interface g0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
exit

interface g0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
exit

interface g0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
exit

interface g0/0
 no shutdown

ip dhcp pool VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1

ip dhcp pool VLAN20
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1

ip dhcp pool VLAN30
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1

## Switch Configuration (Sample CLI)
enable
configure terminal

vlan 10
 name Admin
exit

vlan 20
 name Guest
exit

vlan 30
 name IoT
exit

interface range fa0/1 - 2
 switchport mode access
 switchport access vlan 10
exit

interface fa0/3
 switchport mode access
 switchport access vlan 20
exit

interface fa0/4
 switchport mode access
 switchport access vlan 30
exit

interface g0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
exit

## WRT300N Wireless Router Setup 
WAN Port (to VLAN 30):

Static IP: 192.168.30.2

Gateway: 192.168.30.1

LAN IP: 192.168.20.1

DHCP Server: Enabled

Range: 192.168.20.100 – 192.168.20.110

Wireless SSID: IoT-WiFi

Security: WPA2

Passphrase: SecureIoT2025

## Troubleshooting & Validation 
PC1> ping 192.168.10.1
Reply from 192.168.10.1: bytes=32 time<1ms TTL=255

PC2> ping 192.168.20.1
Reply from 192.168.20.1: bytes=32 time<1ms TTL=255
VLANs successfully separated

Inter-VLAN routing confirmed

Wireless devices assigned IP via WRT300N

DHCP working across all segments

## Security Measures
VLAN segmentation for Admin, Guest, and IoT traffic

NAT + DHCP isolated to WRT300N for IoT

WPA2 encryption on wireless network

Unused switch ports administratively shut down

## Future Improvements
Add ACLs to block IoT-to-Admin communication

Implement syslog or SNMP for monitoring

Simulate external Internet via NAT overload

Try replacing WRT300N with Layer 3 switch

## What I Learned
VLAN segmentation enhances both security and performance

Router-on-a-stick is a practical method for inter-VLAN routing

Wireless setup adds realism and complexity to home lab projects

CLI strengthens Layer 2/3 configuration skills

