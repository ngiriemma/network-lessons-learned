# Lesson Learned – SolarWinds Interface Alerts After Switch Replacement

## Context
After replacing a Cisco Catalyst 9200 switch, SolarWinds started reporting **"Interface Needs Attention"** alerts.
However, from a network perspective, the switch and connectivity were fully operational.

The root cause was related to **SolarWinds interface polling and alert state persistence** after hardware replacement.

---

## Observed Symptoms
- SolarWinds reports **Interface Needs Attention**
- Network traffic is flowing normally
- Interfaces mentioned in alerts belong to the **old switch**
- Switch IP address remained the same
- Switch hostname was changed
- Some alerts cleared automatically, others did not

---

## Root Cause Analysis
When a switch is replaced **without changing its management IP address**, SolarWinds may still associate:
- Old interface indexes
- Old interface descriptions
- Historical poller data

with the new hardware.

As a result:
- SolarWinds continues polling **non-existing interfaces**
- Alerts remain active even though the physical issue is resolved

This is expected behavior when device hardware changes but the node identity (IP) remains the same.

---

## Resolution Steps (No Downtime)

> The following steps **do NOT cause network downtime**, as they only affect SolarWinds polling and alerting.

### Step 1 – Verify the network state
Before making any change in SolarWinds:
- Confirm the switch is reachable
- Verify interfaces are up and passing traffic
- Ensure the issue is not a real physical problem

---

### Step 2 – List and re-poll interfaces
In SolarWinds:
1. Go to **Node Details**
2. Click **List Resources**
3. Review the interface list
4. Ensure only valid interfaces are selected
5. Submit changes to force re-discovery

---

### Step 3 – Check interface pollers
From **Node Details**:
1. Click **Pollers**
2. Verify interface-related pollers are correctly assigned
3. Remove any pollers linked to obsolete interfaces (if present)

---

### Step 4 – Clear triggered alerts manually
Some alerts do not clear automatically.

1. Go to **Alerts**
2. Locate the triggered alert
3. Select **Clear Triggered Alert**
4. Confirm clearing action

---

### Step 5 – Verify node identity consistency
Key realization in this case:
- The **IP address was reused**
- The **switch hostname changed**

Once SolarWinds completed interface re-polling and alerts were cleared, the issue was resolved **without removing the node**.

> Removing and re-adding the node should be a **last resort only**.

---

## Important Notes
- Not all alerts clear automatically after hardware replacement
- Some alerts are cleared by re-polling interfaces
- Others require **manual clearing**
- Removing the node is not always necessary

---

## Final Outcome
- SolarWinds interface alerts cleared
- No downtime occurred
- Monitoring accurately reflects the new switch state

---

## Key Takeaways
- Replacing hardware without changing IP can confuse monitoring tools
- Always validate interface inventory after switch replacement
- Clearing alerts manually may be required
- Avoid deleting and re-adding nodes unless strictly necessary

---

_End of lesson learned_
