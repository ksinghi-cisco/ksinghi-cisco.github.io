---
title: Dynamic Service Side Routing at Site 40
tags: [formatting]
keywords: eigrp, service side routing, site 40, cedge40
last_updated: May 26, 2020
summary: "Implementing Dynamic Service Side routing at Site 40 - EIGRP"
sidebar: mydoc_sidebar
permalink: mydoc_eigrp_site40.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Updating the cEdge Service VPN 10 with an EIGRP Template
<br/>
- Activity Verification and Remediation
<br/>
" type="primary" %}

## Overview

We will run EIGRP on VPN 10 in Site 40 with an L3 Device. The L3 device has been configured with the corresponding EIGRP configuration. Once EIGRP neighbourship is established between the L3 Device and cEdge40, we will try to reach a route being advertised by the L3 Device (*10.40.11.0/24*) from the DC-vEdges.

Given below is the section of the topology that we will be working on for this activity

![](/images/Site40_ConfiguringEIGRP/99_topo.png)

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Updating the cEdge Service VPN 10 with an EIGRP Template](#updating-the-cedge-service-vpn-10-with-an-eigrp-template)
<br/>
- [Activity Verification and Remediation](#activity-verification-and-remediation)
<br/>
" type="primary" %}

## Updating the cEdge Service VPN 10 with an EIGRP Template

1. Go to **Configuration => Templates** and click on the three dots next to *cEdge_dualuplink_devtemp*. Click on **Edit**

    ![](/images/Site40_ConfiguringEIGRP/01_edit.PNG)

2. Under **Service VPN**, click on the three dots next to the *cedge-vpn10* template and choose to **Edit** it

    ![](/images/Site40_ConfiguringEIGRP/02_editssv.PNG)

3. Click on **EIGRP** under **Additional Cisco VPN Templates** to add an EIGRP Template

    ![](/images/Site40_ConfiguringEIGRP/03_eigrptemp.PNG)

4. Click on the EIGRP drop down and click on **Create Template** to create a new EIGRP Template. We are creating our Templates on the fly over here, but could have created them before hand from the Feature Templates, if required

    ![](/images/Site40_ConfiguringEIGRP/04_createtemp.PNG)

5. Give the template a name of *site40-eigrp* and a Description of *EIGRP Template for Site 40 cEdge*. Populate the **Autonomous System ID** as a Device Variable with a value of *eigrp_as_num*. Click on **New Redistribute** under the Unicast Address Family => Re-Distribute section

    ![](/images/Site40_ConfiguringEIGRP/05_eigrptemp.PNG)

6. No routes get redistributed into EIGRP but we want to ensure that WAN Routes are advertised into the Site 40 LAN. For this purpose, choose **OMP** and click on **Add**. This will redistribute OMP routes into EIGRP

    ![](/images/Site40_ConfiguringEIGRP/06_redis.PNG)

7. Under the Unicast Address Family section, click on the **Network** tab. Click on **New Network** and Enter a Global Network Prefix of *10.40.10.0/24*. Click on **Add**

    ![](/images/Site40_ConfiguringEIGRP/07_nw.PNG)

8. Under **Interface**, click on *Interface* to add a new one. Enter the **Interface Name** as *GigabitEthernet4* and click on **Add**. This is our LAN facing interface in VPN 10 on cEdge40

    ![](/images/Site40_ConfiguringEIGRP/08_int.PNG)

9. Make sure the EIGRP template looks like the image given below and click on **Save** to save the template

    ![](/images/Site40_ConfiguringEIGRP/09_save.PNG)

10. This should take you back to the *cedge-vpn10* Template configuration window. Populate the *site40-eigrp* template in the EIGRP field. Click on **Save**

    ![](/images/Site40_ConfiguringEIGRP/10_saveagain.PNG)

11. Make sure that the VPN 10 Service VPN has *Cisco VPN Interface Ethernet, EIGRP* tacked on to it and click on **Update**

    ![](/images/Site40_ConfiguringEIGRP/11_upd.PNG)

12. We are taken to the configuration page for the cEdge40. Enter the Autonomous System ID as *40* and click on **Next**

    ![](/images/Site40_ConfiguringEIGRP/12_as.PNG)

13. Review the side-by-side config diff (notice the EIGRP configuration added) and click on **Configure Devices**.

    ![](/images/Site40_ConfiguringEIGRP/13_confdif.PNG)

This completes the EIGRP related configuration on VPN 10 for the Site 40 cEdge.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Updating the cEdge Service VPN 10 with an EIGRP Template~~](#updating-the-cedge-service-vpn-10-with-an-eigrp-template)
<br/>
- [Activity Verification and Remediation](#activity-verification-and-remediation)
<br/>
" type="primary" %}

## Activity Verification and Remediation

1. Log in to the CLI of cEdge40 via Putty. The username and password are `admin`. Enter `show ip eigrp vrf 10 40 neighbors` to view the EIGRP neighbours in VPN 10, AS 40. We will see one neighbour (the L3 Device)

    ![](/images/Site40_ConfiguringEIGRP/14_eigrpnei.PNG)
    ```
    show ip eigrp vrf 10 40 neighbors
    ```

2. Run `show ip route vrf 10` - you should see a *10.40.11.0/24* route learnt via EIGRP

    ![](/images/Site40_ConfiguringEIGRP/15_routes.PNG)
    ```
    show ip route vrf 10
    ```

3. Log in via Putty to **DC-vEdge1** and try to ping an IP in the *10.40.11.0/24* network. Type `ping vpn 10 10.40.11.1` - the pings should fail. Issue `show ip route vpn 10` and you will notice that there is no route for the *10.40.11.0/24* subnet

    ![](/images/Site40_ConfiguringEIGRP/16_dcve1noroute.PNG)
    ```
    ping vpn 10 10.40.11.1
    show ip route vpn 10
    ```

4. This is due to the fact that EIGRP routes aren't advertised into OMP. To remedy this, we will need to modify our cEdge Template. Go to **Configuration => Templates => Feature tab** and click on the three dots next to *cedge-vpn10*. Choose to **Edit**

    ![](/images/Site40_ConfiguringEIGRP/17_editvpn10.PNG)

5. Navigate to the **Advertise OMP** section and set EIGRP to Global - **On**. Click on **Update**

    ![](/images/Site40_ConfiguringEIGRP/18_adveigrp.PNG)

6. Click **Next** on the Device page since we don't have to update any values. Note that this change will be pushed to multiple devices, even those that don't have EIGRP configured (e.g. Site 50 Devices). We need to make sure that this change is pushed to the Site 40 cEdge

    ![](/images/Site40_ConfiguringEIGRP/19_next.PNG)

7. Check the side-by-side configuration, noting that EIGRP routes will now be advertised into OMP. Click on **Configure Devices**

    ![](/images/Site40_ConfiguringEIGRP/20_conf.PNG)

8. Confirm the change (pushed to 3 devices) and click on OK

    ![](/images/Site40_ConfiguringEIGRP/21_confirm.PNG)

9. Wait for the change to successfully go through

    ![](/images/Site40_ConfiguringEIGRP/22_succ.PNG)

10. Once successful, go to the CLI for **DC-vEdge1** and issue `show ip route vpn 10` again. You should see routes for *10.40.11.0/24*

    ![](/images/Site40_ConfiguringEIGRP/23_dcveroute.PNG)
    ```
    show ip route vpn 10
    ```

11. Run a ping to *10.40.11.1* via the CLI `ping vpn 10 10.40.11.1`. It should be successful

    ![](/images/Site40_ConfiguringEIGRP/24_pingsucc.PNG)
    ```
    ping vpn 10 10.40.11.1
    ```

This completes the EIGRP verification and remediation activity.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Updating the cEdge Service VPN 10 with an EIGRP Template~~](#updating-the-cedge-service-vpn-10-with-an-eigrp-template)
<br/>
- [~~Activity Verification and Remediation~~](#activity-verification-and-remediation)
<br/>
" type="primary" %}
