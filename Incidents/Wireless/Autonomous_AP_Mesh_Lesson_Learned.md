# Lesson Learned: Understanding Autonomous AP (RAP) and LWAP Mesh Architecture

## Context
This lesson documents a real-world industrial / enterprise wireless bridging scenario involving:
- Autonomous Access Points (AP)
- Cisco Wireless LAN Controller (WLC 9800)
- Mesh networking (RAP / MAP)
- Industrial devices such as PLCs, sensors, and HMIs

The goal is to clearly understand where the autonomous AP sits in the architecture and how traffic flows from the field to the corporate network.

All names, IPs, and locations are sanitized.

---

## Key Question
How do we determine where an autonomous system is connected when LWAP mesh is involved?

---

## Verification from WLC (CLI)

To identify the AP role and mode:

show ap dot11 5ghz summary  
show ap dot11 24ghz summary  
show ap name <AP-NAME> config dot11 5ghz  

Key fields to observe:
- AP Mode
- AP Role
- Bridge Group Name
- Site Tag

### Observed Result
- AP Role: Mesh AP
- AP Mode: Bridge

This confirms the AP is operating in mesh bridge mode.

---

## Mesh Concepts Refresher

### RAP – Root Access Point (Root Bridge)
- Wired Ethernet uplink to the switch
- Entry point from RF mesh into the wired LAN
- Parent of the mesh network
- Provides connectivity to the enterprise network

In this scenario, the autonomous AP is the RAP.

---

### MAP – Mesh Access Point (Non-Root)
- No wired uplink
- Connects wirelessly to the RAP
- Forwards traffic back to the RAP
- Controlled by the WLC

---

## Architecture Explanation

### Logical Flow

Autonomous AP (RAP) → Mesh RF → LWAP (MAP) → Ethernet → Switch → WLC

### Real-World Interpretation
The autonomous AP is not behind the WLC.  
It is upstream, acting as the root bridge for the mesh.

---

## End-to-End Traffic Flow

### Side A – Field / Industrial Area

PLC / Sensors  
→ Access Switch  
→ Autonomous AP (RAP – Root Bridge)  
→ Mesh RF  

The autonomous AP is physically cabled to the switch and is the starting point of RF connectivity.

---

### Side B – Control Room / Corporate Network

Mesh AP (MAP – LWAP)  
→ Ethernet  
→ Switch  
→ WLC 9800  
→ HMI / Servers / SCADA  

---

## Why This Architecture Is Used
- No cabling possible between field and control room
- Industrial environment constraints
- Reliable long-distance wireless bridge
- Clear separation between OT and IT domains

---

## Common Misconception
Assuming the autonomous AP is behind the WLC.  
In reality, the autonomous AP is the root bridge (RAP).

---

## Lessons Learned
- Always verify AP Role and AP Mode on the WLC
- RAP defines the entry point of the mesh into the LAN
- Autonomous APs can act as core bridging elements
- Understanding traffic direction avoids wrong troubleshooting paths

---

## Troubleshooting Questions
- Where is the RAP physically connected?
- Which AP has the wired uplink?
- Is the issue RF (mesh) or wired (LAN/WLC)?

---

## Summary
This hybrid autonomous and controller-based mesh design is common in industrial and OT environments.
Understanding the RAP/MAP relationship is critical for correct troubleshooting and design decisions.

---

End of lesson
