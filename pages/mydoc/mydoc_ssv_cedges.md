---
title: Configuring Service Side VPNs - cEdges
tags: [formatting]
keywords: notes, tips, cautions, warnings, admonitions
last_updated: July 3, 2016
summary: "Configure the Service Side VPNs for the cEdges at Sites 40 and 50"
sidebar: mydoc_sidebar
permalink: mydoc_ssv_cedges.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Configuring the cEdge VPN 10 Feature Templates
<br/>
- Configuring the cEdge VPN 20 Feature Templates
<br/>
- Configuring the cEdge VPN 30 Feature Templates
<br/>
" type="primary" %}

{% include important.html content="Most of the steps in this section are quite repetitive and very similar to the previous section where we configured the Service Side VPN Templates for the vEdges. Thus, the steps will be quite brief, augmented by images which can be used as reference points to complete this section. This will also serve as a way to increase familiarity with creating and managing Templates. " %}

## Configuring the cEdge VPN 10 Feature Templates

1. Create a new VPN Template by navigating to **Configuration -> Templates -> Feature Tab** and choosing to **Add Template**. Search for *csr* and select the CS1000V Device Type, along with selecting the **Cisco VPN** template

    ![](/images/Service_cEdges_Templates/01_add.PNG)

2. Populate the details in the Template as shown below and click on **Save**. This will create the VPN 10 Template for cEdges

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                        |
    |---------------------------|-----------------------|---------------------------------------|------------------------------|
    |                           | Template Name         | NA                                    | *cedge-vpn10*                |
    |                           | Description           | NA                                    | *VPN 10 Template for the cEdges* |
    | Basic Configuration       | VPN                   | Global                                | 10                           |
    | DNS | Primary DNS Address   | Global                                | 10.2.1.5                     |
    | DNS | Secondary DNS Address | Global                                | 10.2.1.6                     |
    | Advertise OMP             | Static (IPv4)         | Global                                | On                           |
    | Advertise OMP             | Connected (IPv4)      | Global                                | On                           |

    ![](/images/Service_cEdges_Templates/02_sett.PNG)

    ![](/images/Service_cEdges_Templates/03_ompsave.PNG)

3. We will now create the VPN 10 Interface Template for cEdges. While on the **Configuration -> Templates -> Feature Tab** page, click on **Add Template** and search for *csr*. Choose the Device as CSR1000v and the Template as **Cisco VPN Interface Ethernet**

    ![](/images/Service_cEdges_Templates/06_vpn10int.PNG)

4. Populate the details as shown below and click on **Save**

    | Section             | Field          | Global or Device Specific (drop down) | Value                                  |
    |---------------------|----------------|---------------------------------------|----------------------------------------|
    |                     | Template Name  | NA                                    | *cedge-vpn10-int*                      |
    |                     | Description    | NA                                    | *VPN 10 Interface Template for cEdges* |
    | Basic Configuration | Shutdown       | Global                                | No                                     |
    | Basic Configuration | Interface Name | Device Specific                       | *vpn10_if_name*                        |
    | Basic Configuration | IPv4 Address/ prefix-length   | Device Specific                       | *vpn10_if_ipv4_address*                |

    ![](/images/Service_cEdges_Templates/07_vpn10intconf.PNG)

This completes the configuration of the VPN 10 Feature Templates for the cEdges.


