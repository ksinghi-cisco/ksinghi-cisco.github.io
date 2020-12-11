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

The following configuration components come into play when setting up Dynamic On-Demand Tunnels:

* Control Policies
* OMP Templates (max path and ECMP limits)
* System Templates (for configuring Dynamic Tunnels)

We will set up Dynamic On-Demand Tunnels between vEdge30 and cEdge40 with the DC-vEdges functioning as backup forwarding nodes.

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

1. Open a CLI session to vEdge30 using the saved session in Putty (or SSH to 192.168.0.30). Log in via the credentials mentioned below and enter the command `show omp tlocs | tab`. Notice that TLOC routes learnt from cEdge40 are Chosen, Installed and Resolved (C,I,R) or Chosen, Resolved (C,R)

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/DT/01.PNG)

    ```
    show omp tlocs | tab
    ```

2. Log in to cEdge40 via the saved session in Putty (or SSH to 192.168.0.40). Use the same credentials as above and enter the command `show sdwan omp tlocs`. Look for the TLOC route entries for 10.255.255.31 (vEdge30) and these are also Chosen, Installed and Resolved (C,I,R) or Chosen, Resolved (C,R)

    ![](/images/DT/02.PNG)
    ![](/images/DT/03.PNG)
    ![](/images/DT/04.PNG)
    ![](/images/DT/05.PNG)

    ```
    show sdwan omp tlocs
    ```

3. Back at vEdge30, check the OMP routes for VPN 10 and VPN 20 subnets behind cEdge40. Run the commands `show omp routes 10.40.10.0/24` and `show omp routes 10.40.20.0/24`. vEdge30 routes traffic for the subnets directly to cEdge40 (normal full mesh operation of SD-WAN)

    ![](/images/DT/06.PNG)

    ```
    show omp routes 10.40.10.0/24
    show omp routes 10.40.20.0/24
    ```

4. Similarly, cEdge40 routes traffic for the vEdge30 VPN 10 and VPN 20 subnets directly to vEdge30. Run the commands `show sdwan omp routes 10.30.10.0/24` and `show sdwan omp routes 10.30.20.0/24` on cEdge40

    ![](/images/DT/07.PNG)

    ```
    show sdwan omp routes 10.30.10.0/24
    show sdwan omp routes 10.30.20.0/24
    ```

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

1. On the vManage GUI, navigate to **Configuration => Policies**

    ![](/images/DT/08.PNG)

2. We will create a new policy for Dynamic On-Demand Tunnels. Click on **Add Policy**

    ![](/images/DT/09.PNG)

3. Click on **Site** and then on **New Site List** to create a New Site List

    ![](/images/DT/10.PNG)

4. Name the Site List *Site30_40* and enter *30,40* in the Add Site field. Click on **Add**

    ![](/images/DT/11.PNG)

5. Make sure the Site List looks like the image below and click on **Next**

    ![](/images/DT/12.PNG)

6. Click on **Add Topology** and then on **Custom Control (Route & TLOC)** to create a new control policy

    ![](/images/DT/13.PNG)

7. Give the control policy a **Name** of *site30-40-dynamic-tunnels* and a **Description** of *Dynamic Tunnels between Site 30 and 40 with DC as a backup*. Click on **Sequence Type** and choose **Route**

    ![](/images/DT/14.PNG)

8. Click on **Sequence Rule** and select **Site**. Populate the Site List *Site30_40* and click on **Actions**

    ![](/images/DT/15.PNG)

9. Set the Action to **Accept** and click on **TLOC Action** and **TLOC**. Populate TLOC Action as *Backup* and the TLOC List as *DC-TLOCs*. Click on **Save Match and Actions**

    ![](/images/DT/16.PNG)

10. Click on **Default Action** and then the pencil icon to change the default of Reject Enabled to Accept Enabled. Click on Accept and choose to Save. Make sure the Default Action is set to Accept Enabled and click on **Save Control Policy**

    ![](/images/DT/19.PNG)

11. Click **Next** till you're at the **Apply Policies to Sites and VPNs** tab and give the policy a Name of *Dynamic-Tunnels-Site30_40* with a Description of *Dynamic Tunnels between Site 30 and Site 40*. Under **Topology**, click on **New Site List** for the *site30-40-dynamic-tunnels* policy and choose the **Site30_40** Site List under **Outbound Site List**. Click on **Add** and then click on **Preview** to view the CLI output of the policy

    ![](/images/DT/20.PNG)

12. We will notice that the control policy is setting the TLOC of Site 30 and Site 40 OMP Routes to the *DC-TLOCs* TLOC list. 
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
