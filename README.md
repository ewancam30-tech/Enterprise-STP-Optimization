# Enterprise STP Optimization Project

## Spanning Tree Protocol Design, Root Bridge Control, Loop Prevention, Verification, and Troubleshooting

**Enterprise Network Engineering Portfolio: Project 5**

---

## Enterprise Cover

| **Field** | **Details** |
|-----------|-------------|
| **Project Name** | Enterprise STP Optimization Project |
| **Portfolio Track** | Enterprise Network Engineering |
| **Project Number** | Project 5 |
| **Primary Technology** | Spanning Tree Protocol, Rapid PVST+, Root Bridge Optimization |
| **Platform** | Cisco Packet Tracer |
| **Prepared By** | Ewan D. Campbell |
| **Business Name** | Campbell Technologies |
| **Project Type** | Layer 2 Redundancy, Loop Prevention, and Enterprise Switching Optimization |
| **Status** | Completed |
| **Previous Project** | Project 4: Enterprise Inter-VLAN Routing |
| **Next Project** | Project 6: Enterprise ACL Security Policy Implementation |

---

## Executive Summary

This project documents the design, implementation, verification, and troubleshooting of **Spanning Tree Protocol optimization** in an enterprise switching environment. The network builds directly upon the routed VLAN environment created in **Project 4**, where inter-VLAN routing was implemented using Router-on-a-Stick.

After VLAN segmentation and routing were completed, the next engineering requirement was to improve Layer 2 resiliency while preventing switching loops. To accomplish this, the network was enhanced with redundant switch-to-switch trunk links and optimized using **Rapid PVST+**, root bridge control, PortFast, BPDU Guard, and root protection concepts.

### What Was Achieved

This project introduced enterprise Layer 2 redundancy while maintaining network stability. The implementation ensures that:

1. Redundant trunk links exist between switches.
2. Spanning Tree prevents Layer 2 loops.
3. SW1 is intentionally configured as the STP root bridge.
4. SW2 acts as the secondary root bridge if SW1 fails.
5. Access ports use PortFast for faster endpoint connectivity.
6. BPDU Guard protects access ports from unauthorized switches.
7. SW2 F0/23 remains dedicated to Printer0 as an access port in VLAN 50.
8. Verification commands confirm STP state, root bridge placement, blocked ports, trunk status, and connectivity.

### Why This Matters

In real enterprise networks, redundancy is required for availability. However, redundant Layer 2 links can create switching loops if they are not controlled properly. Switching loops can cause broadcast storms, MAC address table instability, duplicate frames, and network outages.

This project demonstrates the ability to design a stable Layer 2 topology that supports redundancy without sacrificing control. It also shows the engineering judgment required to move beyond basic switching and into production-style network design.

---

## Key Takeaways

### Technical Concepts

| **Concept** | **Explanation** |
|------------|-----------------|
| **Spanning Tree Protocol** | Prevents Layer 2 switching loops by placing redundant paths into blocking or alternate states. |
| **Rapid PVST+** | Cisco per-VLAN rapid STP implementation that provides faster convergence than traditional STP. |
| **Root Bridge** | The central reference point STP uses to calculate the loop-free topology. |
| **Bridge Priority** | The value used to influence which switch becomes the STP root bridge. |
| **Root Port** | The switch port with the best path toward the root bridge. |
| **Designated Port** | The forwarding port selected for a network segment. |
| **Alternate Port** | A backup port that remains blocked until the active path fails. |
| **PortFast** | Allows access ports to move directly to forwarding state for end devices. |
| **BPDU Guard** | Shuts down an access port if it receives a BPDU, protecting against unauthorized switch connections. |
| **Root Guard** | Prevents an unexpected switch from becoming the root bridge on protected ports. |

---

## Engineering Change Request

| **Field** | **Details** |
|-----------|-------------|
| **Change ID** | ECR-NET-005 |
| **Change Title** | Implement Enterprise STP Optimization and Layer 2 Redundancy |
| **Requested By** | Network Engineering |
| **Business Driver** | Improve Layer 2 resiliency, prevent switching loops, and optimize enterprise switch convergence. |
| **Change Type** | Planned network configuration change |
| **Affected Systems** | SW1, SW2, switch trunks, access ports, Printer0 access port, VLAN forwarding paths |
| **Risk Level** | Medium. Incorrect STP configuration may cause loops, blocked connectivity, root bridge instability, or access port shutdowns. |
| **Implementation Window** | Approved maintenance window or after business hours |
| **Rollback Plan** | Remove redundant trunk configuration from F0/22, restore previous spanning tree settings, keep SW2 F0/23 as the Printer0 access port, and return switch ports to previous configuration. |
| **Success Criteria** | SW1 is confirmed as root bridge, SW2 is confirmed as secondary root, F0/24 and F0/22 trunks are operational, SW2 F0/23 remains in VLAN 50 for Printer0, STP blocks the appropriate path, and end-to-end connectivity remains successful. |

---

## Business Scenario

### The Problem

Campbell Technologies previously completed VLAN segmentation and inter-VLAN routing. The environment now supports departmental separation and controlled routing between VLANs.

However, the switching topology still has a resiliency concern. If the single trunk link between SW1 and SW2 fails, devices connected to SW2 may lose access to the router, shared printer, management network, and other VLANs.

The business requires improved availability between switches without introducing Layer 2 instability.

### Current Limitation

| **Issue** | **Business Impact** |
|----------|---------------------|
| Single inter-switch trunk | Failure may isolate users connected to SW2. |
| Default STP behavior | Root bridge selection may be unpredictable. |
| No intentional STP tuning | Traffic paths may not align with the enterprise design. |
| No access port STP protection | Unauthorized switches could create loops. |
| Printer port already assigned to SW2 F0/23 | Redundant trunk design must avoid using the printer port. |

