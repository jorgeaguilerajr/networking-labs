# Small Office VLAN Segmentation Lab

## 📖 Overview
This lab simulates a small business office network with three departments: HR, Sales, and IT.  
It demonstrates:
- VLAN segmentation for traffic isolation
- Inter-VLAN routing using Router-on-a-Stick (Cisco 2811)
- Centralized DHCP pools per VLAN
- ACLs to enforce access control policies
- Security best practices for switch and router management

---

## 🖥️ Topology
![Topology Diagram](topology.png)

---

## 🌐 IP Scheme
| VLAN | Department | Subnet           | Gateway         | DHCP Pool Range             |
|------|------------|------------------|-----------------|-----------------------------|
| 10   | HR         | 192.168.10.0/24 | 192.168.10.1    | 192.168.10.2 – 192.168.10.100 |
| 20   | Sales      | 192.168.20.0/24 | 192.168.20.1    | 192.168.20.2 – 192.168.20.100 |
| 30   | IT         | 192.168.30.0/24 | 192.168.30.1    | 192.168.30.2 – 192.168.30.100 |
| 99   | Mgmt       | 192.168.99.0/24 | 192.168.99.1    | N/A                         |

---

## 🔑 Key Configurations
- **Router (2811)**: Sub-interfaces with 802.1Q encapsulation for each VLAN.  
- **DHCP**: Pools configured on the router for automatic IP assignments.  
- **Switch (2960)**: VLAN creation, access ports assigned, and a trunk to the router.  
- **ACLs**: Sales VLAN blocked from initiating traffic to IT VLAN, while IT retains access to Sales for support.  

*(See full configs in the [configs/](./configs) folder)*  

---

## ✅ Verification
- PCs received IPs via DHCP (`show ip dhcp binding` on router).  
- **Ping Tests**:
  - HR → IT ✅
  - HR → Sales ✅
  - IT → Sales ✅ (RDP allowed)
  - Sales → IT ❌ (blocked by ACL)
- ACL counters incremented for denied packets (`show access-lists`).  

*(See screenshots in the [verification/](./verification) folder)*  

---

## 🔒 Security Best Practices
- Removed default VLAN 1 and created a dedicated management VLAN (99).  
- Placed unused ports in a blackhole VLAN.  
- Configured SSH-only management access on network devices.  
- Implemented ACLs to enforce least-privilege between departments.  

---

## 📘 Reflection
This project gave me practical experience with VLAN segmentation, router-on-a-stick inter-VLAN routing, and enforcing policies with ACLs.  

The biggest challenge was learning how ACLs interact with return traffic — since IOS ACLs are stateless, blocking Sales → IT traffic also blocked ICMP replies until I refined the rules. This taught me the importance of designing ACLs carefully around business requirements.  

Overall, this lab represents a real-world small office design where HR, Sales, and IT need to be segmented for security but still require controlled communication paths. It improved my confidence in both Cisco CLI configuration and in documenting network designs for others to follow.  
