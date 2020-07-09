---
title: Site 30 vEdge Templates
audience: writer, designer
tags: [navigation]
last_updated: May 25, 2020
keywords: tags, navigation, buttons, links, association
summary: "Creating Feature and Device Templates for the vEdge in Site 30"
sidebar: mydoc_sidebar
permalink: mydoc_Site30_Templates.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Creating the Site 30 Feature Templates

    <br/>
- Modifying a Device Template and Attaching Devices

<br/>

" type="primary" %}

##  Overview

vEdge30 and the DC-vEdges are quite similar from a configuration standpoint. The templates already created for the DC-vEdges can be re-used for Site 30, but we will be making a copy of those templates and applying the renamed copies to the Device Template for Site 30. This is because DC and Branch sites will generally have some configuration changes down the line which will not apply to both sites. It's a good practice to keep the number of templates to a minimum, keeping in mind the treatment given to different sites. If Site 30 and the DC Site share the same template, any changes made on one will affect the other.

## Creating the Site 30 Feature Templates

We will set up the VPN templates for VPN 0 in Site 30 by making a copy of the *DCvEdge-vpn0* Feature Template created before. No other major changes will be made to the template itself

1. From **Configuration => Templates => Feature tab** search in the search box for *dc*. We should see a few templates, out of which we will be making copies of *DCvEdge-vpn0*, *DC-vEdge_INET* and *DC-vEdge_MPLS* for use at Site 30

    ![](/images/vEdgeSite30_Templates/01_confTemp_searchdc.PNG)

2. Click on the three dots next to *DCvEdge-vpn0* and choose **Copy**. Rename the template to *vEdge30-vpn0* with a description of *VPN0 for the Site30 INET and MPLS link*. Click on **Copy**

    ![](/images/vEdgeSite30_Templates/02_copyvpn0.PNG)

3. Click on the dots next to the newly created template and choose to **Edit**. Make sure the Template Name and Description match. Click on **Update**

    ![](/images/vEdgeSite30_Templates/03_edittemp_updatedesc.PNG)

4. Repeat steps 2 and 3 above, making copies of *DC-vEdge_INET* and *DC-vEdge_MPLS*, renaming them to *vEdge30_INET* and *vEdge30_MPLS* respectively. Update the descriptions as necessary, while copying the template and (if required - note that the description does not get updated at times while copying) by editing the template and choosing to **Update**

    ![](/images/vEdgeSite30_Templates/04_inet.PNG)

    ![](/images/vEdgeSite30_Templates/05_mpls.PNG)

5. If we go back to the main **Configuration => Templates => Feature Tab**, and search for *vedge30* in the search string, there should be 3 templates visible

    ![](/images/vEdgeSite30_Templates/06_3featvedge30.PNG)

Thus, we have simply made copies of the DC-vEdge Feature Templates and updated the name/description so as to apply different configuration to the two Sites (Site 30 and DC) down the line, if required.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the Site 30 Feature Templates~~](#creating-the-site-30-feature-templates)
<br/>

- [Modifying a Device Template and Attaching Devices](#modifying-a-device-template-and-attaching-devices)

<br/>

" type="primary" %}

## Modifying a Device Template and Attaching Devices

1. Go to **Configuration => Templates** and make sure you're on the Device tab. Click on the three dots next to the *DCvEdge_dev_temp*. Click on **Copy**. Rename the Template *vEdge30_dev_temp* and give it a Description of *Device template for the Site 30 vEdge*. Click on **Copy**

    ![](/images/vEdgeSite30_Templates/07_devtemp.PNG)

2. Click on the three dots next to the newly created template and click on **Edit**. Update the **Transport and Management VPN** section as per the screenshot below. We will be re-using the VPN 512 Templates created for the DC-vEdges. Click on **Update** once done.

    ![](/images/vEdgeSite30_Templates/08_editdevtemp.PNG)

3. Click on the three dots next to the newly created *vEdge30_dev_temp* Template and click on **Attach Devices**

    ![](/images/vEdgeSite30_Templates/09_attach.PNG)

4. Choose **vEdge30** from the list and click on **Attach**

    ![](/images/vEdgeSite30_Templates/10_chooseve30attach.PNG)

5. The device should show up in the list. Click on the three dots next to vEdge30 and choose to **Edit Device Template**. Populate the details as shown below and click on **Update**

    ![](/images/vEdgeSite30_Templates/11_popdet_upd.PNG)

6. **DO NOT** click on Next or Configure Devices at this point. Log in to the CLI for vEdge30 and issue a `show bfd sessions`.

    ![](/images/vEdgeSite30_Templates/12_shbfdbefore.PNG)

7. Back at the vManage GUI, click on **Next** and then **Configure Devices**. You can view the side-by-side difference, making note of the fact that we are adding an MPLS interface

    ![](/images/vEdgeSite30_Templates/13_sbs_ge00_ge01_conf.PNG)

8. Once the configuration goes through, log back into the CLI of vEdge30 and issue `show bfd sessions`. You should see BFD sessions on the mpls TLOC as well

    ![](/images/vEdgeSite30_Templates/14_shbfd_after.PNG)

9. On the vManage GUI, if you click on **Full WAN Connectivity** on the Main Dashboard, you will see that vEdge30 has a total of 9 BFD sessions

    ![](/images/vEdgeSite30_Templates/15_ninebfdsess.PNG)

10. To see the BFD sessions, we can also go to **Monitor => Network**, click on vEdge30. Choose Real-Time from the left hand side and put **BFD Sessions** in the Device Options. Choose Do Not Filter

    ![](/images/vEdgeSite30_Templates/16_mon_net_scrollreal.PNG)

    ![](/images/vEdgeSite30_Templates/17_bfdsess.PNG)

11. We will see the same information as what was visible on the CLI in Step 8. Note that Site40 is missing from this list. That is because we haven't added the MPLS configuration to Site 40 yet. This will be done in the next section.

    ![](/images/vEdgeSite30_Templates/18_sameinfoascli.PNG)

12. Navigate to **Configuration => Devices** and you will see that all devices are now in vManage mode

    ![](/images/vEdgeSite30_Templates/99_vmmode.PNG)

This completes our Configuration for bringing Site 30 under the control of vManage.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the Site 30 Feature Templates~~](#creating-the-site-30-feature-templates)

    <br/>
- [~~Modifying a Device Template and Attaching Devices~~](#modifying-a-device-template-and-attaching-devices)

<br/>

" type="primary" %}
