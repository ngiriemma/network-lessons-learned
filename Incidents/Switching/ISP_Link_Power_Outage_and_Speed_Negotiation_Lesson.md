# Lesson Learned: ISP Link Failure After Power Outage – Speed Negotiation Impact

## Scenario
A customer experienced repeated outages on a WAN link connected to an ISP.
The issue occurred **only after power outages**:

- The physical interface went down during the outage
- After power restoration, the link did not recover automatically
- Manual intervention on the ISP side (loop / link bounce) was required
- The problem persisted for several months and caused service downtime

Under normal conditions, the link was stable and operational.

---

## Key Challenge
The main challenge was **not understanding auto-negotiation theory**, but identifying the **relationship between the issue and power outages**.

Because the problem appeared only after power loss, it initially looked like an ISP or hardware issue.
The real trigger was **link renegotiation during recovery**, not steady-state operation.

---

## Analysis
The investigation focused on:
- Interface behavior after power restoration
- Speed and duplex negotiation state
- ISP handoff characteristics
- Recovery behavior without manual intervention

A key observation emerged:
> After power restoration, the customer switch and the ISP device failed to renegotiate the link correctly.

This pointed to a **Layer 1 / Layer 2 negotiation timing issue**.

---

## Root Cause
The WAN interface relied on **auto-negotiation**.

After power outages:
- The ISP device presented a fixed-speed signal
- Auto-negotiation on the customer switch did not complete successfully
- The link remained unusable until renegotiation was forced manually

The issue was therefore **power-event triggered**, not configuration or routing related.

---

## Solution Implemented
The following configuration was applied on the customer switch interface connected to the ISP:

```text
speed nonegotiate
```

### What this means in practice
- The ISP port already had a fixed speed configured
- By disabling negotiation, the customer switch aligned directly with the ISP’s configured speed
- Link recovery no longer depended on negotiation timing after power restoration

No additional configuration changes were required.

---

## Result
- The WAN link now recovers automatically after power outages
- No manual ISP intervention is required
- The long-standing issue has been permanently resolved
- Network availability and stability have improved significantly

A problem that lasted for months was resolved with **one configuration command**.

---

## Auto-Negotiation Explained (Practical View)

### What Auto-Negotiation Does
Auto-negotiation allows two Ethernet devices to automatically agree on:
- Speed
- Duplex
- Flow control

It works very well in **LAN environments** where both sides renegotiate reliably.

---

### Where Auto-Negotiation Can Fail
Auto-negotiation issues commonly appear when:
- One side uses fixed speed
- The other side relies on auto
- Devices reboot at different times
- Carrier-grade equipment behaves differently after power events

These failures often appear **only during recovery**, not during normal operation.

---

## Best Practices

### Switch ↔ Switch (LAN)
```text
speed auto
duplex auto
```
- Recommended
- IEEE-compliant
- Reliable renegotiation

### Client ↔ ISP (WAN Handoff)
```text
speed <fixed>
duplex full
speed nonegotiate
```
- More deterministic
- Avoids renegotiation failures
- Improves recovery after power outages

---

## Lessons Learned
- Power outages can expose hidden Layer-1 issues
- Auto-negotiation failures may only appear during recovery
- WAN links behave differently from LAN links
- Default settings are not always appropriate
- Simple changes can have a large operational impact

---

## Key Takeaway
When an ISP link fails **only after power outages**, always evaluate **speed and negotiation behavior**.

Sometimes, **removing negotiation entirely** is the most reliable solution.

---

_End of lesson_