<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Configuring the cEdge VPN 10 Feature Templates~~](#configuring-the-cedge-vpn-10-feature-templates)
<br/>
- [Configuring the cEdge VPN 20 Feature Templates](#configuring-the-cedge-vpn-20-feature-templates)
<br/>
- [Configuring the cEdge VPN 30 Feature Templates](#configuring-the-cedge-vpn-30-feature-templates)
<br/>
" type="primary" %}

## Configuring the cEdge VPN 20 Feature Templates

As indicated before, creating the templates is a repetitive task so we will be going through pretty much the same steps as before, changing *vpn10* to *vpn20* wherever applicable.

1. Create a new VPN Template by navigating to **Configuration -> Templates -> Feature Tab** and choosing to **Add Template**. Search for *csr* and select the CS1000V Device Type, along with selecting the **Cisco VPN** template. Alternatively, you can create a copy of the *cedge-vpn10* template, rename it to *cedge-vpn20* and then edit the specifics clicking on **Update** to save the changes (followed in step 2 below).

    ![](/images/Service_cEdges_Templates/01_add.PNG)

2. Populate the details in the Template as shown below and click on **Save**. This will create the VPN 20 Template for cEdges

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                        |
    |---------------------------|-----------------------|---------------------------------------|------------------------------|
    |                           | Template Name         | NA                                    | *cedge-vpn20*                |
    |                           | Description           | NA                                    | *VPN 20 Template for the cEdges* |
    | Basic Configuration       | VPN                   | Global                                | 20                           |
    | DNS | Primary DNS Address   | Global                                | 10.2.1.5                     |
    | DNS | Secondary DNS Address | Global                                | 10.2.1.6                     |
    | Advertise OMP             | Static (IPv4)         | Global                                | On                           |
    | Advertise OMP             | Connected (IPv4)      | Global                                | On                           |

    ![](/images/Service_cEdges_Templates/04_vpn20.PNG)

3. We will now create the VPN 20 Interface Template for cEdges. While on the **Configuration -> Templates -> Feature Tab** page, click on **Add Template** and search for *csr*. Choose the Device as CSR1000v and the Template as **Cisco VPN Interface Ethernet**. Once again, alternatively, make a copy of the *cedge-vpn10-int* template and rename it to *cedge-vpn20-int*, updating the description. Then Edit this newly created template and **Update** (followed in step 4 below)

    ![](/images/Service_cEdges_Templates/06_vpn10int.PNG)

4. Populate the details as shown below and click on **Save**

    | Section             | Field          | Global or Device Specific (drop down) | Value                                  |
    |---------------------|----------------|---------------------------------------|----------------------------------------|
    |                     | Template Name  | NA                                    | *cedge-vpn20-int*                      |
    |                     | Description    | NA                                    | *VPN 20 Interface Template for cEdges* |
    | Basic Configuration | Shutdown       | Global                                | No                                     |
    | Basic Configuration | Interface Name | Device Specific                       | *vpn20_if_name*                        |
    | Basic Configuration | IPv4 Address/ prefix-length   | Device Specific                       | *vpn20_if_ipv4_address*                |

    ![](/images/Service_cEdges_Templates/08_vpn20intconf.PNG)

This completes the configuration of the VPN 20 Feature Templates for the cEdges.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Configuring the cEdge VPN 10 Feature Templates~~](#configuring-the-cedge-vpn-10-feature-templates)
<br/>
- [~~Configuring the cEdge VPN 20 Feature Templates~~](#configuring-the-cedge-vpn-20-feature-templates)
<br/>
- [Configuring the cEdge VPN 30 Feature Templates](#configuring-the-cedge-vpn-30-feature-templates)
<br/>
" type="primary" %}

## Configuring the cEdge VPN 30 Feature Templates

As indicated before, creating the templates is a repetitive task so we will be going through pretty much the same steps as before, changing *vpn10* to *vpn30* wherever applicable.

1. Create a new VPN Template by navigating to **Configuration -> Templates -> Feature Tab** and choosing to **Add Template**. Search for *csr* and select the CS1000V Device Type, along with selecting the **Cisco VPN** template. Alternatively, you can create a copy of the *cedge-vpn10* template, rename it to *cedge-vpn30* and then edit the specifics clicking on **Update** to save the changes (followed in step 2 below).

    ![](/images/Service_cEdges_Templates/01_add.PNG)

2. Populate the details in the Template as shown below and click on **Save**. This will create the VPN 30 Template for cEdges

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                        |
    |---------------------------|-----------------------|---------------------------------------|------------------------------|
    |                           | Template Name         | NA                                    | *cedge-vpn30*                |
    |                           | Description           | NA                                    | *VPN 30 Template for the cEdges* |
    | Basic Configuration       | VPN                   | Global                                | 30                           |
    | DNS | Primary DNS Address   | Global                                | 10.2.1.5                     |
    | DNS | Secondary DNS Address | Global                                | 10.2.1.6                     |
    | Advertise OMP             | Static (IPv4)         | Global                                | On                           |
    | Advertise OMP             | Connected (IPv4)      | Global                                | On                           |

    ![](/images/Service_cEdges_Templates/05_vpn30.PNG)

3. We will now create the VPN 30 Interface Template for cEdges. While on the **Configuration -> Templates -> Feature Tab** page, click on **Add Template** and search for *csr*. Choose the Device as CSR1000v and the Template as **Cisco VPN Interface Ethernet**. Once again, alternatively, make a copy of the *cedge-vpn10-int* template and rename it to *cedge-vpn30-int*, updating the description. Then Edit this newly created template and **Update** (followed in step 4 below)

    ![](/images/Service_cEdges_Templates/06_vpn10int.PNG)

4. Populate the details as shown below and click on **Save**

    | Section             | Field          | Global or Device Specific (drop down) | Value                                  |
    |---------------------|----------------|---------------------------------------|----------------------------------------|
    |                     | Template Name  | NA                                    | *cedge-vpn20-int*                      |
    |                     | Description    | NA                                    | *VPN 20 Interface Template for cEdges* |
    | Basic Configuration | Shutdown       | Global                                | No                                     |
    | Basic Configuration | Interface Name | Device Specific                       | *vpn20_if_name*                        |
    | Basic Configuration | IPv4 Address/ prefix-length   | Device Specific                       | *vpn20_if_ipv4_address*                |

    ![](/images/Service_cEdges_Templates/09_vpn30intconf.PNG)

This completes the configuration of the VPN 30 Feature Templates for the cEdges.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Configuring the cEdge VPN 10 Feature Templates~~](#configuring-the-cedge-vpn-10-feature-templates)
<br/>
- [~~Configuring the cEdge VPN 20 Feature Templates~~](#configuring-the-cedge-vpn-20-feature-templates)
<br/>
- [~~Configuring the cEdge VPN 30 Feature Templates~~](#configuring-the-cedge-vpn-30-feature-templates)
<br/>
" type="primary" %}
