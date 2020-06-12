---
title: Configuring Application Aware Routing
tags: [special_layouts]
keywords: knowledge base, support portal, grid, doc portal
last_updated: June 3, 2020
summary: "Manipulate the path taken by traffic based on network parameters  like latency, loss and jitter."
sidebar: mydoc_sidebar
permalink: mydoc_aar.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Creating and Activating the AAR Policy
<br/>
- Viewing modified traffic flows and current network statistics
<br/>
- Configuring a Policer to simulate network impairment
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating a Policer List
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the IPv4 ACL Policy
    <br/>
- Applying the Policer on the MPLS link
<br/>
- Viewing changed statistics and resultant traffic flows
<br/>

" type="primary" %}

## Overview

While we can use Traffic Engineering to steer traffic towards a particular preferred transport, Application Aware Routing takes things to a different level by not only allowing us to punt traffic over a preferred path, but also define SLA parameters for traffic to be redirected if network conditions aren't favourable for the type of traffic.

To set a baseline, we will first see how traffic flows on VPN 10 (let's assume that this VPN has Voice traffic in it). We will then implement AAR and SLA Classes to route traffic out a preferred transport and switch the chosen transport if SLA parameters are not met.

To check existing traffic flows, follow the steps below:

1. Navigate to **Monitor -> Network** and select **cEdge40** from the list. Scroll down on the left-hand side and click on **Troubleshooting**. Choose **Simulate Flows**. Choose a VPN of *VPN - 10* and a Source/Interface of *GigabitEthernet4*. Enter the Destination IP as *10.100.10.2* and click on **Simulate**. Notice that traffic is attempting to use all available transports

    ![](/images/AAR_LLQ/01_path.PNG)

2. Click on **Advanced Options** and enter the DSCP value as 46 (i.e. VoIP RTP traffic). Click on **Simulate**. This traffic also uses all possible transports, which might not be ideal for our network

    ![](/images/AAR_LLQ/02_path46.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Creating and Activating the AAR Policy](#creating-and-activating-the-aar-policy)
<br/>
- [Viewing modified traffic flows and current network statistics](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Creating and Activating the AAR Policy

We will now set up an AAR Policy for VoIP (i.e. DSCP 46) traffic.

1. On the vManage GUI, go to **Configuration -> Policies** and click **Add Policy**. Click on **Next** twice (till you get to the Configure Traffic Rules page) and click on **Add Policy** under Application Aware Routing. We thus have an overarching Policy (let's call it the Main Policy) and an application-aware routing policy within it. As of now, we will configure the AAR routing policy. Towards the end, we will enter the details of the Main Policy

    ![](/images/AAR_LLQ/03_pol_next2_add.PNG)

2. Give this AAR Policy a name of *VPN10-AAR* and a Description of *Transport Preference for Traffic in VPN 10*. Click on **Sequence Type** and then click on **Sequence Rule**. Under Match, select DSCP and enter a DSCP value of **46** under Match Conditions

    ![](/images/AAR_LLQ/04_aarpol.PNG)

3. Click on the Actions tab and choose **SLA Class List**. Click on the box under SLA Class and choose **New SLA Class List**

    ![](/images/AAR_LLQ/05_newsla.PNG)

4. Give the SLA Class a Name of *Voice-SLA* and specify the **Loss %** as *1*. Enter *200* for the **Latency** and *15* for the **Jitter**. Click on **Save**

    ![](/images/AAR_LLQ/06_vsla.PNG)

5. Still under actions, select the *Voice-SLA* SLA Class that we just created and set the Preferred Color to *mpls*. Click on **Save Match and Actions**

    ![](/images/AAR_LLQ/07_actsave.PNG)

6. Ensure your App Route looks like the image below and click on **Save Application Aware Routing Policy**. Click **Next**

    ![](/images/AAR_LLQ/08_save.PNG)

7. At the **Apply Policies to Sites and VPNs** page, give the Policy a Name of *AAR-VPN10* and a Description of *Transport Preference for VPN 10*. Click on the Application Aware Routing tab and click on **New Site List and VPN List**. Under **Select Site List** choose *Branches* and *DC*. Under **Select VPN List** choose *Corporate*. Click on *Add*

    ![](/images/AAR_LLQ/09_name.PNG)

8. Click on **Save Policy** in the lower middle part of the screen to save our AAR Policy

    ![](/images/AAR_LLQ/10_savepol.PNG)

9. Click on the three dots next to the *AAR-VPN10* policy we just created and choose to **Activate** it. Click on **Activate** again

    ![](/images/AAR_LLQ/11_act.PNG)

    ![](/images/AAR_LLQ/12_actconf.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [Viewing modified traffic flows and current network statistics](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Viewing modified traffic flows and current network statistics

To view the changes made by the Policy on our network, follow the steps below.

1. On the vManage GUI, go to **Monitor -> Network** and click on cEdge40. Choose **Troubleshooting** from the left-hand column and click on **Simulate Flows**. Enter the VPN as *VPN - 10* and the Source/Interface as *GigabitEthernet4*. Set a Destination IP of *10.100.10.2* and click on **Simulate**. We find that traffic is taking all possible transports, just like before. This is expected since we haven't defined anything for regular traffic

    ![](/images/AAR_LLQ/13_regtraf.PNG)

2. On the same screen, click on **Advanced Options** and set the DSCP to *46*. Click on **Simulate**

    ![](/images/AAR_LLQ/14_dscptraf.PNG)

    VoIP Traffic is now traversing the MPLS link as the preferred route. We see just one path in the image above - if 10.255.255.11 (DC-vEdge1) is rebooted and this tested again, traffic will go through 10.255.255.12 (DC-vEdge2). If Simulate is clicked multiple times, traffic does take different paths.

3. We will now check the current network statistics. On the left-hand side go to **Tunnel** and put a check mark against all the *mpls* Tunnel Endpoints. Click on Real-Time after scrolling up to the chart and make sure Packet Loss/Latency is checked under **Chart Options**. We may see negligible packet loss occurring (let the chart run for 5 minutes before analysing, it should get updated every few seconds)

    ![](/images/AAR_LLQ/16_minorloss.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Configuring a Policer to simulate network impairment

In order to simulate impairment in the network (Packet Loss and Latency), we can use a Policer and a Shaper. Over here, we will configure a Policer which will be applied to the MPLS link in order to simulate Packet Loss.

Later on, we will leverage a Shaper to simulate Latency.

### Creating a Policer List

1. On the vManage GUI, navigate to **Configuration -> Policies**. Click on **Custom Options** (top right-hand corner). Under **Localized Policy** click on **Lists**

    ![](/images/AAR_LLQ/17_lppolicer.PNG)

2. Click on **Policer** (left-hand side) to create Policer configuration which will simulate network impairment on our MPLS link (Packet Loss). Click on **New Policer List** and give it a name of *AAR-Impair-Policer-PL*. Specify the **Burst** as *15000* and **Exceed** as *Drop*. The **Rate** should be *7000*. Click on **Add**

    | Field             | Value                 |
    |-------------------|-----------------------|
    | Policer List Name | AAR-Impair-Policer-PL |
    | Burst (bps)       | 15000                 |
    | Exceed            | Drop                  |
    | Rate (bps)        | 7000                  |

    ![](/images/AAR_LLQ/18_polic.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

### Configuring the IPv4 ACL Policy

1. Go to the **Localized Policy** tab and click on **Add Policy**

    ![](/images/AAR_LLQ/19_addpol.PNG)

2. Click **Next** till you are at the **Configure Access Control Lists** page. Click on **Add Access Control List Policy** and choose **Add IPv4 ACL Policy**

    ![](/images/AAR_LLQ/20_acl.PNG)

3. Enter a name of *Impair-PL-AAR* with a Description of *Impairment ACL*. Click on **Add ACL Sequence** and click on **Sequence Rule**. Go to the **Actions** tab and make sure the Accept radio button is selected. Choose **Policer** and select the *AAR-Impair-Policer-PL* we created before. Click on **Save Match and Actions**. Refer to the table and image below

    | Step | Field                  | Value                 |
    |------|------------------------|-----------------------|
    | 1    | Name                   | Impair-PL-AAR         |
    |      | Description            | Impairment ACL        |
    | 2    | Add ACL Sequence       |                       |
    | 3    | Sequence Rule          |                       |
    | 4    | Actions                |                       |
    | 5    | Policer                |                       |
    | 6    | Policer                | AAR-Impair-Policer-PL |
    | 7    | Save Match and Actions |                       |

    ![](/images/AAR_LLQ/21_polacl.PNG)

4. Click on **Save Access Control List Policy**

    ![](/images/AAR_LLQ/22_save.PNG)

5. On the **Policy Overview** page (this is our Main Policy), enter a Policy Name of *Policer-AAR-Impairment* and a Description of Injecting Impairment for AAR via a Policer - Packet Loss. Click on **Save Policy**

    ![](/images/AAR_LLQ/23_savepol.PNG)

We have completed configuration of our Policer. It needs to be applied to a link in order to simulate network impairment.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Applying the Policer on the MPLS link

1. Navigate to **Configuration -> Templates -> Feature Tab** and locate the *cedge-vpn0-int-dual_mpls* VPN Interface template. Click on the 3 dots next to it and choose to **Copy**

    ![](/images/AAR_LLQ/26_copyint.PNG)

2. Rename it t0 *cedge-vpn0-int-dual_mpls-impair* and a Description *cEdge VPN 0 Interface Template for Devices with a dual uplink - MPLS with Impairment*. Click on **Copy**

    ![](/images/AAR_LLQ/27_copyname.PNG)

3. Click on the three dots next to this newly copied template and click on **Edit**

    ![](/images/AAR_LLQ/28_edit.PNG)

4. Navigate to the ACL/QoS section and modify the following fields. Click on **Update**

    | Field                    | Global or Device Specific (drop down) | Value         |
    |--------------------------|---------------------------------------|---------------|
    | Ingress ACL - IPv4       | Global                                | On            |
    | IPv4 Ingress Access List | Global                                | Impair-PL-AAR |
    | Egress ACL - IPv4        | Global                                | On            |
    | IPv4 Egress Access List  | Global                                | Impair-PL-AAR |

    ![](/images/AAR_LLQ/29_aclupd.PNG)

5. Under **Configuration -> Templates** go to the **Device** tab and locate the *cedge_dualuplink_devtemp* template. Click on the three dots next to it and choose to **Edit**

    ![](/images/AAR_LLQ/30_editdevtemp.PNG)

6. Under Transport & Management VPN, update the **Cisco VPN Interface Ethernet** from *cedge-vpn0-int-dual_mpls* to *cedge-vpn0-int-dual_mpls-impair*. Make sure this is done on the VPN interface for the MPLS link

    ![](/images/AAR_LLQ/31_mplsimpair.PNG)

7. Scroll down to the **Additional Templates** section and update the **Policy** to *Policer-AAR-Impairment*. Click on **Update**. Click on **Next**

    ![](/images/AAR_LLQ/25_update_next_confdev.PNG)

8. You can choose to view the Side by Side or simply click on **Configure Devices**

    ![](/images/AAR_LLQ/32_sbs.PNG)

This completes the implementation of our Policer on the MPLS link to simulate network impairment.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [~~Applying the Policer on the MPLS link~~](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Viewing changed statistics and resultant traffic flows

1. Navigate to **Monitor -> Network** and click on cEdge40. Click on **Tunnel** on the left-hand side and make sure all the **MPLS** Tunnel Endpoint entries are selected, with the public-internet entries being unchecked. Click on **Real Time** (top right corner) and the Chart Options drop-down (top left corner) is set to Loss Percentage/FEC Loss Recovery Rate. Let this run for a few minutes - you will notice a spike in Packet Loss

    ![](/images/AAR_LLQ/33_pl.PNG)

2. Head over to **Troubleshooting** (left-hand side, might need to scroll down) and click on **Simulate Flows**. Enter the **VPN** as *VPN - 10*, the **Source/Interface** as *GigabitEthernet4* and the **Destination IP** as *10.100.10.2*. Click on **Simulate**. There should be no change in traffic flow for General traffic, which will still use all available transports

    ![](/images/AAR_LLQ/34_regtrafsame.PNG)

3. Under **Advanced Options**, set DSCP to a value of *46* and click on **Simulate**. You will notice that VoIP traffic (i.e. DSCP 46) is now taking the Internet path since MPLS doesn't conform to the SLA requirements that we defined. Compare the current traffic flow with the one in Step 2 [over here](#viewing-modified-traffic-flows-and-current-network-statistics)

    ![](/images/AAR_LLQ/35_plaar.PNG)

4. We will now revert the configuration to what it was pre-impairment. Go to **Configuration -> Templates** and locate the *cEdge_dualuplink_devtemp*. Click on the three dots next to it and **Edit**. Change the Cisco VPN Interface Ethernet value under **Transport & Management VPN** back to *cedge-vpn0-int-dual_mpls* and click on **Update**

    ![](/images/AAR_LLQ/36_rev.PNG)

5. Wait for approximately 3 minutes and head over to **Monitor -> Network -> cEdge40 -> Troubleshooting -> Traffic Flows**. Enter the same details as in Step 3 above and click on **Simulate**. VoIP traffic should traverse over the MPLS link again

    ![](/images/AAR_LLQ/37_waitmpls.PNG)

This completes the Application Aware Routing section of the lab.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [~~Applying the Policer on the MPLS link~~](#applying-the-policer-on-the-mpls-link)
<br/>
- [~~Viewing changed statistics and resultant traffic flows~~](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}
