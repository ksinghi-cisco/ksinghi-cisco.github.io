---
title: Network Details
sidebar: mydoc_sidebar
permalink: mydoc_introduction.html
folder: mydoc
---

## Lab Topology

Given below is the lab topology being used for the SWAT SD-WAN Labs

{% include note.html content="There might be minor differences in the topology being used versus what you see here. We will keep this updated as far as possible" %}

![](/images/Topology_Schema/SD-WAN_Lab_13_06_2020.PNG)

Decoding the toplogy:

* There are a total of 5 sites where we will have cEdges/vEdges deployed

* All sites have Service VPNs associated with them.

  * Sites with vEdges have 2 service VPNs (VPN10 and VPN20)

  * Sites with cEdges have 3 service VPNs (VPN10, VPN20 and VPN30)

* Some devices have dual uplinks (MPLS and Internet) while others have single uplinks (MPLS only or Internet only)

* Site DC (Site ID 1) is running OSPF on the LAN. Site 50 is running EIGRP on the LAN

* Site 20 will have TLOC Extensions set up and we will be peering with the MPLS side via eBGP

* cEdge40 and cEdge50 will function as AppNav-XE Controllers

## Device Credentials

Given below are the access details for some key devices in the network

<div class="datatable-begin"></div>

| Device               | Access Method             |  Username | Password   | IP Address |
|----------------------|---------------------------|----------|------------|---|
| vManage              | Browser - GUI             | admin    | admin      | 192.168.0.6 |
| vEdges and cEdges    | Putty                     | admin    | admin      | Various |
| Central Gateway      | Putty                     | admin    | admin      | 192.168.0.1 |
| Ubuntu - Site 40 PC  | vCenter Console/Guacamole | sdwan    | C1sco12345 | 10.40.30.21 |
| Ubuntu - Site 50 PC  | vCenter Console/Guacamole | sdwan    | C1sco12345 | 10.50.10.21 |
| Jumphost             | RDP/Guacamole             | admin    | C1sco12345 | POD Dependent |
| WAAS Central Manager | Browser - GUI             | admin    | admin      | 10.40.30.123 |

<div class="datatable-end"></div>

## Network schema

{% include important.html content="Needless to say, these are super important and the IP Addressing scheme should be followed as enumerated in the lab guide" %}

Use the following table to copy-paste IP Addresses as and when required through the course of the lab.

<div class="datatable-begin"></div>

