---
title: Dynamic On-Demand Tunnels
tags: [formatting]
keywords: Dynamic Tunnels, On-Demand
last_updated: Dec 8, 2020
summary: "Configuring Dynamic On-Demand Tunnels between Site 30 and Site 40 with DC as the backup route"
sidebar: mydoc_sidebar
permalink: mydoc_dt.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Exploring the current setup
<br/>
- Configuring a Control Policy for Dynamic Tunnels
<br/>
- Configuring OMP Templates
<br/>
- Enabling Dynamic Tunnels
<br/>
- Activity Verification
<br/>
" type="primary" %}

## Overview
IPSEC tunnels are established between TLOCs in a full mesh fashion between devices in the SD-WAN overlay. This leads to multiple, potentially idle tunnels remaining up between sites and an overhead of traffic traversing the WAN links (due to BFD).

With version 20.3 of vManage, Cisco SD-WAN allows the creation of on-demand tunnels between sites - i.e. tunnels will only be set up when there is traffic traversing the sites.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Exploring the current setup](#exploring-the-current-setup)
<br/>
- [Configuring a Control Policy for Dynamic Tunnels](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [Configuring OMP Templates](#configuring-omp-templates)
<br/>
- [Enabling Dynamic Tunnels](#enabling-dynamic-tunnels)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Exploring the current setup

1. 


<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Exploring the current setup~~](#exploring-the-current-setup)
<br/>
- [Configuring a Control Policy for Dynamic Tunnels](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [Configuring OMP Templates](#configuring-omp-templates)
<br/>
- [Enabling Dynamic Tunnels](#enabling-dynamic-tunnels)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Configuring a Control Policy for Dynamic Tunnels

1. On the vManage GUI, navigate to **Monitor => Network**. Click on **DC-vEdge1** and then on **Real Time**. Enter *OSPF Neighbors* in the **Device Options** and choose *Do Not Filter*, if prompted. You should see 2 OSPF Neighbors (Central Gateway and DC-vEdge2)

    ![](/images/DC-vEdge_ConfiguringOSPF/18_ospfneigh.PNG)

2. Enter *OSPF Routes* in the **Device Options** and choose *Do Not Filter* if prompted

    ![](/images/DC-vEdge_ConfiguringOSPF/19_routednf.PNG)

3. You should see a Route for the *10.0.0.1/32* network, among others

    ![](/images/DC-vEdge_ConfiguringOSPF/20_ospfroute.PNG)

4. The same information can be verified via the CLI. Log in to DC-vEdge1 and issue `show ospf neigh`, `show ospf route` and `show ip route ospf`

    ![](/images/DC-vEdge_ConfiguringOSPF/21_cliospfneigh.PNG)

    ![](/images/DC-vEdge_ConfiguringOSPF/22_osfproutecli.PNG)

    ![](/images/DC-vEdge_ConfiguringOSPF/23_ospfroute.PNG)
    ```
    show ospf neigh
    show ospf route
    show ip route ospf
    ```

5. Log in to the CLI of **vEdge-30** and issue a `show ip route`. You will notice that a route to *10.0.0.1/32* has been learnt via OMP. Intra-Area and Inter-Area routes are injected into OMP by default

    ![](/images/DC-vEdge_ConfiguringOSPF/24_ve30iproyute.PNG)
    ```
    show ip route
    ```

6. Issue `ping 10.0.0.1 vpn 10` from vEdge30 to verify connectivity with the advertised LAN side route at the DC. The pings should be successful

    ![](/images/DC-vEdge_ConfiguringOSPF/25_ping.PNG)

This completes the OSPF configuration and verification of connectivity at the DC site.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Exploring the current setup~~](#exploring-the-current-setup)
<br/>
- [~~Configuring a Control Policy for Dynamic Tunnels~~](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [Configuring OMP Templates](#configuring-omp-templates)
<br/>
- [Enabling Dynamic Tunnels](#enabling-dynamic-tunnels)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Configuring OMP Templates

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Exploring the current setup~~](#exploring-the-current-setup)
<br/>
- [~~Configuring a Control Policy for Dynamic Tunnels~~](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [~~Configuring OMP Templates~~](#configuring-omp-templates)
<br/>
- [Enabling Dynamic Tunnels](#enabling-dynamic-tunnels)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Enabling Dynamic Tunnels

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Exploring the current setup~~](#exploring-the-current-setup)
<br/>
- [~~Configuring a Control Policy for Dynamic Tunnels~~](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [~~Configuring OMP Templates~~](#configuring-omp-templates)
<br/>
- [~~Enabling Dynamic Tunnels~~](#enabling-dynamic-tunnels)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Activity Verification

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Exploring the current setup~~](#exploring-the-current-setup)
<br/>
- [~~Configuring a Control Policy for Dynamic Tunnels~~](#configuring-a-control-policy-for-dynamic-tunnels)
<br/>
- [~~Configuring OMP Templates~~](#configuring-omp-templates)
<br/>
- [~~Enabling Dynamic Tunnels~~](#enabling-dynamic-tunnels)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>
" type="primary" %}