### Required Solution

The network must support redundant Layer 2 paths while preventing switching loops. Because **SW2 F0/23 is already assigned to Printer0**, the redundant trunk is placed on **F0/22** instead of F0/23.

---

## Business Impact

| **Area** | **Impact** |
|---------|------------|
| **Availability** | Redundant trunk links reduce the risk of switch-to-switch isolation. |
| **Stability** | STP prevents Layer 2 loops that could disrupt the entire network. |
| **Security** | BPDU Guard protects access ports from unauthorized switch connections. |
| **Performance** | PortFast improves endpoint connection time. |
| **Operations** | Root bridge control makes the switching topology predictable. |
| **Printer Availability** | Printer0 remains on SW2 F0/23 in VLAN 50 and is not disrupted by the redundant trunk design. |

---

## Project Objectives

1. Add a redundant trunk link between SW1 and SW2 using F0/22.
2. Preserve SW2 F0/23 as the Printer0 access port in VLAN 50.
3. Configure Rapid PVST+ for faster Layer 2 convergence.
4. Configure SW1 as the primary STP root bridge.
5. Configure SW2 as the secondary STP root bridge.
6. Verify STP root bridge placement.
7. Verify forwarding and alternate port roles.
8. Enable PortFast on endpoint access ports.
9. Enable BPDU Guard on endpoint access ports.
10. Confirm trunk VLAN consistency.
11. Test connectivity before and after STP changes.
12. Simulate link failure and confirm STP convergence.
13. Document commands, verification results, evidence, and troubleshooting scenarios.

---

## Implementation Timeline

| **Phase** | **Task** | **Purpose** |
|----------|----------|-------------|
| Phase 1 | Review existing VLAN and routing design | Confirm Project 4 baseline is operational. |
| Phase 2 | Confirm SW2 F0/23 printer assignment | Preserve the printer access port. |
| Phase 3 | Add redundant trunk link on F0/22 | Improve switch-to-switch resiliency. |
| Phase 4 | Configure Rapid PVST+ | Improve STP convergence speed. |
| Phase 5 | Configure root bridge priority | Make SW1 the intentional STP root. |
| Phase 6 | Configure secondary root | Allow SW2 to become root if SW1 fails. |
| Phase 7 | Configure PortFast | Improve access port startup behavior. |
| Phase 8 | Configure BPDU Guard | Protect access ports from unauthorized switches. |
| Phase 9 | Verify STP topology | Confirm root bridge, port roles, and blocked ports. |
| Phase 10 | Test connectivity | Confirm user and printer traffic still works. |
| Phase 11 | Simulate failure | Confirm redundant path becomes active. |
| Phase 12 | Document evidence | Capture screenshots and command outputs. |

---

## Network Architecture

### Core Design

The architecture uses the same enterprise topology from Project 4, with the addition of a second trunk link between SW1 and SW2. Since **SW2 F0/23 is used for Printer0**, the redundant trunk uses **F0/22**.

| **Component** | **Architecture Role** |
|--------------|------------------------|
| **Router1** | Provides inter-VLAN routing using Router-on-a-Stick. |
| **SW1** | Primary distribution switch and STP root bridge. |
| **SW2** | Access switch and STP secondary root bridge. |
| **F0/24 Trunk** | Primary switch-to-switch trunk. |
| **F0/22 Trunk** | Redundant switch-to-switch trunk. |
| **SW2 F0/23** | Printer0 access port in VLAN 50. |
| **Rapid PVST+** | Controls loop-free forwarding per VLAN. |

---

## Architecture Overview

### Network Topology Diagram
---
<img width="661" height="299" alt="Screenshot 2026-07-06 174208" src="https://github.com/user-attachments/assets/1f2d56cc-c686-4f89-89a7-4ae970676f58" />

### Logical Topology Summary

| **Connection** | **From** | **To** | **Type** | **Purpose** |
|---------------|----------|--------|----------|-------------|
| Router to SW1 | Router1 Gi0/0 | SW1 Gi0/1 | 802.1Q Trunk | Carries routed VLAN traffic to the router. |
| SW1 to SW2 | SW1 F0/24 | SW2 F0/24 | 802.1Q Trunk | Primary inter-switch trunk. |
| SW1 to SW2 | SW1 F0/22 | SW2 F0/22 | 802.1Q Trunk | Redundant inter-switch trunk. |
| Printer | Printer0 | SW2 F0/23 | Access | Printer access port in VLAN 50. |

### STP Design Summary

| **Switch** | **STP Role** | **Reason** |
|-----------|--------------|------------|
| SW1 | Primary Root Bridge | Closest switch to Router1 and VLAN gateways. |
| SW2 | Secondary Root Bridge | Backup root if SW1 fails. |

---

## Enterprise Network Topology Analysis

### Existing Design Before Project 5

Before STP optimization, the network contained:

1. One router connected to SW1.
2. Two Cisco switches.
3. Multiple VLANs.
4. One trunk between SW1 and SW2.
5. Inter-VLAN routing through Router-on-a-Stick.
6. Static endpoint IP addressing.
7. VLAN 999 reserved as the native parking VLAN.
8. Printer0 connected to SW2 F0/23 in VLAN 50.

### Enhanced Design After Project 5

After this project, the network contains:

1. Two trunk links between SW1 and SW2.
2. Primary trunk on F0/24.
3. Redundant trunk on F0/22.
4. Printer0 preserved on SW2 F0/23 as a VLAN 50 access device.
5. Rapid PVST+ enabled.
6. SW1 configured as root bridge.
7. SW2 configured as secondary root bridge.
8. One redundant path controlled by STP.
9. PortFast enabled on access ports.
10. BPDU Guard enabled on access ports.

---

## Design Rationale

### Why F0/22 Was Used for the Redundant Trunk

