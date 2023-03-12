# Gotchas
Surprise! (p. 316 for example)

# NAT
- Up to two NAT rules can be applied, pp. 397-424
- Source port pools and GNAT, p. 406
- NAT pool and exhaustion, pp. 406-408
- NAT pools when you don't want to create a network object; see gateway Web GUI, p.421
- cpview and NAT pool utility, p. 407
# Domain Objects
- Correct way, pp. 387-389
    - tool on gateway to verify
    - No wildcard
    - without FQDN checked it will rely on reverse DNS lookups
# Proxy ARP
- `fw ctl arp -n` on the gateway, p. 416
- when not required, p. 419
# DHCP Relay
- Gaia configuration and example rules, pp. 430-431
# Identify Awareness
- AD Query for Identity Awareness is deprecated due to security issue CVE-2021-26414; use Identity collector instead, p. 453
# VPN
- Excluding satellite's gateway from the VPN, pp. 481-483
- Tunnel status: Logs & Monitor, then click Tunnel and User monitoring. p. 286
# Performance
## Drop Templates
- Drop templates - "don't use them" - p. 319
# Backups
- Properly restoring from a saved Gaia configuration file, p. 227
- A cheeky way to download a gateway backup configuration from the Web GUI without using SCP, pp. 225-226
# Management Server
- How to tell if the managment server is back up after a reboot, p. 235
    - `$MDS_FWDIR/scripts/check_cpm_status.sh` works in both MDS/MDSM and SMS
# CPUSE
- Offline CPUSE, pp. 236-238
# ClusterXL
- VMAC pp. 541, 542
    - assigns identical MAC addresses to all cluster virtual interfaces
    - "VMAC that is advertised by the cluster members (via Gratuitous ARP Requests) keeps the real MAC Address of each member and adds another Virtual MAC address on top of it."
    - When a failover occurs, the G-ARP packet is the only packet sourced from the VMAC Address. This alerts the connected switch about the (physical or virtual) port that the new active member is connectewd to via the MAC learning process, by updating the CAM tables
    - Faster and less prone to issues than some L3 swiches or routers might experience with traditional ARP Table updates, especially if you have a lot of statically NATed hosts
    - Cannot use this feature in the VirtualBox lab; majority of level 2 hypervisors do not have fully functioning virtual switches; Level 1 (bare metal) hypervisor like VMWare ESXi or Microsoft Hyper-V will work with VMAC
- Configuring cluster addresses on different subnets p. 543
    - generally a virtual IP and the corresponding IPs of addresses assigned to the cluster member interface should belong to the same network
    - there is a provision to use a virtual IP  address from a different network
        - might have only a single public IP address but still want a cluster; see sk32073
        - CAVEAT only applicable to either locally managed clusters or remote clusters that are not managed over the Internet (i.e., you have WAN connectivity)
- in most modern physical implementations, cluster interfaces are implemented as VLANs on bonded interfaces
- the parent interface (bond or standalone) containing VLAN sub-interfaces should NOT have an IP address assigned; VLAN ID 1 CANNOT be defined as a sub-interface
    - the cluster interfaces configured on individual NIC ports are all monitored
    - of the VLAN cluster interfaces residing on the same parent port or bond, only those with the lowest and highest VLAN IDs are monitored by default; you can change this configuraiton to monitor all the VLANs if necessary
- Dynamic routing synchronization p. 547
    - When a cluster member comes up, a Funn Sync takes place, and if dynamic routing is used Forwarding Information Base (FIB) manager ensures OSPF and BGP routes are propagated to the standby members
    - `cphaprob routedifcs` - see whether a cluster member has dynamic routing confiugred and is associated with its interfaces
- Sync interface p. 545-546, 548 (sync link usage)
- Cluster edge cases p. 552-553
- `fw hastat1 pp. 554-555
# Troubleshooting
## Packet captures
- https://community.checkpoint.com/fyrhh23835/attachments/fyhrhh23835/member-exclusives/484/2/CPX_Preso_TimHall_FINAL.pdf p582
    - tcpdump, fwmonitor, cppcap comparisons and IMPORTANT recommendations
- Pages 582-583 give the link to tcpdump101.com which can help you write your packet captures
    - tcpdump101 can be downloaded and run in the browser of your choice
## fw ctl zdebug drop and fw ctl zdebug + drop
- Page 582 explains `fw ctl zdebug + drop` is "the extended version"
- Pages 582-583 give the link to tcpdump101.com which can help you write your kernel debug
    - tcpdump101 can be downloaded and run in the browser of your choice
