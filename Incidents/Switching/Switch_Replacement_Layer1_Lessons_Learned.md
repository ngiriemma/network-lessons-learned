# Lesson Learned – Switch Replacement, Uplink Media, and Layer-1 Validation

## Context
During the replacement of a defective network switch, the new switch was correctly selected and logically configured.
However, additional time was lost due to uplink media identification and SFP compatibility verification that were not fully validated beforehand.

This experience reinforced the importance of Layer-1 checks and on-site preparation during switch replacement activities.

---

## Key Lessons Learned

### 1. Configuration alone is not enough
Choosing the correct switch model and applying the expected configuration (VLANs, trunks, routing, management access) is necessary, but it does not guarantee connectivity.

Physical connectivity must always be validated.

---

### 2. Always verify the uplink type
Before replacing a switch, always confirm:
- Is the uplink connected via Ethernet (copper) or Fiber?
- Which interface is used on the existing switch?
- Which interface will be used on the replacement switch?

Never assume the uplink type based only on configuration or documentation.

---

### 3. If the uplink is fiber, verify SFP compatibility
When fiber is involved, checks must be done on both the existing switch and the new switch:
- Which SFP model is installed?
- What speed does it support (100 Mbps, 1G, 10G)?
- What fiber type is used (Single-Mode or Multi-Mode)?
- What wavelength is used (e.g. 850 nm, 1310 nm)?

Both ends of the fiber link must match in speed, standard, wavelength, and fiber type.

---

### 4. Cisco Catalyst 9200 SFP compatibility (important clarification)
When working with Cisco Catalyst 9200 series:
- GLC-GE-100FX V03 is compatible
- Earlier hardware revisions of GLC-GE-100FX may not work

Always verify the exact SFP revision and platform compatibility.

---

### 5. Validate compatibility between old and new switches
Always compare the capabilities of the existing switch and the replacement switch.
Older platforms may support legacy optics differently than newer platforms.

---

### 6. Prepare Ethernet management access (strong recommendation)
Before installation, configure at least one access port in the management VLAN:
- Assign the port to the management VLAN
- Configure an IP address on the switch
- Allow the technician to connect using a standard Ethernet cable

This avoids delays if the correct console cable (Mini-USB / USB) is not available.

---

### 7. Layer-1 issues can cause major delays
If uplink media and SFP compatibility are not verified beforehand, significant time can be lost testing optics and cabling.
Layer-1 validation should always be done before escalating the issue.

---

## Pre-Replacement Checklist

✔ Identify uplink type (Ethernet or Fiber)  
✔ Identify uplink interface  
✔ Identify SFP model and revision on existing switch  
✔ Verify SFP compatibility on replacement switch  
✔ Confirm fiber type and wavelength  
✔ Prepare management VLAN Ethernet access  
✔ Confirm console cable availability  
✔ Have spare compatible SFPs available  

---

## Useful Cisco Commands – Layer-1 and SFP Verification

### Identify installed SFPs
```cisco
show inventory
show inventory | include -i sfp
show inventory | include -i transceiver
```

### Check SFP on a specific interface
```cisco
show interfaces GigabitEthernet1/0/49 transceiver
show interfaces transceiver detail
```

### Verify interface physical status
```cisco
show interfaces GigabitEthernet1/0/49
show interfaces status
```

### Verify physical neighbor connectivity
```cisco
show cdp neighbors interface GigabitEthernet1/0/49 detail
show lldp neighbors interface GigabitEthernet1/0/49 detail
```

---

## Final Takeaway
A switch replacement is not only a configuration task.
It is a Layer-1 validation exercise involving uplinks, SFP compatibility, cabling, and management access preparation.

---
_End of lesson learned_
