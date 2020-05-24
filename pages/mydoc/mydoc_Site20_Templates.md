---
title: Templates for vEdges in Site 20
tags: [formatting]
keywords: device template, vedge, vmanage
summary: "Create Feature and Device Templates for the Site 20 vEdges"
sidebar: mydoc_sidebar
permalink: mydoc_Site20_Templates.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Creating the Site 20 VPN Feature Templates
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN0 and VPN512 Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the INET and MPLS VPN Interface Feature Template
    <br/>
- Modifying a Device Template and Attaching Devices

<br/>

" type="primary" %}

##  Overview

We can take the Feature Templates created for the DC-vEdges and use them as a starting point for configuring the Feature Templates at Site 20. Necessary changes based on the topology will need to be made (for example, things like a single uplink at the Site20 devices vs. a dual uplink at the DC devices)

## Creating the Site 20 VPN Feature Templates

We are now going to set up the VPN Interface Feature Template for the MPLS link, making a copy from the INET template that we created in the previous section.

1. Identify the *DC-vEdge_INET* Feature Template from **Configuration -> Templates -> Feature tab**. Click on the three dots in the extreme right-hand side of the template and click Copy. Name it *DC-vEdge_MPLS* with a Description of *MPLS interface for the DC-vEdges*. Click on **Copy**

    ![](/images/DC-vEdge_Templates/28_mpls_namedesccopy.PNG)

2. Click on the 3 dots next to the copied template and choose to **Edit**. Modify the details as per the table given below and click on **Update** (we have changed the Device Specific names to reflect mpls and set the restrict to On)

    | Section             | Field            | Global or Device Specific (drop down) | Value                              |
    |---------------------|------------------|---------------------------------------|------------------------------------|
    |                     | Template Name    | NA                                    | *DC-vEdge_MPLS*                    |
    |                     | Description      | NA                                    | *MPLS interface for the DC-vEdges* |
    | Basic Configuration | Shutdown         | Global                                | No                                 |
    | Basic Configuration | Interface Name   | Device Specific                       | *vpn0_mpls_if_name*                |
    | Basic Configuration | IPv4 Address     | Device Specific                       | *vpn0_mpls_if_ip_add*              |
    | Tunnel              | Tunnel Interface | Global                                | On                                 |
    | Tunnel              | Color            | Device Specific                       | *vpn0_mpls_if_color*               |
    | Tunnel              | Restrict         | Global                                | On                                 |

    ![](/images/DC-vEdge_Templates/29_name_if_ip.PNG)

    ![](/images/DC-vEdge_Templates/30_tunn_mpls_update.PNG)