SW2 F0/23 is already used by Printer0. Reassigning F0/23 as a trunk would remove the printer from VLAN 50 and disrupt printer access. Therefore, F0/22 was selected as the redundant trunk port.

| **Decision** | **Rationale** |
|-------------|---------------|
| Keep SW2 F0/23 as printer port | Preserves existing printer connectivity. |
| Use SW1 F0/22 to SW2 F0/22 as redundant trunk | Adds redundancy without disrupting the printer. |
| Keep SW1 F0/24 to SW2 F0/24 as primary trunk | Maintains the existing primary inter-switch path. |
| Use Rapid PVST+ | Provides faster STP convergence. |
| Make SW1 root bridge | Keeps Layer 2 forwarding aligned with the router location. |

### Why SW1 Is the Root Bridge

SW1 is connected directly to Router1, which provides the default gateways for all VLANs. Because SW1 is closest to the Layer 3 boundary, it should control the Layer 2 topology.

### Why SW2 Is the Secondary Root

SW2 is configured as the secondary root so that if SW1 fails, SW2 can become the root bridge in a predictable way.

---

## Engineering Decisions

| **Engineering Decision** | **Design Rationale** |
|--------------------------|----------------------|
| Use Rapid PVST+ | Improves convergence and supports per-VLAN STP behavior. |
| Configure SW1 as root bridge | Places the root bridge near the router and default gateways. |
| Configure SW2 as secondary root | Provides predictable backup root behavior. |
| Add redundant trunk on F0/22 | Improves availability without disturbing Printer0 on SW2 F0/23. |
| Keep Printer0 on SW2 F0/23 | Preserves the existing printer VLAN design. |
| Allow only required VLANs on trunks | Reduces unnecessary VLAN exposure and broadcast propagation. |
| Use VLAN 999 as native parking VLAN | Prevents untagged user traffic from using a production VLAN. |
| Enable PortFast on access ports | Reduces endpoint connection delay. |
| Enable BPDU Guard on access ports | Protects against unauthorized switch connections. |
| Do not enable PortFast on trunks | Prevents unsafe forwarding behavior on switch-to-switch links. |

---

## Implementation Risks

| **Risk** | **Impact** | **Mitigation** |
|---------|------------|----------------|
| Accidentally using SW2 F0/23 as trunk | Printer0 loses VLAN 50 access. | Keep F0/23 as an access port and use F0/22 for the redundant trunk. |
| Incorrect root bridge | Traffic may take an inefficient path. | Configure root bridge priority manually. |
| Trunk VLAN mismatch | Some VLANs may fail across switches. | Verify allowed VLAN list on all trunks. |
| Native VLAN mismatch | Untagged traffic may be handled incorrectly. | Use VLAN 999 consistently. |
| PortFast on trunk | Possible loop exposure. | Apply PortFast only to endpoint access ports. |
| BPDU Guard on wrong port | Trunk may shut down if BPDUs are received. | Apply BPDU Guard only to access ports. |
| Redundant link without STP | Layer 2 loop may occur. | Confirm STP is enabled before adding redundancy. |

---

## Equipment

| **Equipment** | **Qty** | **Purpose** |
|--------------|---------|-------------|
| Cisco 1941 or Cisco 2111 Router | 1 | Provides Router-on-a-Stick inter-VLAN routing. |
| Cisco Catalyst 2960 Switch | 2 | Provides Layer 2 switching, VLANs, trunks, and STP. |
| End User PCs | 10 | Test hosts across departmental VLANs. |
| Network Printer | 1 | Shared service device in printer VLAN. |
| Copper Ethernet Cables | As needed | Connect endpoints, router, and switch trunks. |
| Console Access | 1 | Configure and verify Cisco IOS devices. |
| Cisco Packet Tracer | 1 | Lab implementation and verification platform. |

---

## IP Addressing Plan

| **VLAN** | **Name** | **Network** | **Gateway** | **Device** | **IP Address** |
|---------|----------|-------------|-------------|------------|----------------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 | PC1 | 192.168.10.10 |
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 | PC2 | 192.168.10.11 |
| 20 | Finance | 192.168.20.0/24 | 192.168.20.1 | PC3 | 192.168.20.10 |
| 20 | Finance | 192.168.20.0/24 | 192.168.20.1 | PC4 | 192.168.20.11 |
| 30 | Sales | 192.168.30.0/24 | 192.168.30.1 | PC7 | 192.168.30.10 |
| 30 | Sales | 192.168.30.0/24 | 192.168.30.1 | PC8 | 192.168.30.11 |
| 40 | IT | 192.168.40.0/24 | 192.168.40.1 | PC5 | 192.168.40.10 |
| 40 | IT | 192.168.40.0/24 | 192.168.40.1 | PC6 | 192.168.40.11 |
| 50 | Printer | 192.168.50.0/24 | 192.168.50.1 | Printer0 | 192.168.50.10 |
| 60 | Guest | 192.168.60.0/24 | 192.168.60.1 | PC9 | 192.168.60.10 |
| 60 | Guest | 192.168.60.0/24 | 192.168.60.1 | PC10 | 192.168.60.11 |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | SW1 | 192.168.99.11 |
| 99 | Management | 192.168.99.0/24 | 192.168.99.1 | SW2 | 192.168.99.12 |
| 999 | Native Parking | No user subnet | Not assigned | N/A | N/A |

---

## Technology Stack

| **Technology** | **Role in Project** |
|----------------|---------------------|
| Cisco IOS | Switch and router configuration, verification, and troubleshooting. |
| VLANs | Logical segmentation for departments and services. |
| 802.1Q Trunking | Carries multiple VLANs across switch-to-switch and router-to-switch links. |
| Rapid PVST+ | Provides fast per-VLAN loop prevention. |
| STP Root Bridge Control | Ensures predictable Layer 2 topology. |
| PortFast | Speeds up endpoint access port forwarding. |
| BPDU Guard | Protects access ports from unauthorized switches. |
| Packet Tracer | Provides simulation environment for implementation and testing. |

