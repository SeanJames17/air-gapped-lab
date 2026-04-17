# Air-Gapped Lab

## Overview
This repository documents a fully air‑gapped infrastructure lab designed to simulate a
secure, isolated enterprise environment. The lab is hosted on a Windows 11 system and
leverages Hyper‑V to run multiple servers, security boundaries, and enterprise workloads
with tightly controlled network access.

The environment is purpose‑built to model real‑world restricted networks where most
systems operate without internet connectivity, while access is selectively brokered
through controlled infrastructure services.

---

## Lab Environment

### Host System
- Windows 11 (domain-joined)
- Hyper‑V enabled
- Multiple virtual switches both (WAN,LAN,Airgap)

---

## Virtualized Infrastructure (Hyper‑V)

### Network & Security
- **pfSense Firewall (VM)**
  - Acts as the central firewall and traffic broker
  - Enforces strict firewall rules between network segments
  - Controls all allowed communication between air‑gapped systems and shared services

---

### Identity & Core Services

#### Domain Controller
- Windows Server
- Active Directory Domain Services (AD DS)
- DNS
- NTP
- CA 
- File Services (internal file shares)

**Network Configuration:**
- Dual‑NIC configuration:
  - **NIC 1 (Internet‑facing):** Controlled internet access for administrative needs
    such as downloading updates, tools, or artifacts
  - **NIC 2 (Internal / Air‑Gapped):** Serves domain services to isolated systems

The Domain Controller functions as a controlled boundary system, providing directory,
name resolution, time services, and file sharing to the air‑gapped environment while
maintaining a separate, restricted path for external access.

---

### Tanium Environment
- **Tanium Primary Server**
- **Tanium Secondary Server**
- **Tanium Module Server**
- **Tanium Web Console Host**

All Tanium components operate within the air‑gapped network and do not have direct
internet access. Access to the Tanium web console is provided internally, with all
external communication strictly controlled via firewall policy and shared services.

---

## Network Design

- Fully segmented, air‑gapped internal network
- No direct internet access for Tanium servers or internal workloads
- pfSense VM enforces firewall rules and traffic flow
- Domain Controller bridges services between secured internal systems and controlled
  external access
- Internal DNS provided by Active Directory
- No NAT or accidental bridging of isolated virtual switches
- The environment is segmented using a dedicated “Air‑Gap” virtual switch.
Systems connected to this network are intentionally isolated from external networks and are only permitted to communicate with:
Other systems on the same Air‑Gap virtual switch
The Domain Controller, which provides directory, DNS, time, and file services

All other network traffic is denied by default unless explicitly allowed by firewall policy. This enforces a least‑privilege networking model and prevents unintended access outside of the isolated environment.

This design allows realistic testing of enterprise software and infrastructure behavior
in environments where internet access is unavailable or heavily restricted.

---

## Objectives

- Build and maintain a segmented, air‑gapped network
- Practice Active Directory design and administration
- Operate Tanium in a restricted environment
- Enforce firewall rules and trust boundaries with pfSense
- Simulate real‑world patching and file transfer workflows
- Develop troubleshooting skills in constrained environments

---

## Key Technologies

- Windows 11
- Windows Server
- Hyper‑V
- Active Directory & DNS
- pfSense
- Tanium Platform
- Enterprise firewalling and network segmentation concepts

---

## Security Model

- No unrestricted internet access for internal systems
- Explicit trust boundaries enforced by firewall rules
- Controlled administrative access via shared services only
- Emphasis on least‑privilege networking and isolation
- Manual and deliberate artifact transfer

---

## Purpose of This Repository

This repository documents:
- Architecture decisions and network design
- Security boundaries and access controls
- Systems administration workflows
- Troubleshooting and lessons learned
- Repeatable lab build patterns

It also serves as a practical demonstration of enterprise‑focused systems administration
experience.

---

## Disclaimer

This lab is for educational and testing purposes only.  
No production data, credentials, or proprietary content is stored in this repository.
