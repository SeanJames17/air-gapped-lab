# Network Design

## Overview
This document describes the network architecture and security model used in the
Air‑Gapped Lab. The design focuses on strict segmentation, default‑deny networking,
and controlled access to shared infrastructure services while maintaining isolation
for internal enterprise workloads.

The network is intentionally designed to mirror restricted or regulated environments
where most systems operate without direct internet access.

---

## Network Segmentation Model

The lab is segmented using multiple Hyper‑V virtual switches to enforce logical and
security boundaries between systems.

### Air‑Gap Virtual Switch
- Dedicated internal Hyper‑V virtual switch labeled **Air‑Gap**
- Hosts all isolated workloads, including Tanium servers
- No direct inbound or outbound internet connectivity
- Communication restricted to:
  - Other systems on the Air‑Gap switch
  - The Domain Controller (for shared services only)

### External / Administrative Network
- Used strictly for controlled administrative access
- Provides limited internet connectivity where required
- Not directly accessible from air‑gapped systems

---

## Firewall Architecture (pfSense)

A **pfSense firewall** deployed as a virtual machine enforces all network access
control between segments.

### Firewall Responsibilities
- Acts as the sole traffic broker between network segments
- Enforces explicit firewall rules for allowed communication
- Blocks all other traffic by default (default‑deny model)

### Firewall Policy Summary
- **Allowed:**
  - Air‑Gap systems ↔ Domain Controller (required services only)
  - Intra‑Air‑Gap communication between trusted systems
- **Denied by Default:**
  - Any outbound internet access from air‑gapped systems
  - Any lateral or cross‑network traffic not explicitly permitted

This model ensures that no system can communicate outside its intended boundary unless
a rule explicitly allows it.

---

## Domain Controller Network Design

The Domain Controller functions as a shared infrastructure and boundary system.

### Dual‑NIC Configuration
The Domain Controller uses two network interfaces for separation of duties:

- **NIC 1 – External / Administrative**
  - Provides controlled internet access
  - Used for administrative tasks such as downloading updates, tools, or artifacts
  - Restricted by firewall policy

- **NIC 2 – Internal / Air‑Gapped**
  - Serves Active Directory, DNS, NTP, and file services to air‑gapped systems
  - No direct internet access

Air‑gapped systems never communicate directly with the external network; they rely on
the Domain Controller for required internal services only.

---

## Core Network Services

The Domain Controller provides the following services to the air‑gapped environment:

- Active Directory Domain Services (AD DS)
- DNS (internal name resolution)
- NTP (time synchronization)
- File services (internal file share)

### DNS Configuration
- Internal DNS is hosted on the Domain Controller
- Custom DNS records were created to support internal service discovery
- Air‑gapped systems resolve all internal hostnames without relying on external DNS
- External DNS resolution is not available to air‑gapped systems

---

## Access & Trust Boundaries

- Air‑gapped systems do not have direct internet access
- All external access is mediated through infrastructure components
- Trust boundaries are enforced at the network and service level
- Firewall rules follow a least‑privilege approach
- No NAT or accidental switch bridging is permitted

---

## Design Rationale

This network design emphasizes:
- Strong isolation of critical systems
- Clear trust boundaries between services
- Realistic enterprise security constraints
- Repeatable and documentable architecture decisions

The environment enables practical testing and troubleshooting of enterprise software
and identity services in scenarios where connectivity is constrained or unavailable.

---

## Notes
- Network rules and segmentation are intentionally conservative
- The design favor correctness and security over convenience
- Documentation evolves as the lab grows
