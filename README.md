# GNS3 Networking Fundamentals Lab

Last reviewed: May 5, 2026

This repo documents a hands-on GNS3 bring-up and early networking lab sequence completed on a Windows 11 Pro workstation using Hyper-V and the GNS3 VM.

The work started as a practical setup and troubleshooting session, then progressed into three core networking exercises:

- same-subnet host connectivity
- ARP behavior on a switched network
- inter-subnet routing through a basic Linux router

It is written as a portfolio-ready case study backed by screenshots from the actual session.

## Objective

Build a working GNS3 practice environment on Windows, then use it to demonstrate foundational Network+ topics:

- local GNS3 controller/server troubleshooting
- Hyper-V GNS3 VM integration
- VPCS node setup and template correction
- IPv4 addressing and subnet masks
- same-subnet communication over a Layer 2 switch
- ARP resolution for local traffic
- default gateway behavior for remote networks
- basic routing between two IPv4 subnets

## Environment

- Host OS: Windows 11 Pro
- Virtualization platform: Hyper-V
- GNS3 version shown in evidence: `2.2.58.1`
- GNS3 VM package: Hyper-V edition
- Lightweight endpoint nodes: VPCS
- Router used for inter-subnet lab: Alpine Linux Docker container in GNS3 VM

## Hiring Manager Quick View

| Review area | Evidence |
|---|---|
| Lab platform bring-up | GNS3 VM download, controller settings, server selection |
| Troubleshooting | Fixed controller connectivity, VPCS executable path, and compute-placement mismatch |
| Switching and IPv4 | Two-host same-subnet ping validation |
| ARP and gateway logic | ARP table shows the gateway MAC during routed traffic |
| Routing | Two-subnet communication succeeds through a Linux router with IP forwarding enabled |
| Documentation quality | Screenshot map, step narrative, and lessons learned tied to actual outputs |

## Evidence Set

Screenshots are stored in [`images/`](images/).

1. [01-gns3-vm-download.png](images/01-gns3-vm-download.png) - GNS3 VM package downloaded successfully
2. [02-local-server-controller-settings.png](images/02-local-server-controller-settings.png) - local controller/server settings reviewed while resolving controller connectivity
3. [03-vm-server-selection.png](images/03-vm-server-selection.png) - GNS3 shows both `MAIN-PC` and `GNS3 VM` as available compute targets
4. [04-vpcs-executable-path.png](images/04-vpcs-executable-path.png) - VPCS executable path corrected so lightweight host nodes could start
5. [05-two-host-same-subnet-ping.png](images/05-two-host-same-subnet-ping.png) - first successful same-subnet ping between two VPCS nodes
6. [06-alpine-router-template-start-command.png](images/06-alpine-router-template-start-command.png) - Alpine Docker template creation for a simple router appliance
7. [07-cross-compute-link-error.png](images/07-cross-compute-link-error.png) - compute-placement error when trying to link nodes across `MAIN-PC` and `GNS3 VM`
8. [08-alpine-router-interface-config.png](images/08-alpine-router-interface-config.png) - router interfaces configured with `192.168.10.1/24` and `192.168.20.1/24`, plus IPv4 forwarding
9. [09-pc1-routing-success-and-arp.png](images/09-pc1-routing-success-and-arp.png) - PC1 reaches its gateway and the remote subnet; ARP shows the gateway MAC
10. [10-pc2-routing-success-and-arp.png](images/10-pc2-routing-success-and-arp.png) - PC2 reaches its gateway and the remote subnet from the opposite side

## Topology Progression

### 1. Initial same-subnet test

```text
PC1 ---- PC2
```

This quick test exposed the missing VPCS executable path before any real networking work could continue.

### 2. Switched LAN test

```text
PC1 ---- Switch1 ---- PC2
```

This validated that two hosts in the same subnet could communicate through a Layer 2 switch without a router.

### 3. Routed two-subnet lab

```text
PC1 ---- Switch1 ---- AlpineRouter ---- Switch2 ---- PC2
```

IP plan used in the routed lab:

- PC1: `192.168.10.10/24`, gateway `192.168.10.1`
- Router `eth0`: `192.168.10.1/24`
- Router `eth1`: `192.168.20.1/24`
- PC2: `192.168.20.10/24`, gateway `192.168.20.1`

## Lab Narrative

### 1. Bring up the GNS3 platform

The session began with the GNS3 VM package download and initial Hyper-V integration.

Early friction points were normal for a first-time setup:

- the GNS3 GUI was open but not fully connected to the local controller
- the Hyper-V-backed GNS3 VM had to be enabled and selected correctly
- the local GNS3 server settings had to be verified before VM settings would load consistently

Evidence:

- [01-gns3-vm-download.png](images/01-gns3-vm-download.png)
- [02-local-server-controller-settings.png](images/02-local-server-controller-settings.png)
- [03-vm-server-selection.png](images/03-vm-server-selection.png)

### 2. Fix the first endpoint template problem

The first lab used VPCS nodes because they are lightweight and appropriate for basic IP addressing and ICMP testing.

