---
title: Feature and Device Templates for the DC-vEdges
tags: [getting_started]
last_updated: May 21, 2020
keywords: feature template, vedge, vmanage
summary: "Create Feature and Device Templates for the DC-vEdges in order to bring them in vManage mode."
sidebar: mydoc_sidebar
permalink: mydoc_DCvEdges_templates.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Creating the DC-vEdge VPN Feature Templates
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN0 Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN512 Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the INET VPN Interface Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the MPLS VPN Interface Feature Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the Mgmt VPN Interface Feature Template
    <br/>
- Creating a Device Template and Attaching Devices
    <br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview

We have already seen feature templates in action and their versatility in large deployments is unmatched. Coupled with Device Specific parameters, we have a networking construct which is extremely malleable and can be applied in wide, arcing sweeps to similar devices through Device Templates that act as containers for grouping multiple Feature Templates.

In this section, we will be creating feature templates for our DC-vEdges. We will then apply these Feature Templates to Device Templates. Devices will be attached to these Device Templates, thereby ensuring that the DC-vEdges are controlled by vManage.

## Creating the DC-vEdge VPN Feature Templates

### Creating the VPN0 Feature Template

1. On the vManage GUI, navigate to **Configuration => Templates**

    ![](/images/DC-vEdge_Templates/01_conftemp.PNG)

2. Click on the **Feature** tab and click on **Add Template**

    ![](/images/DC-vEdge_Templates/02_feataddtemp.PNG)

3. Search for *vedge* in the search box and put a check mark next to **vEdge Cloud**. This will give the options to select Feature Templates applicable to the selected device type. Click on **VPN** to start configuring a VPN Template. This is going to be our VPN Template for VPN 0

    ![](/images/DC-vEdge_Templates/03_vecloudvpn.PNG)

4. Give the Template a name of *DCvEdge-vpn0* and a description of *VPN0 for the DC-vEdges INET and MPLS link*

    ![](/images/DC-vEdge_Templates/03_ztempnamedesc.PNG)

5. Under **Basic Configuration**, specify the VPN as 0 (zero)

    ![](/images/DC-vEdge_Templates/04_tempnamedescvpn.PNG)

