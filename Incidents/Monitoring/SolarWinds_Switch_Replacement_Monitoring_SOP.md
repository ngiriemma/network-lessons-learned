# SOP – SolarWinds Monitoring During Switch Replacement

## Purpose
This SOP defines a **standard procedure** to handle SolarWinds alerts during and after a switch replacement.
It includes:
- A clear resolution workflow
- A decision tree
- Pre- and post-replacement monitoring checklists

The objective is to **avoid false positives, unnecessary downtime, and node re-creation**.

---

## Scope
Applicable to:
- Cisco switch replacements (e.g. Catalyst 9200, 9300)
- Environments monitored by SolarWinds (NPM)

---

## 1. Pre-Replacement Monitoring Checklist

Before replacing the switch, perform the following checks in SolarWinds:

- ☐ Confirm the node is **Up**
- ☐ Note existing **active alerts**
- ☐ Identify **interfaces currently monitored**
- ☐ Export or record interface descriptions if needed
- ☐ Confirm whether the **IP address will be reused**
- ☐ Confirm whether the **hostname will change**

> Reusing the same IP address is supported, but may require interface re-polling after replacement.

---

## 2. Post-Replacement Monitoring Checklist

After the switch is replaced and confirmed operational:

- ☐ Verify the switch is reachable (ICMP / SNMP)
- ☐ Confirm interfaces are **Up** and passing traffic
- ☐ Check SolarWinds for **Interface Needs Attention** alerts
- ☐ Identify whether alerts reference **old interfaces**
- ☐ Allow 1–2 polling cycles for auto-clear

If alerts persist, follow the decision tree below.

---

## 3. Decision Tree – How to Resolve Interface Alerts

### Step 1 – Is the network actually impacted?
- ❓ Are users or services affected?
  - **Yes** → Treat as a real incident and troubleshoot the interface
  - **No** → Proceed to Step 2

---

### Step 2 – Are alerted interfaces valid?
- ❓ Do the interfaces exist on the new switch?
  - **Yes** → Check interface config, errors, speed/duplex
  - **No** → Proceed to Step 3

---

### Step 3 – Refresh interface inventory (no downtime)
1. Go to **Node Details**
2. Select **List Resources**
3. Review interface list
4. Select only valid interfaces
5. Submit to re-poll

⏱ No downtime – monitoring-only action.

---

### Step 4 – Clear triggered alerts manually
- Navigate to **Alerts**
- Locate the triggered alert
- Select **Clear Triggered Alert**

> Some SolarWinds alerts do not auto-clear after hardware changes.

---

### Step 5 – Are alerts cleared?
- **Yes** → Incident resolved
- **No** → Proceed to Step 6

---

### Step 6 – Review pollers
- Go to **Node Details → Pollers**
- Verify interface pollers
- Remove pollers linked to obsolete interfaces if present

---

### Step 7 – Last resort only
- ❗ Remove and re-add the node
- ❗ Use only if interface inventory cannot be reconciled

> Node deletion may cause loss of historical data and should be avoided unless necessary.

---

## 4. Important Rules

- Interface re-polling does **not cause downtime**
- Clearing alerts does **not affect the device**
- Reusing IP + changing hostname may confuse monitoring tools
- Not all alerts clear automatically

---

## 5. Final Validation

After resolution:
- ☐ SolarWinds shows **node Up**
- ☐ No stale interface alerts
- ☐ Interfaces reflect the new switch hardware
- ☐ Monitoring matches real network state

---

## Key Takeaway
> SolarWinds alerts after switch replacement are often caused by **stale interface data**, not real failures.

A structured approach avoids unnecessary escalation and node recreation.

---

_End of SOP_
