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

    VoIP Traffic is now traversing the mpls link as the preferred route.

3. We will now check the current network statistics. On the left-hand side go to **Tunnel** and put a check mark against all the *mpls* Tunnel Endpoints. Click on Real-Time after scrolling up to the chart and make sure Packet Loss/Latency is checked under **Chart Options**. We may see negligible packet loss occurring (let the chart run for some time before beginning to analyze)

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

### Creating a Policer List

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