6. Populate the Primary and Secondary DNS Address as 10.y.1.5 and 10.y.1.6 respectively, where y is *1* if you're on the SLC DC and *2* if you're on the GHI DC (the email with lab details should enumerate which DC you're on). Set the drop down to **Global** in order to enter the IPs. The option to enter the Secondary DNS server will pop up once the Primary is populated

    ![](/images/DC-vEdge_Templates/05_dns.PNG)

7. Under **IPv4 Route**, click on **New IPv4 Route** and specify the Prefix as Global. Populate *0.0.0.0/0* as the prefix and click on **Add Next Hop**

    ![](/images/DC-vEdge_Templates/07_ipv4route.PNG)

8. Click on **Add Next Hop** again in the popup window

    ![](/images/DC-vEdge_Templates/08_addhop.PNG)

9. From the drop down, set the value to Device Specific and enter the key as *vpn0_inet_next_hop*

    ![](/images/DC-vEdge_Templates/09_nexthopinet2.PNG)

10. Click on **Add Next Hop**. We will now be adding the default route for the MPLS link

    ![](/images/DC-vEdge_Templates/10_addmplshop.PNG)

11. Choose **Device Specific** from the drop down and give it a name of *vpn0_mpls_next_hop*. Click on **Add**

    ![](/images/DC-vEdge_Templates/11_nexthopmpls.PNG)

12. Make sure the IPv4 Route screen shows **2 Next Hop** and click on Add

    ![](/images/DC-vEdge_Templates/12_addroute.PNG)

13. Back at the main Feature Template page, click on **Save**. This will create our VPN 0 Feature Template

    ![](/images/DC-vEdge_Templates/13_save.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Creating the DC-vEdge VPN Feature Templates](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN512 Feature Template](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the INET VPN Interface Feature Template](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the MPLS VPN Interface Feature Template](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the Mgmt VPN Interface Feature Template](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [Creating a Device Template and Attaching Devices](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the VPN512 Feature Template

We will make use of the just created VPN 0 Feature Template to create our VPN 512 Feature Template.

1. On the **Configuration => Templates** page navigate to the Feature tab and look for *DCvEdge-vpn0*. Click on the three dots for this template and click on **Copy**

    ![](/images/DC-vEdge_Templates/14_copytemp.PNG)

2. Give the Template a name of *DCvEdge-vpn512* and a description of *VPN512 for the DC-vEdges*. Click on **Copy**

    ![](/images/DC-vEdge_Templates/15_tempnamedesccopy.PNG)

3. Click on the three dots for the newly created template and click on **Edit**

    ![](/images/DC-vEdge_Templates/16_editcopied.PNG)

4. Update the Description, if it hasn't been updated and change the VPN to **512**

    ![](/images/DC-vEdge_Templates/17_vpntag.PNG)

5. Scroll down to the IPv4 Route section and click on the **pencil** icon to edit the 0.0.0.0/0 Route

    ![](/images/DC-vEdge_Templates/18_editroute.PNG)

6. Click on **2 Next Hop**. We will be removing the MPLS next hop entry and modifying the name of the INET next hop for the management network

    ![](/images/DC-vEdge_Templates/19_vpn512defroute.PNG)

7. Click on the minus sign to remove the MPLS next hop

    ![](/images/DC-vEdge_Templates/20_delmplsnh.PNG)

8. Update the Device Specific information for the first entry to *vpn512_next_hop*. Click on **Save Changes**

    ![](/images/DC-vEdge_Templates/21_vpn512nh.PNG)

9. Click on **Save Changes** again.  The Update IPv4 Route page should now reflect 1 Next Hop

    ![](/images/DC-vEdge_Templates/22_save.PNG)

10. Click on **Update** on the main feature template page to save the changes that we have made. The Selected Gateway Configuration should have the number 1 against it

    ![](/images/DC-vEdge_Templates/23_update.PNG)

We have created our VPN512 Feature Template

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Creating the DC-vEdge VPN Feature Templates](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the INET VPN Interface Feature Template](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the MPLS VPN Interface Feature Template](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the Mgmt VPN Interface Feature Template](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [Creating a Device Template and Attaching Devices](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the INET VPN Interface Feature Template

We are now going to set up the VPN Interface Feature Templates for the Internet link. This template specifies the configuration for the interfaces in a VPN. There will be two interfaces in VPN 0 (INET and MPLS) and one interface in VPN 512. Let's start off with configuring the INET interface.

1. From **Configuration => Templates** on the Feature tab, Add a new template. Search for *ved* in the search box and choose the vEdge Cloud Device. Click on **VPN Interface Ethernet** to start creating our VPN Interface Template

    ![](/images/DC-vEdge_Templates/24_addtempvpnint.PNG)

2. Populate the details on this page as given below. Screenshots can be used for reference. Click on **Save** once the fields have been populated

    | Section                | Field            | Global or Device Specific (drop down) | Value                              |
    |------------------------|------------------|---------------------------------------|------------------------------------|
    |                        | Template Name    | NA                                    | *DC-vEdge_INET*                    |
    |                        | Description      | NA                                    | *INET interface for the DC-vEdges* |
    | Basic Configuration    | Shutdown         | Global                                | No                                 |
    | Basic Configuration    | Interface Name   | Device Specific                       | *vpn0_inet_if_name*                |
    | Basic Configuration    | IPv4 Address     | Device Specific                       | *vpn0_inet_if_ip_add*              |
    | Tunnel                 | Tunnel Interface | Global                                | On                                 |
    | Tunnel                 | Color            | Device Specific                       | *vpn0_inet_if_color*               |
    | Tunnel - Allow Service | All              | Global                                | On                                 |

    ![](/images/DC-vEdge_Templates/25_tempnamedescip.PNG)

    ![](/images/DC-vEdge_Templates/26_tunn.PNG)

    ![](/images/DC-vEdge_Templates/27_allowall_save.PNG)

This completes the configuration of our INET Interface Feature Template. Notice that we will be populating quite a few details when the Device is attached to a Device Template which contains this Feature Template.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Creating the DC-vEdge VPN Feature Templates](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET VPN Interface Feature Template~~](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the MPLS VPN Interface Feature Template](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the Mgmt VPN Interface Feature Template](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [Creating a Device Template and Attaching Devices](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the MPLS VPN Interface Feature Template

We are now going to set up the VPN Interface Feature Template for the MPLS link, making a copy from the INET template that we created in the previous section.

1. Identify the *DC-vEdge_INET* Feature Template from **Configuration => Templates => Feature tab**. Click on the three dots in the extreme right-hand side of the template and click Copy. Name it *DC-vEdge_MPLS* with a Description of *MPLS interface for the DC-vEdges*. Click on **Copy**

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

- [Creating the DC-vEdge VPN Feature Templates](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET VPN Interface Feature Template~~](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the Mgmt VPN Interface Feature Template](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [Creating a Device Template and Attaching Devices](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the Mgmt VPN Interface Feature Template

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

- [~~Creating the DC-vEdge VPN Feature Templates~~](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET VPN Interface Feature Template~~](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the Mgmt VPN Interface Feature Template~~](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [Creating a Device Template and Attaching Devices](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Creating a Device Template and Attaching Devices

Most of the work has already been done, with respect to creating the building blocks for our Device Templates. All that's left is ensuring we create a Device Template with the corresponding Feature Templates and associate the Devices with the Template.

1. Navigate to the **Configuration => Templates** section and make sure you're on the **Device** tab. Click on **Create Template => From Feature Template**

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

- [~~Creating the DC-vEdge VPN Feature Templates~~](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the INET VPN Interface Feature Template~~](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the Mgmt VPN Interface Feature Template~~](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [~~Creating a Device Template and Attaching Devices~~](#creating-a-device-template-and-attaching-devices)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Activity Verification

1. Go to **Configuration => Devices** and you should see that the two DC-vEdges are now in vManage mode

    ![](/images/DC-vEdge_Templates/47_configdev_vmmode.PNG)

2. On checking the main dashboard (**Dashboard => Main Dashboard**) we should see 5 sites with full WAN connectivity (if you recall, we previously could see only 4 sites with full WAN connectivity and Site 50 wasn't showing up at all. This was because BFD sessions weren't established on the MPLS link)

    ![](/images/DC-vEdge_Templates/48_maindash.PNG)

3. If we click on **Full WAN Connectivity**, Site 50 now shows up

    ![](/images/DC-vEdge_Templates/49_site50showsup.PNG)

4. Use Putty to access **cEdge51** and issue `show bfd sessions`. We now see BFD sessions with DC-vEdge1 and DC-vEdge2, on the MPLS link

    ![](/images/DC-vEdge_Templates/50_bfdsessestonce51.PNG)

This completes the verification activity

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the DC-vEdge VPN Feature Templates~~](#creating-the-dc-vedge-vpn-feature-templates)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN0 Feature Template~~](#creating-the-vpn0-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN512 Feature Template~~](#creating-the-vpn512-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-inet-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the MPLS VPN Interface Feature Template~~](#creating-the-mpls-vpn-interface-feature-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the Mgmt VPN Interface Feature Template~~](#creating-the-mgmt-vpn-interface-feature-template)
    <br/>
- [~~Creating a Device Template and Attaching Devices~~](#creating-a-device-template-and-attaching-devices)
    <br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
