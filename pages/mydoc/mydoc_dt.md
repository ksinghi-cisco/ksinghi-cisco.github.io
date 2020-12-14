---
title: Dynamic On-Demand Tunnels
tags: [formatting]
keywords: Dynamic Tunnels, On-Demand
last_updated: Dec 8, 2020
sidebar: mydoc_sidebar
summary: "Configuring Dynamic On-Demand Tunnels between Site 30 and Site 40 with DC as the backup route"
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

    ![](/images/DT/21.PNG)

12. We will notice that the control policy is setting the TLOC of Site 30 and Site 40 OMP Routes to the *DC-TLOCs* TLOC list. It is also setting a **tloc-action backup** to populate the *ultimate tloc* value in the OMP route, pointing to the other site TLOC (rather than punting traffic out the DC-TLOCs). Click on **Save Policy**

This completes the Control Policy required for Dynamic On-Demand Tunnels.

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

We will be applying OMP Templates to the vSmarts and the WAN Edges at Site 30 and Site 40.

1. On the vManage GUI, go to **Configuration => Templates**

    ![](/images/DT/22.PNG)

2. Click on the **Feature** tab and then click on **Add Template**

    ![](/images/DT/23.PNG)

3. Search for *vSmart* in the **Select Devices** section and select the vSmart Device. Click on **OMP** under Basic Configuration to start configuring an OMP Template for the vSmarts

    ![](/images/DT/24.PNG)

4. Give the template a name of *vsmart-omp-dt* with a Description of *OMP modification for Dynamic Tunnels - vSmart*. Set the **Number of Paths Advertised per Prefix** to a Global value of *16* and click on **Save**

    ![](/images/DT/25.PNG)

5. We will now apply this Feature Template to the vSmart Device Template. Go to the Device tab in Templates and locate the *vSmart-dev-temp* Device Template. Click on the three dots next to it and choose to **Edit** the template

    ![](/images/DT/26.PNG)

6. Under **OMP**, set the template to *vsmart-omp-dt*. Click on **Update**. Click on **Next** and **Configure Devices**

    ![](/images/DT/27.PNG)

    ![](/images/DT/28.PNG)

    ![](/images/DT/29.PNG)

7. Confirm the configuration change and click on **OK**

    ![](/images/DT/30.PNG)

8. Navigate to **Configuration => Templates => Feature Tab** and click on **Add Template**

    ![](/images/DT/34.PNG)

9. Search for *vedge* and select **vEdge Cloud**. Click on **OMP**

    ![](/images/DT/35.PNG)

10. Give the template a name of *vedge-omp-dt* with a Description of *OMP modification for Dynamic Tunnels - vEdge*. Set the **ECMP Limit** to a Global value of *16* and click on **Save**

    ![](/images/DT/36.PNG)

11. Navigate to **Configuration => Templates => Feature Tab** and click on **Add Template**. Search for *csr* and select **CSR1000v**. Click on **Cisco OMP**

    ![](/images/DT/37.PNG)

12. Give the template a name of *cedge-omp-dt* with a Description of *OMP modification for Dynamic Tunnels - cEdge*. Set the **ECMP Limit** to a Global value of *16* and click on **Save**

    ![](/images/DT/38.PNG)

13. We will now attach the OMP templates just created to **vEdge30** and **cEdge40**. Navigate to **Configuration => Templates**. While on the Device Tab, locate the *vEdge30_dev_temp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/DT/39.PNG)

14. Update the **OMP** template as *vedge-omp-dt* and click on **Update**. Click **Next** and **Configure Devices** to push the changes to vEdge30

    ![](/images/DT/40.PNG)

15. Navigate to **Configuration => Templates**. While on the Device Tab, locate the *cEdge_dualuplink_devtemp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/DT/41.PNG)

16. Update the **Cisco OMP** template as *cedge-omp-dt* and click on **Update**. Click **Next** and **Configure Devices** to push the changes to cEdge40

    ![](/images/DT/42.PNG)

    ![](/images/DT/43.PNG)

This completes the configuration of our OMP Feature Templates for vEdge30 and cEdge40 to support Dynamic On-Demand Tunnels.


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

We will now add some basic configuration on the DC-vEdges and enable Dynamic On-Demand Tunnels via System templates.

1. Navigate to **Configuration => Templates => Feature Tab** and locate the *DCvEdge-vpn0* Feature Template. Click on the three dots next to it and choose to **Edit** the template

    ![](/images/DT/31.PNG)

2. Scroll down to the **Service** section and click on **New Service**. Set the Service Type as *TE* and click on **Add**. Click on **Update**. Click on **Next** and **Configure Devices**. Confirm the configuration change

    ![](/images/DT/32.PNG)

    ![](/images/DT/33.PNG)

3. On the vManage GUI, go to **Configuration => Templates**. Click on the **Feature** tab and then click on **Add Template**. Search for *vedge* in the **Select Devices** section and select the vEdge Cloud. Click on **System** under Basic Configuration to start configuring a System Template for vEdge30

    ![](/images/DT/44.PNG)