---

## Skills Demonstrated

1. Configured Rapid PVST+ on Cisco switches.
2. Controlled STP root bridge election.
3. Configured primary and secondary root bridges.
4. Added redundant trunk links safely.
5. Preserved existing printer access-port design.
6. Verified STP port roles and states.
7. Configured PortFast on access ports.
8. Configured BPDU Guard on endpoint ports.
9. Verified trunk VLAN consistency.
10. Simulated trunk failure and observed STP convergence.
11. Troubleshot Layer 2 issues using Cisco IOS commands.

---

## Project Continuity

| **Phase** | **Project** | **Status** |
|----------|-------------|------------|
| Project 1 | Basic Enterprise LAN Foundation | Completed |
| Project 2 | Switch Configuration and Endpoint Connectivity | Completed |
| Project 3 | Enterprise VLAN Segmentation | Completed |
| Project 4 | Enterprise Inter-VLAN Routing | Completed |
| Project 5 | Enterprise STP Optimization | Completed |
| Project 6 | Enterprise ACL Security Policy Implementation | Planned |

---

## Port Assignment Summary

### SW1 Port Assignments

| **Port** | **Role** | **VLAN/Trunk** | **Connected Device** | **Purpose** |
|---------|----------|----------------|----------------------|-------------|
| F0/1 | Access | VLAN 10 | PC1 | HR Workstation |
| F0/2 | Access | VLAN 10 | PC2 | HR Workstation |
| F0/3 | Access | VLAN 20 | PC3 | Finance Workstation |
| F0/4 | Access | VLAN 20 | PC4 | Finance Workstation |
| F0/5 | Access | VLAN 40 | PC5 | IT Workstation |
| F0/6 | Access | VLAN 40 | PC6 | IT Workstation |
| Gi0/1 | Trunk | 10,20,30,40,50,60,99,999 | Router1 Gi0/0 | Router-on-a-Stick uplink |
| F0/24 | Trunk | 10,20,30,40,50,60,99,999 | SW2 F0/24 | Primary inter-switch trunk |
| F0/22 | Trunk | 10,20,30,40,50,60,99,999 | SW2 F0/22 | Redundant inter-switch trunk |

### SW2 Port Assignments

| **Port** | **Role** | **VLAN/Trunk** | **Connected Device** | **Purpose** |
|---------|----------|----------------|----------------------|-------------|
| F0/1 | Access | VLAN 30 | PC7 | Sales Workstation |
| F0/2 | Access | VLAN 30 | PC8 | Sales Workstation |
| F0/3 | Access | VLAN 60 | PC9 | Guest Workstation |
| F0/4 | Access | VLAN 60 | PC10 | Guest Workstation |
| F0/23 | Access | VLAN 50 | Printer0 | Network Printer |
| F0/24 | Trunk | 10,20,30,40,50,60,99,999 | SW1 F0/24 | Primary inter-switch trunk |
| F0/22 | Trunk | 10,20,30,40,50,60,99,999 | SW1 F0/22 | Redundant inter-switch trunk |

---

## Implementation Summary

### Implementation Sequence

1. Verified the Project 4 baseline.
2. Confirmed existing VLANs and trunk links.
3. Confirmed SW2 F0/23 is assigned to Printer0 in VLAN 50.
4. Added a second trunk link between SW1 F0/22 and SW2 F0/22.
5. Enabled Rapid PVST+ on both switches.
6. Configured SW1 as the primary root bridge.
7. Configured SW2 as the secondary root bridge.
8. Verified root bridge placement.
9. Verified forwarding and alternate port states.
10. Configured PortFast on access ports.
11. Configured BPDU Guard on access ports.
12. Verified trunk allowed VLANs.
13. Tested same-VLAN and cross-VLAN connectivity.
14. Tested Printer0 connectivity in VLAN 50.
15. Simulated trunk failure.
16. Confirmed STP recovery through the redundant path.
17. Captured screenshots and documented troubleshooting cases.

---

## Configuration Walkthrough

### Step 1: Verify Existing VLANs on SW1 and SW2

```text
show vlan brief
show interfaces trunk
show spanning-tree summary
```

---

### Step 2: Configure Rapid PVST+ on SW1

```text
enable
configure terminal
spanning-tree mode rapid-pvst
end
write memory
```

### Step 3: Configure Rapid PVST+ on SW2

```text
enable
configure terminal
spanning-tree mode rapid-pvst
end
write memory
```

---

### Step 4: Configure SW1 as the Primary Root Bridge

```text
enable
configure terminal
spanning-tree vlan 10,20,30,40,50,60,99,999 root primary
end
write memory
```

---

### Step 5: Configure SW2 as the Secondary Root Bridge

```text
enable
configure terminal
spanning-tree vlan 10,20,30,40,50,60,99,999 root secondary
end
write memory
```

---

### Step 6: Configure SW1 Trunk Ports

```text
enable
configure terminal

interface fastEthernet0/24
 description PRIMARY_TRUNK_TO_SW2_F0_24
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,99,999
 no shutdown

interface fastEthernet0/22
 description REDUNDANT_TRUNK_TO_SW2_F0_22
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,99,999
 no shutdown

end
write memory
```

---

### Step 7: Configure SW2 Trunk Ports

```text
enable
configure terminal

interface fastEthernet0/24
 description PRIMARY_TRUNK_TO_SW1_F0_24
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,99,999
 no shutdown

interface fastEthernet0/22
 description REDUNDANT_TRUNK_TO_SW1_F0_22
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,99,999
 no shutdown

end
write memory
```

---

