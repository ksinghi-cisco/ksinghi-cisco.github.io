---
title: Network Details
sidebar: mydoc_sidebar
datatable: true
permalink: mydoc_introduction.html
folder: mydoc
---

## Lab Topology

Given below is the lab topology being used for the SWAT SD-WAN Labs

{% include note.html content="There might be minor differences in the topology being used versus what you see here. We will keep this updated as far as possible" %}

![](/images/Topology_Schema/SD-WAN_Lab_23_07_2020.PNG)

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

| Device               | Access Method             |  Username | Password   | IP Address/URL |
|----------------------|---------------------------|----------|------------|---|
| vManage              | Browser - GUI             | admin    | admin      | 192.168.0.6 |
| vEdges and cEdges    | Putty                     | admin    | admin      | Various |
| Central Gateway      | Putty                     | admin    | admin      | 192.168.0.1 |
| Ubuntu - Site 40 PC  | vCenter Console | sdwan    | C1sco12345 | 10.40.30.21 |
| Ubuntu - Site 50 PC  | vCenter Console | sdwan    | C1sco12345 | 10.50.10.21 |
| Jumphost             | RDP/Guacamole             | admin    | C1sco12345 | 10.2.1.22X <br> <br> X is your POD number |
| vCenter | Browser - GUI             | sdwanpodX <br><br> X is your POD number <br> <br> *e.g. sdwanpod5*   | C1sco12345     | 10.2.1.50 |
| Site 30 AD <br> <br> Domain: swatsdwanlab.com  | RDP/Guacamole | administrator   | C1sco12345 | 10.30.10.50 |
| Site 30 PC  | RDP/Guacamole | swatsdwanlab\sdwan    | C1sco12345 | 10.30.10.21 |
| Cisco Umbrella  | Browser - GUI | ghi.pod0X@gmail.com <br> <br> X is your POD number    | C1sco@12345 | login.umbrella.com |
| Guacamole  | Browser - GUI | sdwanpod | C1sco12345 | 10.2.1.20X:8080/guacamole <br> <br> X is your POD number |
| WAAS Central Manager | Browser - GUI             | admin    | admin      | 10.40.30.123 |

<div class="datatable-end"></div>

## Network schema

{% include important.html content="Needless to say, these are super important and the IP Addressing scheme should be followed as enumerated in the lab guide" %}

Use the following table to copy-paste IP Addresses as and when required through the course of the lab. There is a search function which is super handy - search with the name of the VM you are looking for so as to return complete results.

* If the POD assigned to you is in location SLC

    * y (in the table below) is 1
    * X is your POD number

<br>

* If the POD assigned to you is in location GHI

    * y (in the table below) is 2
    * X is your POD number

VM names need to be used accordingly.

<div class="datatable-begin"></div>

