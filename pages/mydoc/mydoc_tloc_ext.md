---
title: TLOC Extensions at Site 20
tags: [single_sourcing]
keywords: includes, conref, dita, transclusion, transclude, inclusion, reference
last_updated: July 3, 2016
summary: "Configuring TLOC Extensions for transport redundancy."
sidebar: mydoc_sidebar
permalink: mydoc_tloc_ext.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Feature Templates for TLOC Extensions
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN Interface Template for the TLOC-EXT interface
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN Interface Template for the Tunnel interface
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the BGP Template for the MPLS link
    <br/>
- Updating the VPN and Device Templates
<br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview
A number of sites have a couple of routers in place, but transport connectivity to just one of the available transports. In the event of a link failure, there is no mechanism for traffic to be redirected over the other transport. That's where TLOC Extensions come in.

TLOC Extensions allow vEdge/cEdge routers with a single transport to utilize the link on another vEdge/cEdge router at the same site. Given below is a graphical representation of what we're trying to achieve in this section of the lab.

![](/images/TLOC_Ext/tloc_ext.PNG)

vEdge20 is connected to the Internet transport whereas vEdge21 is connected to MPLS. If the Internet link goes down, vEdge20 doesn't have a way to utilize the MPLS link available at vEdge21. TLOC Extensions seek to remedy this.

vEdge/cEdge routers build IPSec tunnels across directly connected transports AND across the transport connected to the neighbouring vEdge/cEdge router to facilitate transport redundancy.

Without TLOC Extensions, the vEdges at Site 20 look something like the images below. Note that both have control connections to the vSmarts and vManage via the directly connected transport

![](/images/TLOC_Ext/01_cc.PNG)

BFD sessions are established across the directly connected transport as well.

![](/images/TLOC_Ext/02_bfd.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the TLOC-EXT interface](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the Tunnel interface](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Feature Templates for TLOC Extensions

We will need to create a total of three Feature Templates for this section which will be applied to vEdge20 and vEdge21 Device Templates.

Towards the end of the lab, we will copy and modify the VPN 0 feature template used by the INET interface on vEdge20 to allow for NAT. Both vEdges at Site20 use the same feature template for VPN 0 ge0/0 so making a change on one will impact the other as well. Hence, we will be breaking off the vEdge20 VPN Interface template from the one being used. This new template will be identical to the VPN 0 interface template being used at this Site, except for NAT being enabled on ge0/0.

### Creating the VPN Interface Template for the TLOC-EXT interface

1. On the vManage GUI, click on **Configuration -> Templates** and go to the **Feature** tab. click on **Add Template** and search for *vedge*. Select *vEdge Cloud* from the list and choose **VPN Interface Ethernet** to create an Interface Template

    ![](/images/TLOC_Ext/03_addtemp.PNG)

2. Enter the details as shown in the table below. Use the images for reference. Click on **Save** once done

    | Section             | Field          | Global or Device Specific (drop down) | Value                                                          |
    |---------------------|----------------|---------------------------------------|----------------------------------------------------------------|
    |                     | Template Name  | NA                                    | *Site20_TLOC_Ext_NoTunn*                                       |
    |                     | Description    | NA                                    | *Site 20 TLOC Extension Template without Tunnel Configuration* |
    | Basic Configuration | Shutdown       | Global                                | No                                                             |
    | Basic Configuration | Interface Name | Device Specific                       | *if_name_notunn_tlocext*                                       |
    | Basic Configuration | IPv4 Address   | Device Specific                       | *if_ipv4_address_notunn*                                       |
    | Advanced            | TLOC Extension | Global                                | ge0/0                                                          |

    ![](/images/TLOC_Ext/04_notunn.PNG)

    ![](/images/TLOC_Ext/05_tesave.PNG)

This completes configuration of the VPN Interface Template for TLOC Extension interfaces, without a Tunnel. Each participating vEdge/cEdge will have an interface that will not have a Tunnel associated with it (but will have a TLOC Extension association) and another one which will have a Tunnel (but won't have a TLOC Extension associated with it).

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the Tunnel interface](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the VPN Interface Template for the Tunnel interface

1. Navigate to **Configuration -> Templates -> Feature tab** and search for *tloc*. You should get one template (the one we just created). Click on the three dots next to it and choose **Copy**

    ![](/images/TLOC_Ext/06_copy.PNG)

2. Rename the Template to *Site20_Tunn_no_tlocext* with a Description of *Site 20 Template with Tunnel Configuration no TLOC-Ext*. Clcik on **Copy**

    ![](/images/TLOC_Ext/07_naem.PNG)

3. Click on the three dots next to the newly created template and choose to **Edit**

    ![](/images/TLOC_Ext/08_edit.PNG)

4. Update the details as in the table below. Click on **Update** when done



<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the BGP Template for the MPLS link

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Updating the VPN and Device Templates

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [~~Updating the VPN and Device Templates~~](#updating-the-vpn-and-device-templates)
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
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [~~Updating the VPN and Device Templates~~](#updating-the-vpn-and-device-templates)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