### Step 8: Configure SW2 Printer Port

```text
enable
configure terminal

interface fastEthernet0/23
 description PRINTER0_ACCESS_PORT
 switchport mode access
 switchport access vlan 50
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown

end
write memory
```

### Why This Matters

SW2 F0/23 must remain an access port. If it is configured as a trunk, Printer0 will lose its correct VLAN 50 assignment.

---

### Step 9: Configure SW1 Access Ports with PortFast and BPDU Guard

```text
enable
configure terminal

interface range fastEthernet0/1 - 6
 description ENDPOINT_ACCESS_PORTS
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

end
write memory
```

---

### Step 10: Configure SW2 Access Ports with PortFast and BPDU Guard

```text
enable
configure terminal

interface range fastEthernet0/1 - 4
 description ENDPOINT_ACCESS_PORTS
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable

interface fastEthernet0/23
 description PRINTER0_ACCESS_PORT
 switchport mode access
 switchport access vlan 50
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown

end
write memory
```

---

## Verification Methodology

| **Layer** | **Verification Area** | **Validation Method** |
|----------|------------------------|-----------------------|
| Layer 1 | Physical links | Confirm F0/24 and F0/22 trunk links are connected and up. |
| Layer 2 | VLAN database | Confirm VLANs exist on SW1 and SW2. |
| Layer 2 | Trunks | Confirm F0/24 and F0/22 trunk mode, native VLAN, and allowed VLANs. |
| Layer 2 | Printer access port | Confirm SW2 F0/23 is an access port in VLAN 50. |
| Layer 2 | STP mode | Confirm Rapid PVST+ is enabled. |
| Layer 2 | Root bridge | Confirm SW1 is root for all required VLANs. |
| Layer 2 | Port roles | Confirm root, designated, and alternate ports. |
| Layer 2 | PortFast | Confirm access ports are optimized. |
| Layer 2 | BPDU Guard | Confirm access ports are protected. |
| Layer 3 | Connectivity | Confirm same-VLAN and cross-VLAN communication. |
| Resilience | Failure test | Disable one trunk and confirm backup path works. |

---

## Verification Commands

| **Command** | **Purpose** | **Expected Result** |
|------------|-------------|---------------------|
| `show spanning-tree summary` | Shows STP mode and enabled features. | Rapid PVST+ is enabled. |
| `show spanning-tree vlan 10` | Shows STP details for VLAN 10. | SW1 is root bridge. |
| `show spanning-tree root` | Displays root bridge per VLAN. | SW1 is listed as root for VLANs. |
| `show spanning-tree blockedports` | Shows blocked or alternate ports. | One redundant path is blocked or alternate. |
| `show interfaces trunk` | Verifies trunk status and allowed VLANs. | F0/24 and F0/22 are trunks. |
| `show vlan brief` | Verifies VLAN database and access ports. | SW2 F0/23 appears under VLAN 50. |
| `show running-config interface fastEthernet0/23` | Verifies printer port configuration. | F0/23 is access VLAN 50 with PortFast and BPDU Guard. |
| `show running-config | section spanning-tree` | Displays STP configuration. | Root settings, Rapid PVST+, and protections are present. |
| `show interfaces status` | Confirms interface state. | Access and trunk ports are connected or administratively expected. |
| `ping` | Tests connectivity. | Successful replies. |
| `traceroute` | Verifies routed path. | Traffic routes through router gateway. |

---

## Verification Command Explanations

### `show spanning-tree vlan 10`

Purpose: Displays detailed STP information for a specific VLAN.

Expected output on SW1:

```text
VLAN0010
  Spanning tree enabled protocol rstp
  Root ID    Priority    24586
             Address     xxxx.xxxx.xxxx
             This bridge is the root
```

Expected output on SW2:

```text
VLAN0010
  Root ID    Priority    24586
             Address     SW1_MAC_ADDRESS

Interface        Role Sts Cost Prio.Nbr Type
Fa0/24           Root FWD 19   128.24   P2p
Fa0/22           Altn BLK 19   128.22   P2p
```

Engineering significance: Confirms which switch is root and which redundant port is blocking.

---

### `show interfaces trunk`

Purpose: Confirms trunk status, native VLAN, and allowed VLANs.

Expected result:

```text
Port        Mode         Encapsulation  Status        Native vlan
Fa0/24      on           802.1q         trunking      999
Fa0/22      on           802.1q         trunking      999

Port        Vlans allowed on trunk
Fa0/24      10,20,30,40,50,60,99,999
Fa0/22      10,20,30,40,50,60,99,999
```

Engineering significance: Confirms the primary and redundant trunks are operational and consistent.

---

### `show vlan brief`

Purpose: Confirms VLAN database and access port membership.

Expected SW2 result:

```text
50   PRINTER    active    Fa0/23
```

Engineering significance: Confirms Printer0 is still assigned to VLAN 50 and was not accidentally converted into a trunk port.

---

## Implementation Evidence

| **Evidence** | **Command or Test** | **What It Proves** |
|-------------|---------------------|--------------------|
| STP Mode | `show spanning-tree summary` | Proves Rapid PVST+ is enabled. |
| Root Bridge SW1 | `show spanning-tree vlan 10` | Proves SW1 is root bridge. |
| Secondary Root SW2 | `show running-config | section spanning-tree` | Proves SW2 is configured as secondary root. |
| Blocked Port | `show spanning-tree blockedports` | Proves STP is preventing loops. |
| Trunk Status | `show interfaces trunk` | Proves F0/24 and F0/22 trunks are operational. |
| VLAN Database | `show vlan brief` | Proves VLANs exist and ports are assigned. |
| Printer Port | `show running-config interface fastEthernet0/23` | Proves SW2 F0/23 is the Printer0 access port. |
| PortFast | `show running-config interface` | Proves access ports use PortFast. |
| BPDU Guard | `show running-config interface` | Proves access ports use BPDU Guard. |
| Connectivity | `ping` | Proves network communication remains functional. |
| Failover Test | Disable primary trunk and retest ping | Proves redundant path works. |

