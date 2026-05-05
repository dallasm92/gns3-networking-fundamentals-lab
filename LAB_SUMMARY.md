# Lab Summary

## Scope

This lab documents the first successful GNS3 environment bring-up and three foundational networking exercises:

- GNS3 VM and controller setup on Windows 11 Pro with Hyper-V
- same-subnet host connectivity through VPCS
- inter-subnet routing through a simple Alpine Linux router

## Key Results

- GNS3 local controller and GNS3 VM were both brought online successfully.
- VPCS node startup issues were fixed by setting the correct executable path.
- Two VPCS hosts communicated successfully on the same subnet.
- A lightweight Alpine router was built as a Docker template with two interfaces.
- Routed connectivity succeeded between:
  - `192.168.10.0/24`
  - `192.168.20.0/24`
- ARP behavior matched expectation:
  - local same-subnet communication resolved peer MAC addresses directly
  - routed communication resolved the default gateway MAC instead

## Evidence Highlights

- Initial platform bring-up:
  - [01-gns3-vm-download.png](images/01-gns3-vm-download.png)
  - [02-local-server-controller-settings.png](images/02-local-server-controller-settings.png)
  - [03-vm-server-selection.png](images/03-vm-server-selection.png)
- VPCS repair and first same-subnet success:
  - [04-vpcs-executable-path.png](images/04-vpcs-executable-path.png)
  - [05-two-host-same-subnet-ping.png](images/05-two-host-same-subnet-ping.png)
- Routed lab:
  - [08-alpine-router-interface-config.png](images/08-alpine-router-interface-config.png)
  - [09-pc1-routing-success-and-arp.png](images/09-pc1-routing-success-and-arp.png)
  - [10-pc2-routing-success-and-arp.png](images/10-pc2-routing-success-and-arp.png)

## Core Concepts Reinforced

- local controller vs VM-backed compute in GNS3
- IPv4 addressing and subnet boundaries
- same-subnet communication over Layer 2
- default gateway behavior for remote networks
- inter-subnet forwarding by a router
- ARP as evidence for local versus routed delivery
