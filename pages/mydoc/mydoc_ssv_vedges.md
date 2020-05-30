---
title: Service Side VPN configuration - vEdges
tags: [formatting]
keywords: Service Side VPN, vEdges, Service VPN
last_updated: May 26, 2020
summary: "Configure the Service Side VPNs for the vEdges at DC, Site 20 and Site 30"
sidebar: mydoc_sidebar
permalink: mydoc_ssv_vedges.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Configuring the vEdge VPN 10 Feature Templates
<br/>
- Configuring the vEdge VPN 20 Feature Template
<br/>

" type="primary" %}

## Configuring the vEdge VPN 10 Feature Templates

We are now going to set up the Service Side VPNs for our vEdges. The process is very similar to what we've done in the past, and many of the tasks are repetitive in nature.

1. Click on **Configuration -> Templates -> Feature Tab**

    ![](/images/Service_vEdges_Templates/01_tempf.PNG)

2. Choose to add a new Template. Search for *ve* and choose the vEdge Cloud. Select the Template as a **VPN Template**

    ![](/images/Service_vEdges_Templates/02_addtempvpn.PNG)

3. Populate the details as below. Click on **Save** once done

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                        |
    |---------------------------|-----------------------|---------------------------------------|------------------------------|
    |                           | Template Name         | NA                                    | *vedge-vpn10*                |
    |                           | Description           | NA                                    | *VPN 10 Template for vEdges* |
    | Basic Configuration       | VPN                   | Global                                | 10                           |
    | DNS | Primary DNS Address   | Global                                | 10.2.1.5                     |
    | DNS | Secondary DNS Address | Global                                | 10.2.1.6                     |
    | Advertise OMP             | Static (IPv4)         | Global                                | On                           |
    | Advertise OMP             | Connected (IPv4)      | Global                                | On                           |

    ![](/images/Service_vEdges_Templates/03_vpndet.PNG)

    ![](/images/Service_vEdges_Templates/04_advompsave.PNG)

    This creates the VPN template for VPN 10. We will make a copy of this template and create an almost identical template for VPN 20 later on.

4. We now create the vEdge VPN 10 Interface Template. While on the **Configuration -> Templates -> Feature Tab** page, click on **Add Template** and search for *ve*. Choose the Device as vEdge Cloud and the Template as **VPN Interface Ethernet**

    ![](/images/Service_vEdges_Templates/09_vpnint.PNG)

5. Enter the details as shown below and click on **Save** to create the VPN 10 Interface Feature Template

    | Section             | Field          | Global or Device Specific (drop down) | Value                                  |
    |---------------------|----------------|---------------------------------------|----------------------------------------|
    |                     | Template Name  | NA                                    | *vedge-vpn10-int*                      |
    |                     | Description    | NA                                    | *VPN 10 Interface Template for vEdges* |
    | Basic Configuration | Shutdown       | Global                                | No                                     |
    | Basic Configuration | Interface Name | Device Specific                       | *vpn10_if_name*                        |
    | Basic Configuration | IPv4 Address   | Device Specific                       | *vpn10_if_ipv4_address*                |

    ![](/images/Service_vEdges_Templates/10_intconf.PNG)

We have finished creating the vEdge VPN 10 Feature Templates needed for Service Side VPNs.

<br/>
{% include callout.html content="**Task List**
<br/><br/>
- [~~Configuring the vEdge VPN 10 Feature Templates~~](#configuring-the-vedge-vpn-10-feature-templates)
<br/>
- [Configuring the vEdge VPN 20 Feature Template](#configuring-the-vedge-vpn-20-feature-templates)
<br/>

" type="primary" %}

## Configuring the vEdge VPN 20 Feature Templates

1. Locate the *vedge-vpn10* template created and click on the three dots next to it. Choose to **Copy** the template. Rename the template to *vedge-vpn20* with a Description of *VPN 20 Template for vEdges*. Click on **Copy**

    ![](/images/Service_vEdges_Templates/05_copyvpn10.PNG)

    ![](/images/Service_vEdges_Templates/06_ren.PNG)

2. Choose to edit the newly created *vedge-vpn20* template. Make sure the Description is updated and change the VPN field to 20. Click on **Update**

    ![](/images/Service_vEdges_Templates/07_edit.PNG)

    ![](/images/Service_vEdges_Templates/08_descvpnupda.PNG)

3. At the Feature Templates page, locate the *vedge-vpn10-int* Template and click on the 3 dots next to it. Choose to **Copy** the template. Name the copied template *vedge-vpn20-int* with a Description of *VPN 20 Interface Template for vEdges*. Click on **Copy**

    ![](/images/Service_vEdges_Templates/11_copyvpn10int.PNG)

4. Locate the newly created *vedge-vpn20-int* Template and click on the three dots next to it. Choose to **Edit**. Update the **Description**, **Interface Name** and **IPv4 Address** to reflect vpn20 instead of vpn10, as shown below and click on **Update**

    ![](/images/Service_vEdges_Templates/12_vpn20int.PNG)

This completes the configuration of the vEdge VPN 20 Feature Templates for Service Side VPNs.

<br/>
{% include callout.html content="**Task List**
<br/><br/>
- [~~Configuring the vEdge VPN 10 Feature Templates~~](#configuring-the-vedge-vpn-10-feature-templates)
<br/>
- [~~Configuring the vEdge VPN 20 Feature Template~~](#configuring-the-vedge-vpn-20-feature-templates)
<br/>

" type="primary" %}