That failed at first because the VPCS executable path had not been set correctly in GNS3.

Once the VPCS path was fixed, new VPCS nodes could launch normally.

Evidence:

- [04-vpcs-executable-path.png](images/04-vpcs-executable-path.png)

### 3. Validate same-subnet communication

With VPCS working, the first practical networking test was a simple same-subnet host-to-host ping.

Observed behavior:

- two VPCS endpoints were configured in the same IPv4 subnet
- ICMP succeeded between the two hosts
- this confirmed the GNS3 local server, node template, console access, and basic node connectivity were all working

Evidence:

- [05-two-host-same-subnet-ping.png](images/05-two-host-same-subnet-ping.png)

Operational takeaway:

- same subnet plus Layer 2 connectivity is enough for direct host communication
- no default gateway is required when traffic stays inside the local network

### 4. Build a simple router appliance for the next lab

The next objective was inter-subnet routing, but there was no ready-made router template available in the default node list.

Instead of stopping there, the workflow pivoted to building a small Alpine Linux Docker container template with two adapters and a shell start command.

Evidence:

- [06-alpine-router-template-start-command.png](images/06-alpine-router-template-start-command.png)

This was a practical move:

- it kept the lab legal and lightweight
- it still demonstrated real interface configuration and IP forwarding
- it introduced a new troubleshooting layer around where each node was running

### 5. Diagnose cross-compute placement issues

The first attempt to connect the new router to the existing VPCS nodes failed because the router was placed on `GNS3 VM` while the VPCS nodes were still on `MAIN-PC`.

That produced a useful GNS3 error rather than a vague failure:

- there was no common subnet for the compute backends
- the topology had to be rebuilt so the nodes participating in that lab ran on compatible compute targets

Evidence:

- [07-cross-compute-link-error.png](images/07-cross-compute-link-error.png)

Why this matters:

- this is not just a networking concept problem
- it shows awareness of how the lab platform itself can create connectivity constraints

### 6. Configure the router and enable forwarding

Once the topology placement was corrected, the Alpine router was configured with one interface in each subnet and IPv4 forwarding enabled.

Commands shown in the evidence:

```sh
ip addr add 192.168.10.1/24 dev eth0
ip addr add 192.168.20.1/24 dev eth1
ip link set eth0 up
ip link set eth1 up
sysctl -w net.ipv4.ip_forward=1
```

Evidence:

- [08-alpine-router-interface-config.png](images/08-alpine-router-interface-config.png)

At that point the router had:

- `eth0` on `192.168.10.0/24`
- `eth1` on `192.168.20.0/24`
- forwarding enabled so it could move traffic between the two networks

### 7. Prove default gateway behavior and routed communication

The final validation came from the VPCS endpoints.

From PC1:

- gateway `192.168.10.1` responded
- remote host `192.168.20.10` responded
- TTL dropped to `63`, which is consistent with a single routed hop
- ARP showed the MAC for the default gateway, not the remote host

From PC2:

- gateway `192.168.20.1` responded
- remote host `192.168.10.10` responded

Evidence:

- [09-pc1-routing-success-and-arp.png](images/09-pc1-routing-success-and-arp.png)
- [10-pc2-routing-success-and-arp.png](images/10-pc2-routing-success-and-arp.png)

This is the core lesson of the repo:

- local traffic is delivered directly on the local subnet
- remote traffic is sent to the default gateway
- the router forwards traffic between different IPv4 networks

## What This Lab Demonstrates

- Installing and integrating a GNS3 VM on a Windows Hyper-V host
- Troubleshooting controller connectivity before building labs on top of a broken platform
- Correcting GNS3 node-template issues instead of masking them
- Distinguishing Layer 2 reachability from Layer 3 routing
- Using VPCS as a fast way to practice IP addressing, ICMP, and ARP behavior
- Using a simple Linux router to validate default gateway concepts without needing a commercial router image
- Reading TTL changes and ARP tables as supporting evidence, not just relying on a green topology

## Problems Encountered

- GNS3 controller connectivity was inconsistent until the local server settings and startup behavior were corrected.
- VPCS nodes initially failed because the VPCS executable path was missing.
- Mixed compute placement caused link-creation errors between nodes on `MAIN-PC` and `GNS3 VM`.
- The Alpine router did not behave like a traditional persistent router appliance, so runtime configuration awareness mattered during stop/start testing.

## What I Learned

- A lab platform can fail in ways that look like networking problems but are actually controller, template, or compute-placement issues.
- Same-subnet communication is a useful baseline test because it removes routing from the equation.
- ARP is one of the clearest ways to explain the difference between local delivery and routed delivery.
- Building a minimal router from a Linux container is enough to prove default gateway and forwarding behavior when no packaged router image is available.

## Outcome

This repo captures a real progression:

- platform bring-up
- controller troubleshooting
- endpoint template repair
- same-subnet connectivity validation
- router-template creation
- cross-subnet routing success

That makes it useful as portfolio evidence for entry-level networking, help desk, desktop support, or junior infrastructure roles where practical troubleshooting matters more than just memorizing commands.