| SITE ID | SYSTEM ID     | VM                           | Network Adapter   | Network                 | Interface        | IP                 | Gateway       |
|---------|---------------|------------------------------|-------------------|-------------------------|------------------|--------------------|---------------|
| 1000    | 10.255.255.1  | vManage                      | Network Adapter 1 | Management              | eth1             | 192.168.0.6/24     | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | eth0             | 100.100.100.2/24   | 100.100.100.1 |
|         | 10.255.255.2  | vBond                        | Network Adapter 1 | Management              | eth1             | 192.168.0.7/24     | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | eth0             | 100.100.100.3/24   | 100.100.100.1 |
|         | 10.255.255.3  | vSmart                       | Network Adapter 1 | Management              | eth1             | 192.168.0.8/24     | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | eth0             | 100.100.100.4/24   | 100.100.100.1 |
|         | 10.255.255.4  | vSmart2                      | Network Adapter 1 | Management              | eth1             | 192.168.0.9/24     | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | eth0             | 100.100.100.5/24   | 100.100.100.1 |
| 1       | 10.255.255.11 | DC-vEdge1                    | Network Adapter 1 | Management              | eth0             | 192.168.0.10/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | MPLS10                  | ge0/1            | 192.0.2.2/30       | 192.0.2.1     |
|         |               |                              | Network Adapter 3 | SiteDC_VPN10            | ge0/2            | 10.100.10.2/24      | 10.100.10.1    |
|         |               |                              | Network Adapter 4 | SiteDC-VPN20            | ge0/3            | 10.100.20.2/24      | 10.100.20.1    |
|         |               |                              | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.10/24  | 100.100.100.1 |
|         | 10.255.255.12 | DC-vEdge2                    | Network Adapter 1 | Management              | eth0             | 192.168.0.11/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | MPLS11                  | ge0/1            | 192.0.2.6/30       | 192.0.2.5     |
|         |               |                              | Network Adapter 3 | SiteDC_VPN10            | ge0/2            | 10.100.10.3/24      | 10.100.10.1    |
|         |               |                              | Network Adapter 4 | SiteDC-VPN20            | ge0/3            | 10.100.20.3/24      | 10.100.20.1    |
|         |               |                              | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.11/24  | 100.100.100.1 |
| 20      | 10.255.255.21 | vEdge20                      | Network Adapter 1 | Management              | eth0             | 192.168.0.20/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | TLOCEXT_vEDGE                  | ge0/1            |           192.168.25.20/24         |               |
|         |               |                              | Network Adapter 3 | Site20-VPN10            | ge0/2            | 10.20.10.1/24      |               |
|         |               |                              | Network Adapter 4 | Site20-VPN20            | ge0/3            | 10.20.20.1/24      |               |
|         |               |                              | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.20/24  | 100.100.100.1 |
|         |               |                              | Network Adapter 6 | TLOCEXT2_vEdge                  | ge0/4            |           192.168.26.20/24         |               |
|         | 10.255.255.22 | vEdge21                      | Network Adapter 1 | Management              | eth0             | 192.168.0.21/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | TLOCEXT_vEDGE                  | ge0/1            |      192.168.25.21/24              |               |
|         |               |                              | Network Adapter 3 | Site20-VPN10            | ge0/2            | 10.20.10.2/24      |               |
|         |               |                              | Network Adapter 4 | Site20-VPN20            | ge0/3            | 10.20.20.2/24      |               |
|         |               |                              | Network Adapter 5 | MPLS20                  | ge0/0            | 192.0.2.10/30      | 192.0.2.9     |
|         |               |                              | Network Adapter 6 | TLOCEXT2_vEdge                  | ge0/4            |           192.168.26.21/24         |               |
| 30      | 10.255.255.31 | vEdge30                      | Network Adapter 1 | Management              | eth0             | 192.168.0.30/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | MPLS30                  | ge0/1            | 192.0.2.14/30      | 192.0.2.13    |
|         |               |                              | Network Adapter 3 | Site30-VPN10            | ge0/2            | 10.30.10.1/24      |               |
|         |               |                              | Network Adapter 4 | Site30-VPN20            | ge0/3            | 10.30.20.1/24      |               |
|         |               |                              | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.30/24  | 100.100.100.1 |
| 40      | 10.255.255.41 | cEdge40                      | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.40/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | GigabitEthernet2 | 100.100.100.40     | 100.100.100.1 |
|         |               |                              | Network Adapter 3 | MPLS40                  | GigabitEthernet3 | 192.1.2.18/30      | 192.1.2.17    |
|         |               |                              | Network Adapter 4 | Site40-VPN10            | GigabitEthernet4 | 10.40.10.1/24      |               |
|         |               |                              | Network Adapter 5 | Site40-VPN20            | GigabitEthernet5 | 10.40.20.1/24      |               |
|         |               |                              | Network Adapter 6 | Site40-VPN30            | GigabitEthernet6 | 10.40.30.1/24      |               |
| 50      | 10.255.255.51 | cEdge50                      | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.50/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | Internet                | GigabitEthernet2 | 100.100.100.50/24  | 100.100.100.1 |
|         |               |                              | Network Adapter 3 | Site50-VPN10            | GigabitEthernet3 | 10.50.10.1/24      |               |
|         |               |                              | Network Adapter 4 | Site50-VPN20            | GigabitEthernet4 | 10.50.20.1/24      |               |
|         |               |                              | Network Adapter 5 | Site50-VPN30            | GigabitEthernet5 | 10.50.30.1/24      |               |
|         | 10.255.255.52 | cEdge51                      | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.51/24    | 192.168.0.1   |
|         |               |                              | Network Adapter 2 | MPLS50                  | GigabitEthernet2 | 192.1.2.22/30      | 192.1.2.21    |
|         |               |                              | Network Adapter 3 | Site50-VPN10            | GigabitEthernet3 | 10.50.10.2/24      |               |
|         |               |                              | Network Adapter 4 | Site50-VPN20            | GigabitEthernet4 | 10.50.20.2/24      |               |
|         |               |                              | Network Adapter 5 | Site50-VPN30            | GigabitEthernet5 | 10.50.30.2/24      |               |
| NA      | NA            | WAAS Central Manager         | Network Adapter 1 | Site40-VPN30            | Virtual 1/0      | 10.40.30.123/24    | 10.40.30.1    |
|         |               | WAAS Application Accelerator | Network Adapter 1 | Site40-VPN30            | Virtual 1/0      | 10.40.30.46/24     | 10.40.30.1    |
|         |               | WAAS Application Accelerator | Network Adapter 1 | Site50-VPN30            | Virtual 1/0      | 10.50.30.46/24     | 10.50.30.1    |
| NA      | NA            | CentralGateway               | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.1        |               |
|         |               |                              | Network Adapter 2 | WAN-Trunk               | GigabitEthernet2 | All DGs point here |               |
|         |               |                              | Network Adapter 3 | Shared_Services_VLAN101 | GigabitEthernet3 | 10.2.1.241/24      | 10.2.1.1      |
| NA      | NA            | Ubuntu - Guacamole           | Network Adapter 1 | Shared_Services_VLAN101 | eth0             | 10.2.1.120/24      | 10.2.1.1      |
| NA      | NA            | Windows - Jumphost           | Network Adapter 1 | Shared_Services_VLAN101 | eth0             | 10.2.1.221/24      | 10.2.1.1      |
| 40      | NA            | Ubuntu - Site40_PC           | Network Adapter 1 | Site40-VPN30            | eth0             | 10.40.30.21/24     | 10.40.30.2    |
| 50      | NA            | Ubuntu - Site50_PC           | Network Adapter 1 | Site50-VPN30            | eth0             | 10.50.30.21/24     | 10.50.30.2    |

<div class="datatable-end"></div>

Given below is a printable version of the table (Right click -> **Save Image As** and save it to a preferred location. Then open the image and print, if required)

![](/images/Topology_Schema/NetworkSchema_09_06_2020.PNG)
