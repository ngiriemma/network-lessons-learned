# SOP – Switch Replacement Pre‑Check (Layer‑1 & Access Validation)

## Purpose
This SOP defines the **mandatory pre‑checks** to perform before replacing a network switch.
Its goal is to **prevent delays and outages** caused by Layer‑1 issues, uplink mismatches, or lack of management access.

This SOP should be executed **before every switch replacement**, regardless of scope.

---

## 1. Pre‑Replacement Preparation (Before Going On‑Site)

### 1.1 Identify the uplink type
Confirm how the existing switch is connected upstream:
- ☐ Ethernet (Copper)
- ☐ Fiber

Do **not** assume based on documentation alone.

---

### 1.2 If uplink is Fiber – validate optics
Collect and verify the following on the **existing switch**:
- ☐ SFP model
- ☐ SFP revision (e.g. V03)
- ☐ Speed (100M / 1G / 10G)
- ☐ Fiber type (SM / MM)
- ☐ Wavelength (850 nm / 1310 nm)

Confirm the **replacement switch supports the same optic**.

> Example:  
> Cisco Catalyst 9200 supports **GLC‑GE‑100FX V03**, while earlier revisions may not be compatible.

---

### 1.3 Verify platform compatibility
Compare:
- Existing switch model
- Replacement switch model
- Supported SFPs on both platforms

Never assume backward compatibility between switch generations.

---

### 1.4 Prepare management access (critical)
Before replacement:
- ☐ Configure **one access port** in the management VLAN
- ☐ Ensure the switch has a **management IP**
- ☐ Bring a standard Ethernet cable

Reason:
- Console cables (Mini‑USB / USB) may be unavailable
- Drivers may be missing
- Ethernet access avoids on‑site blocking issues

---

## 2. On‑Site Replacement – Layer‑1 Validation

### 2.1 Verify SFP presence and detection
```cisco
show inventory
show inventory | include -i sfp
show inventory | include -i transceiver
```

Confirm:
- Correct SFP model
- Correct revision
- SFP is detected by the switch

---

### 2.2 Verify uplink interface status
```cisco
show interfaces status
show interfaces GigabitEthernet1/0/49
```

Check:
- Interface is **up/up**
- Speed and duplex are correct
- No excessive CRC or input errors

---

### 2.3 Verify physical neighbor
```cisco
show cdp neighbors interface GigabitEthernet1/0/49 detail
```
or
```cisco
show lldp neighbors interface GigabitEthernet1/0/49 detail
```

Confirm:
- Correct upstream device
- Correct remote interface

---

### 2.4 Verify optical signal (if supported)
```cisco
show interfaces GigabitEthernet1/0/49 transceiver detail
```

If DOM is not supported, the message  
**“Diagnostic Monitoring is not implemented”** is normal.

---

## 3. Management Access Validation

### 3.1 Ethernet access test
- ☐ Connect laptop to management VLAN port
- ☐ Assign IP in same subnet
- ☐ Ping switch management IP
- ☐ SSH to the switch

This step guarantees access even if console connectivity fails.

---

## 4. Post‑Replacement Verification

### 4.1 Confirm uplink stability
```cisco
show interfaces GigabitEthernet1/0/49 counters errors
```

### 4.2 Confirm routing / upstream reachability
```cisco
ping <default‑gateway>
ping 8.8.8.8
```

---

## 5. Final Rule
> A switch replacement is **not complete** until Layer‑1, uplink optics, and management access are validated.

Skipping these checks risks unnecessary downtime and on‑site delays.

---

_End of SOP_
