---
title: Updating the Site 40 cEdge Template
tags: [content_types]
keywords: series, connected articles, tutorials, hello world
last_updated: May 25, 2020
summary: "Updating the Template at Site 40 to include the MPLS link"
sidebar: mydoc_sidebar
permalink: mydoc_Site40_Templates.html
folder: mydoc
---
<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Updating and Creating the Site 40 Feature Templates
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the VPN0 Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the MPLS VPN Interface Feature Template
    <br/>
- Modifying the Device Template

<br/>

" type="primary" %}

## Overview

While the Site40 cEdge is already in vManage mode, we will be looking at updating a Template in this section. The MPLS link on cEdge40 is unconfigured and we will be setting that up. VPN 0 also requires a default route next hop associated with the MPLS link.

## Updating and Creating the Site 40 Feature Templates

### Updating the VPN0 Feature Template

1. Go to **Configuration => Templates => Feature tab**. Click on the three dots next to the *cEdge_VPN0_dual_uplink* template and click on **Edit**. Scroll down to the IPv4 Route section and click on pencil icon to update the default route

    ![](/images/cEdgeSite40_Templates/08_editvpn0temp_addipv4route.PNG)

    ![](/images/cEdgeSite40_Templates/09_editroute.PNG)

2. Click on **1 Next Hop** to edit the next hops associated with the 0.0.0.0/0 route

    ![](/images/cEdgeSite40_Templates/10_clic1nexthop.PNG)

3. Click on **Add Next Hop**, choose **Device Specific** from the drop down in the newly added hop and give it a tag of *vpn0_mpls_next_hop_ip_address*. Click on **Save Changes**

    ![](/images/cEdgeSite40_Templates/11_addhop.PNG)

4. Make sure that the Update IPv4 Route screen shows **2 Next Hop** and click on **Save Changes** again

    ![](/images/cEdgeSite40_Templates/12_2nh_save.PNG)

5. You should be back at the main Feature Template page for *cEdge_VPN0_dual_uplink*. Click on **Update**

    ![](/images/cEdgeSite40_Templates/13_update.PNG)

6. We can add the details of the next hop (which was configured as a Device Specific parameter) on this page itself, without going through the Edit Device Template screen. This is only recommended when minor changes are needed. Double click the *Address(vpn0_mpls_next_hop_ip_address)* field and enter *192.1.2.17*

    ![](/images/cEdgeSite40_Templates/14_editdirectly.PNG)

7. Click on **Next** and then click on **Configure Devices**. Check the side by side difference, if needed, to view the ip route statement pushed by vManage

    ![](/images/cEdgeSite40_Templates/15_sbs_routead_confdev.PNG)

<br/>
{% include callout.html content="**Task List**
<br/><br/>

- [Updating and Creating the Site 40 Feature Templates](#updating-and-creating-the-site-40-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the VPN0 Feature Template~~](#updating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the MPLS VPN Interface Feature Template](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
- [Modifying the Device Template](#modifying-the-device-template)

<br/>

" type="primary" %}

### Creating the MPLS VPN Interface Feature Template

1. Go to **Configuration => Templates => Feature tab**. Click on the three dots next to the *cedge-vpn0-int-dual* template and click on **Copy**.

    ![](/images/cEdgeSite40_Templates/01_copy.PNG)

2. Rename the template to *cedge-vpn0-int-dual_mpls* with a Description of *cEdge VPN 0 Interface Template for devices with a dual uplink - MPLS*. Click on **Copy**

    ![](/images/cEdgeSite40_Templates/02_mpls.PNG)

3. Click on the dots next to the newly created template and choose to **Edit**

    ![](/images/cEdgeSite40_Templates/03_edit.PNG)

4. Make sure the Name and Description match as below. Update the **Interface Name** to *GigabitEthernet3* and the **IPv4 Address/ Prefix Length** to *mpls_ipv4_address*

    ![](/images/cEdgeSite40_Templates/04_name_desc_int_ip.PNG)

5. Under the **Tunnel** section, update the **Color** to *mpls_if_tunnel_color_value* and set **Restrict** to Global from the drop down and On (radio button). Click on **Update**

    ![](/images/cEdgeSite40_Templates/05_tunndet.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Updating and Creating the Site 40 Feature Templates~~](#updating-and-creating-the-site-40-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the VPN0 Feature Template~~](#updating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
- [Modifying the Device Template](#modifying-the-device-template)

<br/>

" type="primary" %}

## Modifying the Device Template

We now need to associate the template created in the previous step with the Device Template being used by cEdge40.

1. Go to **Configuration => Templates** and make sure you're on the Device tab. Click on the three dots next to the *cEdge_dualuplink_devtemp* template. Click on **Edit**.

    ![](/images/cEdgeSite40_Templates/06_editdevtemp.PNG)

2. Update the **Transport and Management VPN** section as per the screenshot below. You will need to click on **+ Cisco VPN Interface Ethernet** under **Additional Cisco VPN 0 Templates** in order to add a Cisco VPN Interface under VPN 0. Populate *cedge-vpn0-int-dual_mpls* and click on **Update**

    ![](/images/cEdgeSite40_Templates/07_addvpnint_update.PNG)

3. We should get the option to populate the details for cEdge40. These can be entered directly on the page, like before. Populate the two fields as shown below. Click **Next**

    ![](/images/cEdgeSite40_Templates/16_editdirectly.PNG)

4. Note that **GigabitEthernet3** is being configured (can be checked via the Config Diff page) and click on **Configure Devices**

    ![](/images/cEdgeSite40_Templates/17_sbsdiff_addgig3.PNG)

5. The configuration should be successful

    ![](/images/cEdgeSite40_Templates/18_confsucc.PNG)

6. Go to **Monitor => Network** and choose **vEdge30** (yes, we're choosing vEdge30 and not the cEdge we just configured). Click on **Real Time** and specify **BFD Sessions** in the Device Options field. Choose Do Not Filter

    ![](/images/cEdgeSite40_Templates/19_mon_net_ve30_real_bfd_dnf.PNG)

7. We should see that vEdge30 has established BFD sessions over the MPLS link with cEdge40

    ![](/images/cEdgeSite40_Templates/20_ve30bfdwithce40mpls.PNG)

8. Click the **Select Device** drop down and click on **cEdge40**. Choose Do Not Filter.

    ![](/images/cEdgeSite40_Templates/21_choosece40.PNG)

    ![](/images/cEdgeSite40_Templates/22_dnf.PNG)

9. The BFD sessions will show up, and we can verify that cEdge40 has established BFD sessions on the MPLS link as well

    ![](/images/cEdgeSite40_Templates/23_bfd_on_mpls.PNG)

10. Given below is a snapshot of the **Full WAN Connectivity** page from the main dashboard (verification only, nothing to be done here)

    ![](/images/cEdgeSite40_Templates/24_snapshot.PNG)

This completes our re-configuration for the Site 40 cEdge.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Updating and Creating the Site 40 Feature Templates~~](#updating-and-creating-the-site-40-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the VPN0 Feature Template~~](#updating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
- [~~Modifying the Device Template~~](#modifying-the-device-template)

<br/>

" type="primary" %}