---

## Screenshot Documentation

### Network Topology

<img width="710" height="311" alt="Screenshot 2026-07-06 170550" src="https://github.com/user-attachments/assets/a42da9d6-cb83-49c4-8cbb-09a98fda740f" />


Purpose: Shows Router1, SW1, SW2, VLANs, access devices, Printer0 on SW2 F0/23, and redundant trunk links on F0/24 and F0/22.

---

### STP Summary

<img width="288" height="184" alt="Screenshot 2026-07-06 182935" src="https://github.com/user-attachments/assets/7d6607cf-ddc6-4074-a566-72df14dde303" />


Command:

```text
show spanning-tree summary
```

Purpose: Confirms Rapid PVST+ is enabled.

---

### SW1 Root Bridge Verification and SW2 Root Port and Alternate Port

<img width="275" height="136" alt="Screenshot 2026-07-06 184456" src="https://github.com/user-attachments/assets/cdbb465d-fadf-443c-a420-36f48d614fc0" />


```text
show spanning-tree vlan 10
```

Purpose: Confirms SW1 is the root bridge.

Purpose: Shows SW2 port roles.

Verification: One trunk, such as F0/24, is forwarding as the root port. The redundant trunk, such as F0/22, is alternate or blocking.

Purpose: Displays STP blocked ports.

---

### Trunk Verification

<img width="253" height="134" alt="Screenshot 2026-07-06 191020" src="https://github.com/user-attachments/assets/a37761c9-c487-43a2-8e32-9a00f575bf21" />


Command:

```text
show interfaces trunk
```

Purpose: Confirms F0/24, F0/22, and G0/2 are operational trunk links.

---

### Printer Port Verification

<img width="275" height="345" alt="Screenshot 2026-07-06 192646" src="https://github.com/user-attachments/assets/7f944353-5c00-4f1b-a5a5-145d7cf018b2" />


Commands:

```text
show vlan brief
show running-config interface fastEthernet0/23
```

Purpose: Confirms SW2 F0/23 is an access port in VLAN 50 for Printer0.

---

### Connectivity Test

<img width="224" height="251" alt="Screenshot 2026-07-06 192848" src="https://github.com/user-attachments/assets/12f4ff82-37ff-4e11-82e0-eb258f9fc7ec" />

<img width="281" height="131" alt="Screenshot 2026-07-06 193606" src="https://github.com/user-attachments/assets/aaacd85e-7bbc-42de-a68a-fcb5db657e6d" />
Command:

```text
ping 192.168.50.10
```

Purpose: Confirms Printer0 remains reachable after STP changes from PC2 in Vlan 10(HR)

---

### Failover Test

```text
screenshots/failover-test.png
```

Commands:

<img width="281" height="131" alt="Screenshot 2026-07-06 193606" src="https://github.com/user-attachments/assets/772e6a68-43f8-4929-ae12-7e2b58fe4a45" />
<img width="209" height="94" alt="Screenshot 2026-07-06 193712" src="https://github.com/user-attachments/assets/e19d7c60-83a1-4658-9f5d-4924b8834cf8" />

show spanning-tree vlan 10
ping 192.168.50.10
```

Purpose: Confirms the backup trunk becomes active if the primary trunk fails and Printer0 remains reachable.

---

## Troubleshooting Case Studies

### Case Study 1: Wrong Switch Became Root Bridge

**Problem:** SW2 became the root bridge instead of SW1.

**Symptoms:**

1. `show spanning-tree root` showed SW2 as root.
2. Traffic path did not match the intended design.
3. SW1 did not display “This bridge is the root.”

**Root Cause:** SW1 was not configured with a lower bridge priority.

**Resolution:**

```text
spanning-tree vlan 10,20,30,40,50,60,99,999 root primary
```

**Lesson Learned:** Root bridge selection should be intentional. Do not rely on default STP election behavior.

---

### Case Study 2: Printer Port Accidentally Configured as a Trunk

**Problem:** Printer0 lost connectivity after F0/23 was accidentally configured as a trunk.

**Symptoms:**

1. Printer0 could not be pinged.
2. `show vlan brief` no longer showed F0/23 under VLAN 50.
3. `show interfaces trunk` showed F0/23 as trunking.

**Investigation:**

```text
show vlan brief
show interfaces trunk
show running-config interface fastEthernet0/23
```

**Root Cause:** SW2 F0/23 was reused for the redundant trunk even though it was already assigned to Printer0.

**Resolution:**

```text
interface fastEthernet0/23
 description PRINTER0_ACCESS_PORT
 switchport mode access
 switchport access vlan 50
 spanning-tree portfast
 spanning-tree bpduguard enable
 no shutdown

interface fastEthernet0/22
 description REDUNDANT_TRUNK_TO_SW1_F0_22
 switchport mode trunk
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,60,99,999
 no shutdown
```

**Verification:**

```text
show vlan brief
show interfaces trunk
ping 192.168.50.10
```

**Lesson Learned:** Always confirm existing port assignments before adding redundant links. Access ports and trunk ports serve different roles.

---

### Case Study 3: Redundant Trunk Created a Loop Risk

**Problem:** A second trunk was added before STP behavior was verified.

**Symptoms:**

1. MAC address table entries changed rapidly.
2. Network became unstable.
3. Ping results were inconsistent.

**Root Cause:** Redundant Layer 2 links were introduced without confirming STP control.

**Resolution:**

```text
spanning-tree mode rapid-pvst
show spanning-tree vlan 10
show spanning-tree blockedports
```

**Lesson Learned:** Always verify STP before relying on redundant Layer 2 links.

---

### Case Study 4: BPDU Guard Shut Down an Access Port

**Problem:** A user connected a small unmanaged switch to an access port.

**Symptoms:**

1. Endpoint lost connectivity.
2. Interface showed err-disabled.
3. Logs indicated BPDU Guard violation.

**Root Cause:** The access port received a BPDU, triggering BPDU Guard.

**Resolution:**

```text
interface fastEthernet0/1
 shutdown
 no shutdown
