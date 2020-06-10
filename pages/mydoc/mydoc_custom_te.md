---
title: Implementing Custom Traffic Engineering
tags: [publishing]
keywords: custom traffic engineering, path selection
last_updated: May 29, 2020
summary: "Influencing Path selection and facilitating custom traffic engineering in Cisco SD-WAN"
sidebar: mydoc_sidebar
permalink: mydoc_custom_te.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Deploying a Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Groups of Interest and Traffic Rules
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Applying and Activating the Policy
    <br/>
- Verification
<br/>

" type="primary" %}

## Overview

The Cisco SD-WAN solution builds a full mesh topology by default and there isn't any traffic engineering that is in place out of the box. The ability to steer application traffic per the network requirements via a specific path is something that can be achieved via data policies. We can leverage data policies to match specific traffic and send it via the preferred transport. To verify current functionality:

1. Log in to the vManage GUI and navigate to **Monitor -> Network**

    ![](/images/DataPol_TrafficEng/01_mon.PNG)

2. Click on **vEdge30** and scroll down the list on the left-hand side to **Troubleshooting**

    ![](/images/DataPol_TrafficEng/02_ve30.PNG)

    ![](/images/DataPol_TrafficEng/03_tshoot.PNG)

3. Click on **Simulate Flows**

    ![](/images/DataPol_TrafficEng/04_flow.PNG)

4. Enter *VPN - 10* as the VPN, *ge0/2* as the Source/Interface and *10.0.0.1* as the Destination IP. Click on **Simulate**

    ![](/images/DataPol_TrafficEng/05_sim.PNG)

    We find that general traffic uses all possible available transports to send data to the other side.

5. Keep all details the same, but this time choose **ftp** under Application. Click **Simulate**

    ![](/images/DataPol_TrafficEng/06_ftp.PNG)

    Once again, ftp traffic is also attempting to take all possible transports.

In our example, we will assume that the requirement is to send FTP traffic over the MPLS link (preferred).

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Setting up Groups of Interest and Traffic Rules](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Deploying a Policy

We begin by creating a Policy and identifying **Groups of Interest** (or interesting traffic). The policy is then expanded to encompass an ACL
### Setting up Groups of Interest and Traffic Rules

When you're done with the preview server, press **Ctrl+C** to exit out of it. If you exit iTerm or Terminal without shutting down the server, the next time you build your site, or if you build multiple sites with the same port, you may get a server-already-in-use message.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

### Applying and Activating the Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Verification

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [~~Verification~~](#verification)
<br/>

" type="primary" %}
