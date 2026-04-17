# Challenges and Lessons Learned

This document captures key challenges encountered while building and operating the
air‑gapped lab, along with the lessons learned. The goal is to document realistic
troubleshooting scenarios and the practical skills developed while resolving them.

---

## Network & Firewall Troubleshooting

### Challenge: Firewall Rules Blocking Required Traffic
One of the primary challenges encountered was determining why systems on the
Air‑Gap virtual switch were unable to communicate with the Domain Controller.

Although the network segmentation was intentional, some required traffic was being
blocked due to overly restrictive firewall rules.

**Resolution:**
- Reviewed pfSense firewall rules to identify implicit default‑deny behavior
- Explicitly allowed required communication between the Air‑Gap network and the
  Domain Controller
- Validated connectivity incrementally rather than opening broad access

**Lesson Learned:**
- Default‑deny firewall models are effective but require careful planning
- When troubleshooting connectivity issues, always validate firewall rules early
- Incremental rule changes reduce risk and simplify troubleshooting

---

## pfSense Misconfiguration Incident

### Challenge: Duplicate pfSense IP Addresses
An early networking issue occurred when two pfSense virtual machines were accidentally
running simultaneously using the same IP address.

This caused intermittent network instability and unpredictable routing behavior,
making troubleshooting more difficult.

**Resolution:**
- Identified the duplicate IP address through network inspection and VM review
- Shut down the unintended pfSense instance
- Verified all firewall and routing traffic was flowing through the intended firewall

**Lesson Learned:**
- Clear inventory and naming of critical infrastructure VMs is essential
- Duplicate IP issues can manifest as vague or inconsistent network failures
- Infrastructure components should be validated before deeper troubleshooting begins

---

## DNS & Name Resolution in Air‑Gapped Environments

### Challenge: Services Failing Without Proper DNS Entries
Several components of the Tanium platform require fully qualified domain names (FQDNs)
rather than IP addresses.

Initially, missing or incomplete DNS records caused service resolution and connectivity
issues inside the air‑gapped environment.

**Resolution:**
- Created the required DNS records on the Domain Controller
- Validated internal name resolution from air‑gapped systems
- Ensured all services referenced FQDNs instead of raw IP addresses

**Lesson Learned:**
- DNS is a critical dependency, especially in isolated networks
- Enterprise applications often assume proper DNS configuration
- Testing name resolution should be a standard troubleshooting step

---

## Virtual Machine & Infrastructure Setup Challenges

### Challenge: Virtual Server Configuration and Dependencies
During the setup of multiple Windows Server VMs, some issues were encountered related to:
- Service dependencies
- VM resource allocation
- Order of infrastructure deployment

Some services failed initially due to upstream dependencies not being available.

**Resolution:**
- Reviewed service dependency order
- Ensured core services (AD, DNS) were stable before deploying dependent systems
- Adjusted VM configurations where necessary

**Lesson Learned:**
- Infrastructure build order matters
- Foundational services should be fully operational before layering on enterprise tools
- Clear documentation helps prevent repeated setup errors

---

## Key Takeaways

- Network issues are often the root cause of seemingly unrelated problems
- Firewall troubleshooting requires both technical accuracy and patience
- DNS misconfigurations can have widespread downstream effects
- Air‑gapped environments magnify small configuration errors
- Methodical, layered troubleshooting is essential in restricted networks

---

## Closing Notes

These challenges reinforced the importance of strong documentation, controlled change
management, and deliberate troubleshooting. The lab continues to evolve as new scenarios
and improvements are explored.