4. Give the template a name of *vedge-system-dt* with a Description of *System modification for Dynamic Tunnels - vEdge*. Under Advanced, set **On-Demand Tunnel** to a Global value of *On* and the **On-Demand Tunnel Idle Timeout (min)** to *5*. Click on **Save**

    ![](/images/DT/45.PNG)

    ![](/images/DT/46.PNG)

5. Go to **Configuration => Templates**. Click on the **Feature** tab and then click on **Add Template**. Search for *csr* in the **Select Devices** section and select the CSR1000v. Click on **Cisco System** under Basic Configuration to start configuring a System Template for cEdge40

    ![](/images/DT/47.PNG)

6. Give the template a name of *cedge-system-dt* with a Description of *System modification for Dynamic Tunnels - cEdge*. Under Advanced, set **On-Demand Tunnel** to a Global value of *On* and the **On-Demand Tunnel Idle Timeout (min)** to *5*. Click on **Save**

    ![](/images/DT/48.PNG)

    ![](/images/DT/49.PNG)

7. We will now attach the System templates just created to **vEdge30** and **cEdge40**. Navigate to **Configuration => Templates**. While on the Device Tab, locate the *vEdge30_dev_temp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/DT/50.PNG)

8. Update the **System** template as *vedge-system-dt* and click on **Update**. Click **Next** and **Configure Devices** to push the changes to vEdge30

    ![](/images/DT/51.PNG)

9. Navigate to **Configuration => Templates**. While on the Device Tab, locate the *cEdge_dualuplink_devtemp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/DT/52.PNG)

10. Update the **Cisco System** template as *cedge-system-dt* and click on **Update**. Click **Next** and **Configure Devices** to push the changes to cEdge40

    ![](/images/DT/53.PNG)

This completes the configuration of our System Feature Templates for vEdge30 and cEdge40 to enable Dynamic On-Demand Tunnels.


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

1. Log in to the CLI of *DC-vEdge1* and *DC-vEdge2* using the saved Putty session (or SSH to 192.168.0.10 and 192.168.0.11, respectively). Use the credentials given below. Issue `clear control connections` on both devices

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/DT/54.PNG)

2. Log in to the CLI of *vEdge30* using the saved Putty session (or SSH to 192.168.0.30). Use the same credentials as above and issue `show omp tlocs | tab`. Notice that the TLOC Routes for cEdge40 are learnt by vEdge30, but they are in an inactive state

    ![](/images/DT/55.PNG)

3. Run the commands `show system on-demand` and `show system on-demand remote-system` on vEdge30. You will notice that vEdge30 shows itself as On-Demand *yes* and Status *Active*. However, the Status of cEdge40 is *inactive*

    ![](/images/DT/56.PNG)

4. Run the command `show omp routes | tab` on vEdge30. Notice that the OMP Routes for the VPN 10 subnet at cEdge40 (10.40.10.0/24) are in an Unresolved, On-Demand Inactive state (U,IA)

    ![](/images/DT/58.PNG)

4. On the vManage GUI, navigate to **Configuration => Policies** and locate the *Dynamic-Tunnels-Site30_40* policy. Click on the three dots next to it and choose to Activate this policy. Click on **Activate** and **Configure Devices** if prompted

    ![](/images/DT/57.PNG)

5. Once the policy is active, go to the CLI of vEdge30 and run `show omp routes | tab` again. We now see that the traffic to the VPN 10 subnet at cEdge40 (10.40.10.0/24) is being routed via the DC-vEdges, with the direct routes to cEdge40 in an Installed, Unresolved and On-Demand Inactive state (I,U,IA)

    ![](/images/DT/59.PNG)

6. Log in to the CLI of vEdge30 and run a Traceroute to 10.40.10.2 via the CLI `traceroute VPN 10 10.40.10.2`. We will see that the initial path will traverse an IP in VPN 10 at the DC-vEdges (10.100.10.3 in this example) and will then start going directly to cEdge40. This is because the initial packet takes the backup DC-vEdge route after which the Tunnel between vEdge30 and cEdge40 is established. Run `show system on-demand` and `show system on-demand remote` and we will see that the Tunnel to cEdge40 is now active, with the Idle timeout counting down from 300 seconds (i.e. 5 minutes, as we had configured in the System Template)

    ![](/images/DT/61.PNG)

7. Subsequent traffic will go directly over the Tunnel between vEdge30 adn cEdge40, as long as the Tunnel is active. This can be verified by running `traceroute vpn 10 10.40.10.2` on vEdge30

    ![](/images/DT/62.PNG)

8. `show omp routes 10.40.10.0/24` indicates that the Chosen, Installed, Resolved (C,I,R) route for the 10.40.10.0 subnet is the direct path to cEdge40

    ![](/images/DT/63.PNG)

9. Wait for approximately 5 minutes and we will find that the Tunnel between vEdge30 and cEdge40 transitions to an *inactive* state after the Idle Timeout expires, assuming there is no traffic between the two Sites

    ![](/images/DT/66.PNG)

10. Once the tunnel is inactive, `show omp routes 10.40.10.0/24` shows the traffic path traversing the DC-vEdges again, with the direct path to cEdge40 in I,U,IA

    ![](/images/DT/67.PNG)

This completes the configuration and verification of Dynamic On-Demand Tunnels.

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