| VM TAG <br> FOR IDENTIFICATION ONLY <br> NOT USED IN THE LAB | SITE ID | SYSTEM ID     | VM Name                     | Network Adapter   | Network                 | Interface        | IP                 | Gateway       |
|----------------------------------------------------------------|---------|---------------|-----------------------------|-------------------|-------------------------|------------------|--------------------|---------------|
| A <br> vManage                                                | 1000    | 10.255.255.1  | sdwan-slc/ghi-vmanage-podX  | Network Adapter 1 | Management              | eth1             | 192.168.0.6/24     | 192.168.0.1   |
| A <br> vManage                                                |         |               |                             | Network Adapter 2 | Internet                | eth0             | 100.100.100.2/24   | 100.100.100.1 |
| B <br> vBond                                                  |         | 10.255.255.2  | sdwan-slc/ghi-vbond-podX    | Network Adapter 1 | Management              | eth1             | 192.168.0.7/24     | 192.168.0.1   |
| B <br> vBond                                                  |         |               |                             | Network Adapter 2 | Internet                | eth0             | 100.100.100.3/24   | 100.100.100.1 |
| C <br> vSmart                                                 |         | 10.255.255.3  | sdwan-slc/ghi-vsmart-podX   | Network Adapter 1 | Management              | eth1             | 192.168.0.8/24     | 192.168.0.1   |
| C <br> vSmart                                                 |         |               |                             | Network Adapter 2 | Internet                | eth0             | 100.100.100.4/24   | 100.100.100.1 |
| D <br> vSmart2                                                |         | 10.255.255.4  | sdwan-slc/ghi-vsmart2-podX  | Network Adapter 1 | Management              | eth1             | 192.168.0.9/24     | 192.168.0.1   |
| D <br> vSmart2                                                |         |               |                             | Network Adapter 2 | Internet                | eth0             | 100.100.100.5/24   | 100.100.100.1 |
| E <br> DC-vEdge1                                              | 1       | 10.255.255.11 | DC-vEdge1-podX              | Network Adapter 1 | Management              | eth0             | 192.168.0.10/24    | 192.168.0.1   |
| E <br> DC-vEdge1                                              |         |               |                             | Network Adapter 2 | MPLS10                  | ge0/1            | 192.0.2.2/30       | 192.0.2.1     |
| E <br> DC-vEdge1                                              |         |               |                             | Network Adapter 3 | SiteDC_VPN10            | ge0/2            | 10.100.10.2/24     | 10.100.10.1   |
| E <br> DC-vEdge1                                              |         |               |                             | Network Adapter 4 | SiteDC-VPN20            | ge0/3            | 10.100.20.2/24     | 10.100.20.1   |
| E <br> DC-vEdge1                                              |         |               |                             | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.10/24  | 100.100.100.1 |
| F <br> DC-vEdge2                                              |         | 10.255.255.12 | DC-vEdge2-podX              | Network Adapter 1 | Management              | eth0             | 192.168.0.11/24    | 192.168.0.1   |
| F <br> DC-vEdge2                                              |         |               |                             | Network Adapter 2 | MPLS11                  | ge0/1            | 192.0.2.6/30       | 192.0.2.5     |
| F <br> DC-vEdge2                                              |         |               |                             | Network Adapter 3 | SiteDC_VPN10            | ge0/2            | 10.100.10.3/24     | 10.100.10.1   |
| F <br> DC-vEdge2                                              |         |               |                             | Network Adapter 4 | SiteDC-VPN20            | ge0/3            | 10.100.20.3/24     | 10.100.20.1   |
| F <br> DC-vEdge2                                              |         |               |                             | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.11/24  | 100.100.100.1 |
| G <br> vEdge20                                                | 20      | 10.255.255.21 | vEdge20-podX                | Network Adapter 1 | Management              | eth0             | 192.168.0.20/24    | 192.168.0.1   |
| G <br> vEdge20                                                |         |               |                             | Network Adapter 2 | TLOCEXT_vEDGE           | ge0/1            | 192.168.25.20/24   |               |
| G <br> vEdge20                                                |         |               |                             | Network Adapter 3 | Site20-VPN10            | ge0/2            | 10.20.10.2/24      |               |
| G <br> vEdge20                                                |         |               |                             | Network Adapter 4 | Site20-VPN20            | ge0/3            | 10.20.20.2/24      |               |
| G <br> vEdge20                                                |         |               |                             | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.20/24  | 100.100.100.1 |
| G <br> vEdge20                                                |         |               |                             | Network Adapter 6 | TLOCEXT2_vEdge          | ge0/4            | 192.168.26.20/24   |               |
| H <br> vEdge21                                                |         | 10.255.255.22 | vEdge21-podX                | Network Adapter 1 | Management              | eth0             | 192.168.0.21/24    | 192.168.0.1   |
| H <br> vEdge21                                                |         |               |                             | Network Adapter 2 | TLOCEXT_vEDGE           | ge0/1            | 192.168.25.21/24   |               |
| H <br> vEdge21                                                |         |               |                             | Network Adapter 3 | Site20-VPN10            | ge0/2            | 10.20.10.3/24      |               |
| H <br> vEdge21                                                |         |               |                             | Network Adapter 4 | Site20-VPN20            | ge0/3            | 10.20.20.3/24      |               |
| H <br> vEdge21                                                |         |               |                             | Network Adapter 5 | MPLS20                  | ge0/0            | 192.0.2.10/30      | 192.0.2.9     |
| H <br> vEdge21                                                |         |               |                             | Network Adapter 6 | TLOCEXT2_vEdge          | ge0/4            | 192.168.26.21/24   |               |
| I <br> vEdge30                                                | 30      | 10.255.255.31 | vEdge30-podX                | Network Adapter 1 | Management              | eth0             | 192.168.0.30/24    | 192.168.0.1   |
| I <br> vEdge30                                                |         |               |                             | Network Adapter 2 | MPLS30                  | ge0/1            | 192.0.2.14/30      | 192.0.2.13    |
| I <br> vEdge30                                                |         |               |                             | Network Adapter 3 | Site30-VPN10            | ge0/2            | 10.30.10.2/24      |               |
| I <br> vEdge30                                                |         |               |                             | Network Adapter 4 | Site30-VPN20            | ge0/3            | 10.30.20.2/24      |               |
| I <br> vEdge30                                                |         |               |                             | Network Adapter 5 | Internet                | ge0/0            | 100.100.100.30/24  | 100.100.100.1 |
| J <br> cEdge40                                                | 40      | 10.255.255.41 | cEdge40-podX                | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.40/24    | 192.168.0.1   |
| J <br> cEdge40                                                |         |               |                             | Network Adapter 2 | Internet                | GigabitEthernet2 | 100.100.100.40     | 100.100.100.1 |
| J <br> cEdge40                                                |         |               |                             | Network Adapter 3 | MPLS40                  | GigabitEthernet3 | 192.1.2.18/30      | 192.1.2.17    |
| J <br> cEdge40                                                |         |               |                             | Network Adapter 4 | Site40-VPN10            | GigabitEthernet4 | 10.40.10.2/24      |               |
| J <br> cEdge40                                                |         |               |                             | Network Adapter 5 | Site40-VPN20            | GigabitEthernet5 | 10.40.20.2/24      |               |
| J <br> cEdge40                                                |         |               |                             | Network Adapter 6 | Site40-VPN30            | GigabitEthernet6 | 10.40.30.2/24      |               |
| K <br> cEdge50                                                | 50      | 10.255.255.51 | cEdge50-podX                | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.50/24    | 192.168.0.1   |
| K <br> cEdge50                                                |         |               |                             | Network Adapter 2 | Internet                | GigabitEthernet2 | 100.100.100.50/24  | 100.100.100.1 |
| K <br> cEdge50                                                |         |               |                             | Network Adapter 3 | Site50-VPN10            | GigabitEthernet3 | 10.50.10.2/24      |               |
| K <br> cEdge50                                                |         |               |                             | Network Adapter 4 | Site50-VPN20            | GigabitEthernet4 | 10.50.20.2/24      |               |
| K <br> cEdge50                                                |         |               |                             | Network Adapter 5 | Site50-VPN30            | GigabitEthernet5 | 10.50.30.2/24      |               |
| L <br> cEdge51                                                |         | 10.255.255.52 | cEdge51-podX                | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.51/24    | 192.168.0.1   |
| L <br> cEdge51                                                |         |               |                             | Network Adapter 2 | MPLS50                  | GigabitEthernet2 | 192.1.2.22/30      | 192.1.2.21    |
| L <br> cEdge51                                                |         |               |                             | Network Adapter 3 | Site50-VPN10            | GigabitEthernet3 | 10.50.10.3/24      |               |
| L <br> cEdge51                                                |         |               |                             | Network Adapter 4 | Site50-VPN20            | GigabitEthernet4 | 10.50.20.3/24      |               |
| L <br> cEdge51                                                |         |               |                             | Network Adapter 5 | Site50-VPN30            | GigabitEthernet5 | 10.50.30.3/24      |               |
| M <br> WAAS                                                   | NA      | NA            | sdwan-slc/ghi-wcm-podX      | Network Adapter 1 | Site40-VPN30            | Virtual 1/0      | 10.40.30.123/24    | 10.40.30.1    |
| N <br> WAAS                                                   |         |               | sdwan-slc/ghi-waa40-podX    | Network Adapter 1 | Site40-VPN30            | Virtual 1/0      | 10.40.30.46/24     | 10.40.30.1    |
| O <br> WAAS                                                   |         |               | sdwan-slc/ghi-waa50-podX    | Network Adapter 1 | Site50-VPN30            | Virtual 1/0      | 10.50.30.46/24     | 10.50.30.1    |
| P <br> Central GW                                             | NA      | NA            | sdwan-slc/ghi-gw-podX       | Network Adapter 1 | Management              | GigabitEthernet1 | 192.168.0.1        |               |
| P <br> Central GW                                             |         |               |                             | Network Adapter 2 | WAN-Trunk               | GigabitEthernet2 | All DGs point here |               |
| P <br> Central GW                                             |         |               |                             | Network Adapter 3 | Shared_Services_VLAN101 | GigabitEthernet3 | 10.y.1.24X/24      | 10.y.1.1      |
| Q <br> Guacamole                                              | NA      | NA            | sdwan-slc/ghi-guac-podX     | Network Adapter 1 | Shared_Services_VLAN101 | eth0             | 10.y.1.20X/24      | 10.y.1.1      |
| R <br> Jumphost                                               | NA      | NA            | sdwan-slc/ghi-jump-podX     | Network Adapter 1 | Shared_Services_VLAN101 | eth0             | 10.y.1.22X/24      | 10.y.1.1      |
| S <br> Site 40 PC                                             | 40      | NA            | sdwan-slc/ghi-site40pc-podX | Network Adapter 1 | Site40-VPN30            | eth0             | 10.40.30.21/24     | 10.40.30.2    |
| T <br> Site 50 PC                                             | 50      | NA            | sdwan-slc/ghi-site50pc-podX | Network Adapter 1 | Site50-VPN10            | eth0             | 10.50.10.21/24     | 10.50.10.100  |
| U <br> Site 30 AD | 30 | NA | sdwan-slc/ghi-ad-podX       | Network Adapter 1 | Site30-VPN10            | eth0 | 10.30.10.50/24 | 10.30.10.2 |
| U <br> Site 30 AD |    |    |                             | Network Adapter 2 | Shared_Services_VLAN101 | eth1 | 10.2.1.18X     |            |
| V <br> Site 30 PC | 30 | NA | sdwan-slc/ghi-site30pc-podX | Network Adapter 1 | Site30-VPN10            | eth0 | 10.30.10.21/24 | 10.30.10.2 |
| V <br> Site 30 PC |    |    |                             | Network Adapter 2 | Shared_Services_VLAN101 | eth1 | 10.2.1.16X     |            |

<div class="datatable-end"></div>

<br>

[Click here](https://github.com/swat-sdwanlab/swat-sdwanlab.github.io/blob/gh-pages/images/Topology_Schema/NetworkSchema_09_07_2020.PNG){:target="_blank"} to download a printable version of this table, for reference.
