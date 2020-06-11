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

We will now set up an AAR Policy for VoIP traffic.

1. On the vManage GUI, go to **Configuration -> Policies** and click **Add Policy**. Click on **Next** twice (till you get to the Configure Traffic Rules page) and click on **Add Policy** under Application Aware Routing. We thus have an overarching Policy (let's call it the Main Policy) and an application-aware routing policy within it. As of now, we will configure the AAR routing policy. Towards the end, we will enter the details of the Main Policy

    ![](/images/AAR_LLQ/03_pol_next2_add.PNG)

2. Give this AAR Policy a name of *VPN10-AAR* and a Description of *Transport Preference for Traffic in VPN 10*. Click on **Sequence Type**
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