```

**Lesson Learned:** BPDU Guard protects the network by shutting down ports that receive unexpected BPDUs.

---

### Case Study 5: VLAN Missing from Redundant Trunk

**Problem:** Some VLANs worked across the trunk, but VLAN 30 failed.

**Symptoms:**

1. Sales PCs could not communicate across switches.
2. Other VLANs worked normally.
3. `show interfaces trunk` showed VLAN 30 missing.

**Resolution:**

```text
interface fastEthernet0/22
 switchport trunk allowed vlan add 30
```

**Lesson Learned:** When trunks are manually restricted, every required VLAN must be allowed on every trunk that should carry it.

---

### Case Study 6: Native VLAN Mismatch

**Problem:** STP and trunk warnings appeared after adding the redundant trunk.

**Symptoms:**

1. Native VLAN mismatch messages appeared.
2. Trunk behavior was inconsistent.
3. Untagged traffic was not handled consistently.

**Resolution:**

```text
interface fastEthernet0/22
 switchport trunk native vlan 999

interface fastEthernet0/24
 switchport trunk native vlan 999
```

**Lesson Learned:** Native VLAN settings must match on both sides of a trunk.

---

## Engineering Reflection

This project helped me understand why Spanning Tree Protocol is essential in enterprise switching. Before this project, STP could be viewed as a background protocol that simply prevents loops. After implementing and verifying it, I now understand that STP is a critical Layer 2 control mechanism that must be intentionally designed.

The most important technical lesson was learning that redundancy and loops are closely related. Adding a second trunk improves availability, but it also creates a loop unless STP is controlling the topology.

I also learned that port planning matters. Because SW2 F0/23 was already assigned to Printer0, the redundant trunk needed to be placed on another available port. This reflects real-world network engineering, where changes must respect existing production connections.

---

## Interview Questions & Model Answers

### 1. What is Spanning Tree Protocol?

**Answer:** Spanning Tree Protocol is a Layer 2 protocol that prevents switching loops by creating a loop-free logical topology. It allows some redundant links to forward traffic while placing other redundant links into blocking or alternate states.

---

### 2. Why is STP needed when redundant switch links exist?

**Answer:** Redundant switch links create multiple paths between switches. At Layer 2, frames do not have a TTL like IP packets, so they can loop endlessly. STP prevents this by blocking one or more redundant paths.

---

### 3. What is the root bridge?

**Answer:** The root bridge is the central reference switch used by STP to calculate the loop-free topology. All switches calculate their best path toward the root bridge.

---

### 4. How does a switch become the root bridge?

**Answer:** The switch with the lowest Bridge ID becomes the root bridge. The Bridge ID includes bridge priority and MAC address. Lower priority wins. If priorities match, the lowest MAC address wins.

---

### 5. Why was F0/22 used instead of F0/23 for the redundant trunk?

**Answer:** F0/23 on SW2 was already assigned to Printer0 as an access port in VLAN 50. Reusing that port as a trunk would break printer connectivity. F0/22 was used as the redundant trunk to preserve the existing printer design.

---

### 6. What is Rapid PVST+?

**Answer:** Rapid PVST+ is Cisco’s rapid per-VLAN implementation of Spanning Tree Protocol. It provides faster convergence than traditional STP and maintains separate STP instances for each VLAN.

---

### 7. What is PortFast?

**Answer:** PortFast allows access ports connected to endpoint devices to move quickly into forwarding state instead of waiting through normal STP transition stages. It should not be used on trunk ports.

---

### 8. What is BPDU Guard?

**Answer:** BPDU Guard protects access ports by shutting them down if they receive a BPDU. This prevents unauthorized switches from affecting the STP topology.

---

### 9. How do you verify the printer port is still correct?

**Answer:** Use `show vlan brief` to confirm F0/23 is assigned to VLAN 50 and use `show running-config interface fastEthernet0/23` to confirm it is configured as an access port.

---

### 10. How do you verify blocked ports?

**Answer:** Use `show spanning-tree blockedports`. This command displays ports that STP has blocked to prevent loops.

---

## Command Reference

| **Command** | **Purpose** |
|------------|-------------|
| `spanning-tree mode rapid-pvst` | Enables Rapid PVST+. |
| `spanning-tree vlan <vlan-list> root primary` | Configures the switch as the primary STP root. |
| `spanning-tree vlan <vlan-list> root secondary` | Configures the switch as the secondary STP root. |
| `show spanning-tree summary` | Displays STP mode and summary information. |
| `show spanning-tree vlan <vlan>` | Displays STP details for a specific VLAN. |
| `show spanning-tree root` | Shows the root bridge per VLAN. |
| `show spanning-tree blockedports` | Shows blocked or alternate ports. |
| `spanning-tree portfast` | Enables PortFast on an access port. |
| `spanning-tree bpduguard enable` | Enables BPDU Guard on a port. |
| `show interfaces trunk` | Displays trunk status and allowed VLANs. |
| `switchport mode trunk` | Configures a port as a trunk. |
| `switchport trunk native vlan 999` | Sets the trunk native VLAN. |
| `switchport trunk allowed vlan <list>` | Restricts VLANs allowed on the trunk. |
| `switchport mode access` | Configures a port as an access port. |
| `switchport access vlan 50` | Assigns the printer access port to VLAN 50. |
| `show vlan brief` | Displays VLAN database and port assignments. |
| `show running-config interface fastEthernet0/23` | Verifies Printer0 access port configuration. |
| `show interfaces status` | Shows interface connection status. |
| `shutdown` | Administratively disables an interface. |
| `no shutdown` | Enables an interface. |
| `write memory` | Saves the configuration. |

---

## Production Considerations

### Change Management

1. Submit a formal change request before modifying STP.
2. Document the affected switches and trunk links.
3. Confirm existing port assignments before repurposing any interface.
4. Identify rollback steps.
5. Schedule the change during a maintenance window.

### Pre-Implementation

1. Back up switch configurations.
2. Capture the existing STP topology.
3. Verify VLANs and trunks.
4. Confirm physical cabling.
5. Confirm SW2 F0/23 is reserved for Printer0.

### Security

1. Enable BPDU Guard on access ports.
2. Avoid using VLAN 1 for production traffic.
3. Use an unused native VLAN such as VLAN 999.
4. Limit allowed VLANs on trunks.
5. Use SSH for remote switch management.
6. Document all trunk and access ports.

---

## Future Enhancements

| **Enhancement** | **Purpose** |
|----------------|-------------|
| EtherChannel | Bundle multiple physical links into one logical link for redundancy and bandwidth. |
| ACLs | Control inter-VLAN communication based on security policy. |
| DHCP Relay | Allow centralized DHCP services across VLANs. |
| SSH Management | Secure remote administration. |
| SNMP Monitoring | Monitor interface and STP status. |
| Syslog Server | Centralize switch and router logs. |
| NTP | Synchronize device timestamps. |
| Port Security | Limit endpoint access by MAC address. |
| Dynamic Routing | Prepare for multi-router enterprise designs. |
| Layer 3 Switching | Improve performance for larger networks. |

---

## Project Roadmap

| **Project** | **Focus Area** | **Status** |
|------------|----------------|------------|
| Project 1 | Basic LAN Foundation | Completed |
| Project 2 | Switch Configuration and Connectivity | Completed |
| Project 3 | VLAN Segmentation | Completed |
| Project 4 | Router-on-a-Stick Inter-VLAN Routing | Completed |
| Project 5 | STP Optimization and Layer 2 Redundancy | Completed |
| Project 6 | ACL Security Policy Implementation | Planned |
| Project 7 | DHCP Services and Relay | Planned |
| Project 8 | EtherChannel and Link Aggregation | Planned |
| Project 9 | Dynamic Routing | Planned |
| Project 10 | Enterprise Network Hardening | Planned |

---

## Final Outcome

The Enterprise STP Optimization Project successfully improved the Layer 2 design by adding redundancy and controlling loop prevention through Rapid PVST+.

The final network now includes:

1. Redundant switch-to-switch trunk links.
2. Primary trunk on SW1 F0/24 to SW2 F0/24.
3. Redundant trunk on SW1 F0/22 to SW2 F0/22.
4. Printer0 preserved on SW2 F0/23 in VLAN 50.
5. Rapid PVST+ for faster convergence.
6. SW1 configured as the primary root bridge.
7. SW2 configured as the secondary root bridge.
8. PortFast enabled on access ports.
9. BPDU Guard enabled on access ports.
10. Controlled trunk VLAN lists.
11. Native VLAN hardening using VLAN 999.
12. Verified blocked or alternate STP paths.
13. Successful connectivity after STP optimization.
14. Successful failover testing after trunk failure simulation.

This project demonstrates practical enterprise switching knowledge and shows the ability to design stable, resilient, and secure Layer 2 networks while preserving existing production device assignments.

---

## Lessons Learned

1. Redundancy must be controlled or it becomes a risk.
2. STP is essential when multiple Layer 2 paths exist.
3. Root bridge placement should be intentional.
4. The switch closest to the gateway is often the best root bridge.
5. Rapid PVST+ improves convergence compared to traditional STP.
6. PortFast should be used only on access ports.
7. BPDU Guard protects the network from unauthorized switches.
8. A blocked STP port is not always a problem. It may be proof that STP is working.
9. Trunk allowed VLAN lists must be consistent.
10. Native VLAN mismatches can create instability.
11. Existing access port assignments must be verified before adding new trunks.
12. SW2 F0/23 must remain the Printer0 access port in this design.
13. F0/22 is the better redundant trunk choice because it avoids disrupting the printer.
14. Verification must include root bridge, port roles, blocked ports, trunks, access ports, and connectivity.
15. Failure testing proves whether redundancy actually works.

---

## Repository Documentation Checklist

| **Item** | **Status** |
|---------|------------|
| README completed | Complete |
| Packet Tracer file uploaded | Complete |
| Topology screenshot uploaded | Complete |
| STP summary screenshot uploaded | Complete |
| Root bridge screenshot uploaded | Complete |
| Blocked port screenshot uploaded | Complete |
| Trunk status screenshot uploaded | Complete |
| Printer port VLAN 50 screenshot uploaded | Complete |
| PortFast and BPDU Guard screenshot uploaded | Complete |
| Failover test screenshot uploaded | Complete |
| Running configuration files uploaded | Complete |
| Troubleshooting notes uploaded | Complete |

---

## Suggested Repository Name

```text
enterprise-stp-optimization
```

## Suggested Repository Description

```text
Enterprise Layer 2 redundancy project demonstrating Rapid PVST+, root bridge control, redundant trunk links on F0/24 and F0/22, Printer0 access-port preservation on SW2 F0/23, PortFast, BPDU Guard, STP verification, and troubleshooting using Cisco Packet Tracer.
```<img width="710" height="311" alt="Screenshot 2026-07-06 170550" src="https://github.com/user-attachments/assets/7c313443-571f-46a0-8c12-2138f0845b55" />