This completes the configuration of the MPLS VPN Interface Feature Template.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Creating the Site 20 VPN Feature Templates](#creating-the-site-20-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 and VPN512 Feature Template~~](#creating-the-vpn0-and-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the INET and MPLS VPN Interface Feature Template](#creating-the-inet-and-mpls-vpn-interface-feature-template)
    <br/>
- [Modifying a Device Template and Attaching Devices](#modifying-a-device-template-and-attaching-devices)

<br/>

" type="primary" %}

### Creating the VPN0 and VPN512 Feature Template

Just like before, we will make a copy of the DC-vEdge_INET Feature Template and use that for our VPN 512 Management Interface Template.

1. Locate the DC-vEdge_INET template created before, click on the 3 dots at the end and choose to **Copy** the template

    ![](/images/DC-vEdge_Templates/31_copyinet_forvpn512_2.PNG)

2. Rename it to *DC-vEdge_mgmt_int* with a Description of *MGMT interface for the DC-vEdges*. Click on **Copy**

    ![](/images/DC-vEdge_Templates/32_rename.PNG)

3. Click on the 3 dots next to the newly created template and choose to **Edit**. Populate the details in the template as per the following table and click on **Update**. The Tunnel Interface has been set to Off

    | Section             | Field            | Global or Device Specific (drop down) | Value                              |
    |---------------------|------------------|---------------------------------------|------------------------------------|
    |                     | Template Name    | NA                                    | *DC-vEdge_mgmt_int*                |
    |                     | Description      | NA                                    | *MGMT interface for the DC-vEdges* |
    | Basic Configuration | Shutdown         | Global                                | No                                 |
    | Basic Configuration | Interface Name   | Device Specific                       | *vpn512_mgmt_if_name*              |
    | Basic Configuration | IPv4 Address     | Device Specific                       | *vpn512_mgmt_if_ip_add*            |
    | Tunnel              | Tunnel Interface | Global                                | Off                                |

    ![](/images/DC-vEdge_Templates/33_name_ifname_ip.PNG)

    ![](/images/DC-vEdge_Templates/34_Tunnoff_Update.PNG)

We have creatd the VPN 512 Interface Template.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the Site 20 VPN Feature Templates~~](#creating-the-site-20-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 and VPN512 Feature Template~~](#creating-the-vpn0-and-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the INET and MPLS VPN Interface Feature Template](#creating-the-inet-and-mpls-vpn-interface-feature-template)
    <br/>
- [Modifying a Device Template and Attaching Devices](#modifying-a-device-template-and-attaching-devices)

<br/>

" type="primary" %}

### Creating the INET and MPLS VPN Interface Feature Template

Most of the work has already been done, with respect to creating the building blocks for our Device Templates. All that's left is ensuring we create a Device Template with the corresponding Feature Templates and associate the Devices with the Template.

1. Navigate to the **Configuration -> Templates** section and make sure you're on the **Device** tab. Click on **Create Template -> From Feature Template**

    ![](/images/DC-vEdge_Templates/35_createdevtemp.PNG)

2. Choose Device Model as **vEdge Cloud**, and give the Template a name of *DCvEdge_dev_temp*. Give it a Description of *Device template for the DC-vEdges*

    ![](/images/DC-vEdge_Templates/36_name_devtype.PNG)

3. Under **Transport and Management** choose the VPN 0 template as *DCvEdge-vpn0* and the VPN 512 Template as *DCvEdge-vpn512*. Click twice on **VPN Interface** under *Additional VPN 0 Templates*. This will add two VPN Interfaces where we can associate our VPN Interface Templates. Click once on **VPN Interface** under *Additional VPN 512 Templates* to add a VPN Interface for VPN 512

    ![](/images/DC-vEdge_Templates/37_popvpntemp_clickaddvpnint.PNG)

4. Populate the VPN Interface fields from the drop down as show below and click on **Create**

    ![](/images/DC-vEdge_Templates/38_vpnint_create.PNG)

5. Click on the three dots next to the newly created Device Template named *DCvEdge_dev_temp* and click on **Attach Devices**

    ![](/images/DC-vEdge_Templates/39_attach.PNG)

6. Move **DC-vEdge1** and **DC-vEdge2** to the list of selected devices and click on Attach

    ![](/images/DC-vEdge_Templates/40_choosevedges.PNG)

7. Click on the three dots next to DC-vEdge1 and choose **Edit Device Template**. Enter the details as shown below (these are the Device Specific paramters we had defined in the Feature Templates, along with some parameters that are part of the Default Templates pre-populated in the Device Template). Click on **Update** once everything has been populated exactly as shown below. This information can also be picked up from the table given in the topology section

    ![](/images/DC-vEdge_Templates/41_enterdet.PNG)

8. Click on the three dots next to DC-vEdge2 and choose **Edit Device Template**. Enter the details as shown below. Click on **Update** once done

    ![](/images/DC-vEdge_Templates/42_enterdetvedge2.PNG)

    Click on **Next** to proceed

9. At this point, you can simply click on **Configure Devices** to start pushing the configuration to the devices, or you can click on an individual device on the left-hand side and followed by Config Diff and then Side by Side to view a comparison of the current configuration on the device vs. what will be pushed out. This is great for reviewing the configuration that is going to be pushed and for learning the syntax. Note that we are adding the MPLS interface and relevant configuration on our devices, which wasn't done before.

    ![](/images/DC-vEdge_Templates/43_checkconfigdif.PNG)

    ![](/images/DC-vEdge_Templates/44_maindiffmpls_configdev.PNG)

10. On clicking on Configure Devices, you will need to put a check mark next to **Confirm configuration changes on 2 devices** and click on OK

    ![](/images/DC-vEdge_Templates/45_checkconfig.PNG)

11. Once complete, you should see a **Success** message against each device that was configured

    ![](/images/DC-vEdge_Templates/46_success.PNG)

    {% include tip.html content="In case a loss of connectivity occurs as a result of the configuration changes that were pushed to the Devices, there is an automatic rollback timer of 6 minutes which kicks in. Devices will revert to their previous configuration in this case. The rollback timer can be configured (on the final page before we choose to configure our devices, there is a hyperlink in the bottom left hand corner)" %}

    ![](/images/DC-vEdge_Templates/44_maindiffmpls_configdev2.PNG)

    <br/>

    {% include callout.html content="**Task List**
    <br/><br/>

    - [~~Creating the Site 20 VPN Feature Templates~~](#creating-the-site-20-vpn-feature-templates)
    <br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 and VPN512 Feature Template~~](#creating-the-vpn0-and-vpn512-feature-template)
        <br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET and MPLS VPN Interface Feature Template~~](#creating-the-inet-and-mpls-vpn-interface-feature-template)
        <br/>
    - [Modifying a Device Template and Attaching Devices](#modifying-a-device-template-and-attaching-devices)

    <br/>

    " type="primary" %}

## Modifying a Device Template and Attaching Devices

1. Go to **Configuration -> Devices** and you should see that the two DC-vEdges are now in vManage mode

    ![](/images/DC-vEdge_Templates/47_configdev_vmmode.PNG)

2. On checking the main dashboard (**Dashboard -> Main Dashboard**) we should see 5 sites with full WAN connectivity (if you recall, we previously could see only 4 sites with full WAN connectivity and Site 50 wasn't showing up at all. This was because BFD sessions weren't established on the MPLS link)

    ![](/images/DC-vEdge_Templates/48_maindash.PNG)

3. If we click on **Full WAN Connectivity**, Site 50 now shows up

    ![](/images/DC-vEdge_Templates/49_site50showsup.PNG)

4. Use Putty to access **cEdge51** and issue `show bfd sessions`. We now see BFD sessions with DC-vEdge1 and DC-vEdge2, on the MPLS link

    ![](/images/DC-vEdge_Templates/50_bfdsessestonce51.PNG)

This completes the verification activity

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the Site 20 VPN Feature Templates~~](#creating-the-site-20-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 and VPN512 Feature Template~~](#creating-the-vpn0-and-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET and MPLS VPN Interface Feature Template~~](#creating-the-inet-and-mpls-vpn-interface-feature-template)
    <br/>
- [~~Modifying a Device Template and Attaching Devices~~](#modifying-a-device-template-and-attaching-devices)

<br/>

" type="primary" %}
