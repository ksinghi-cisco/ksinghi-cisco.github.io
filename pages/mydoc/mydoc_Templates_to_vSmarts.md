---
title: Applying Templates to the vSmarts
tags: [single_sourcing]
keywords: vSmart, Templates, control policies
last_updated: May 25, 2020
summary: "Applying Templates to the vSmarts in order to bring them in vManage mode. This will allow policy enforcement"
sidebar: mydoc_sidebar
permalink: mydoc_Templates_to_vSmarts.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Configuring VPN 0 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 0 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 0 Interface Template
    <br/>

- Configuring VPN 512 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 512 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 512 Interface Template
    <br/>
- Attaching vSmarts to the Device Template and Verification

<br/>

" type="primary" %}

## Configuring VPN 0 Templates for vSmarts

We will now create and apply Templates to the vSmarts. This will allow us to enforce Centralized Policies, which will be used in the following sections.

Unlike before, we will create a Device Template and set up our Feature Templates on the fly. You will notice that vSmart Templates are simpler than the other Templates we've used so far.

{% include note.html content="We will start by creating the overarching Device Template and create Feature Templates from within the Device Template. Hence, most of the sections outlined below are part of the same flow (i.e. Device Template) and follow one after the other, usually on the same Device Template page." %}

### Configuring the main VPN 0 template

1. Go to **Configuration => Templates**

    ![](/images/vSmart_Templates/01_temp.PNG)

2. While on the Device Tab (we're creating Device Templates), click on **Create Template** and choose **From Feature Template**

    ![](/images/vSmart_Templates/02_templ.PNG)

3. Select the Device Model as *vSmart*, populate the Template Name as *vSmart-dev-temp* and the Description as *Device Template for vSmarts*

    ![](/images/vSmart_Templates/03_name.PNG)

4. Under **Transport and Management VPN**, click on the drop down next to **VPN 0**. Click on **Create Template**. This is where we're creating our Feature Templates on the fly

    ![](/images/vSmart_Templates/04_createtemp.PNG)

5. Populate the details in the template as given below

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                           |
    |---------------------------|-----------------------|---------------------------------------|---------------------------------|
    |                           | Template Name         | NA                                    | *vSmart-VPN0*                   |
    |                           | Description           | NA                                    | *VPN0 Template for the vSmarts* |
    | Basic Configuration       | VPN                   | Global                                | VPN 0                           |
    | Basic Configuration - DNS | Primary DNS Address   | Global                                | 10.y.1.5                        |
    | Basic Configuration - DNS | Secondary DNS Address | Global                                | 10.y.1.6                        |

    Over here, y is *1* if you're on the SLC DC and *2* if you're on the GHI DC (the email with lab details should enumerate which DC you're on).

    ![](/images/vSmart_Templates/05_conf1.PNG)

6. Under **IPv4 Route** click on New IPv4 Route and specify the Prefix as *0.0.0.0/0*. Click on **Add Next Hop**

    ![](/images/vSmart_Templates/06_newroute.PNG)

7. Click on **Add Next Hop** again

    ![](/images/vSmart_Templates/07_anh.PNG)

8. Enter the Address as *100.100.100.1*, making it a Global value. Click on **Add**

    ![](/images/vSmart_Templates/08_nh.PNG)

9. Click on **Add** again in the IPv4 Route section to add the route

    ![](/images/vSmart_Templates/09_addagain.PNG)

10. Click on **Save** to save this Feature Template

    ![](/images/vSmart_Templates/10_save.PNG)

This completes the configuration of the Main VPN 0 Template. Continue with configuring the VPN 0 Interface Template.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Configuring VPN 0 Templates for vSmarts](#configuring-vpn-0-templates-for-vsmarts)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the main VPN 0 template~~](#configuring-the-main-vpn-0-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the VPN 0 Interface Template](#configuring-the-vpn-0-interface-template)
    <br/>

- [Configuring VPN 512 Templates for vSmarts](#configuring-vpn-512-templates-for-vsmarts)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the main VPN 512 template](#configuring-the-main-vpn-512-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the VPN 512 Interface Template](#configuring-the-vpn-512-interface-template)
    <br/>
- [Attaching vSmarts to the Device Template and Verification](#attaching-vsmarts-to-the-device-template-and-verification)

<br/>

" type="primary" %}

### Configuring the VPN 0 Interface Template

1. Click on **VPN Interface** from the Additional VPN 0 Templates section and click on the drop down for VPN Interface. Click on **Create Template** to create the VPN Interface Feature Template

    ![](/images/vSmart_Templates/11_vpnint.PNG)

2. Populate the details as given below and click on **Save**

    | Section                                | Field            | Global or Device Specific (drop down) | Value                        |
    |----------------------------------------|------------------|---------------------------------------|------------------------------|
    |                                        | Template Name    | NA                                    | *vSmart-VPN0-Int*            |
    |                                        | Description      | NA                                    | *VPN0 Interface for vSmarts* |
    | Basic Configuration                    | Shutdown         | Global                                | No                           |
    | Basic Configuration              | Interface Name   | Global                                | eth0                         |
    | Basic Configuration - IP Configuration | IPv4 Address     | Device Specific                       | *vpn0_if_ip_address*         |
    | Tunnel                                 | Tunnel Interface | Global                                | On                           |
    | Tunnel                                 | Color            | Global                                | public-internet              |
    | Tunnel - Allow Service                 | All              | Global                                | On                           |

    ![](/images/vSmart_Templates/12_vpnintconf1.PNG)

    ![](/images/vSmart_Templates/13_tunnsave.PNG)

This completes the configuration of the VPN 0 Interface Template.

Make sure the VPN 0 and VPN 0 Interface Templates just created are selected from the drop down in the Device Template we're building before proceeding to create the VPN 512 Templates.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Configuring VPN 0 Templates for vSmarts~~](#configuring-vpn-0-templates-for-vsmarts)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the main VPN 0 template~~](#configuring-the-main-vpn-0-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the VPN 0 Interface Template~~](#configuring-the-vpn-0-interface-template)
    <br/>

- [Configuring VPN 512 Templates for vSmarts](#configuring-vpn-512-templates-for-vsmarts)
<br/>

- [Attaching vSmarts to the Device Template and Verification](#attaching-vsmarts-to-the-device-template-and-verification)

<br/>

" type="primary" %}

## Configuring VPN 512 Templates for vSmarts

1. On the Device Template page itself, click on the drop down next to **VPN 512** under the **Transport and Management VPN** section. Click on **Create Template**

    ![](/images/vSmart_Templates/14_vpn512.PNG)

2. Enter the details as shown below

    | Section                   | Field                 | Global or Device Specific (drop down) | Value                           |
    |---------------------------|-----------------------|---------------------------------------|---------------------------------|
    |                           | Template Name         | NA                                    | *vSmart-VPN512*                   |
    |                           | Description           | NA                                    | *VPN512 Template for the vSmarts* |
    | Basic Configuration       | VPN                   | Global                                | VPN 512                           |
    | Basic Configuration - DNS | Primary DNS Address   | Global                                | 10.y.1.5                        |
    | Basic Configuration - DNS | Secondary DNS Address | Global                                | 10.y.1.6                        |

    Over here, y is *1* if you're on the SLC DC and *2* if you're on the GHI DC (the email with lab details should enumerate which DC you're on).

    ![](/images/vSmart_Templates/15_vpn512conf.PNG)

3. Under **IPv4 Route** click on New IPv4 Route and specify the Prefix as *0.0.0.0/0*. Click on **Add Next Hop**

    ![](/images/vSmart_Templates/16_ipv4route.PNG)

4. Click on **Add Next Hop** again

    ![](/images/vSmart_Templates/17_anh.PNG)

5. Enter the address as *192.168.0.1*, a Global value. Click on **Add**

    ![](/images/vSmart_Templates/18_nh.PNG)

6. Click on **Add** again to add the IPv4 Route and then click on **Save**

    ![](/images/vSmart_Templates/19_addsave.PNG)

7. Back on the main Device Template page, make sure *vSmart-VPN512* is selected as the Template. Click on **VPN Interface** under **Additional VPN 512 Templates** and click on the drop down. Choose to **Create Template**. We're creating the VPN 512 Interface Feature Template at this point

    ![](/images/vSmart_Templates/20_vpn512int.PNG)

8. Enter the details as shown below and click on **Save**

    | Section                                | Field            | Global or Device Specific (drop down) | Value                        |
    |----------------------------------------|------------------|---------------------------------------|------------------------------|
    |                                        | Template Name    | NA                                    | *vSmart-vpn512-int*            |
    |                                        | Description      | NA                                    | *VPN512 Interface Template for the vSmarts* |
    | Basic Configuration                    | Shutdown         | Global                                | No                           |
    | Basic Configuration              | Interface Name   | Global                                | eth1                         |
    | Basic Configuration - IP Configuration | IPv4 Address     | Device Specific                       | *vpn512_if_ip_address*         |
    | Tunnel                                 | Tunnel Interface | Global                                | Off                           |

    ![](/images/vSmart_Templates/21_vpn512intconfg.PNG)

9. Make sure the **Transport and Management VPN** section is populated as shown below and click on **Create**.

    ![](/images/vSmart_Templates/22_create.PNG)

We have completed the Device Template (and consequently the Feature Template) configuration for our vSmarts.

{% include callout.html content="**Task List**
<br/><br/>

- [~~Configuring VPN 0 Templates for vSmarts~~](#configuring-vpn-0-templates-for-vsmarts)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the main VPN 0 template~~](#configuring-the-main-vpn-0-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the VPN 0 Interface Template~~](#configuring-the-vpn-0-interface-template)
    <br/>

- [~~Configuring VPN 512 Templates for vSmarts~~](#configuring-vpn-512-templates-for-vsmarts)
<br/>

- [Attaching vSmarts to the Device Template and Verification](#attaching-vsmarts-to-the-device-template-and-verification)

<br/>

" type="primary" %}

## Attaching vSmarts to the Device Template and Verification

Our Device Template for the vSmarts are set up and we now need to attach them to the Template.

1. Click on **Configuration => Templates** (if not already there) and click the three dots next to the *vSmart-dev-temp* we just created. Click on **Attach Devices**

    ![](/images/vSmart_Templates/23_attach.PNG)

2. Choose both the vSmarts and click on **Attach**

    ![](/images/vSmart_Templates/24_chooseattach.PNG)

3. Populate the details in the Device Template window itself (if you're more comfortable with the **Edit Device Template** option, use that) and click on **Next**. Details to be entered are shown in the image below

    ![](/images/vSmart_Templates/25_popdet.PNG)

4. Click on the Device List on the left-hand side and click on **Config Diff**. Choose **Side By Side Diff** to review the configuration difference

    ![](/images/vSmart_Templates/26_choosedev_config.PNG)

5. Once done reviewing the configuration difference, click on **Configure Devices**

    ![](/images/vSmart_Templates/27_nomajdev.PNG)

6. Confirm the configuration change by clicking on the check box and then clicking OK

    ![](/images/vSmart_Templates/28_confirm.PNG)

7. Wait for the vSmarts to be configured successfully

    ![](/images/vSmart_Templates/29_succ.PNG)

8. Navigate to **Configuration => Devices** and go to the **Controllers** tab. You should see the vSmarts in vManage mode

    ![](/images/vSmart_Templates/30_vmmode.PNG)

This completes our activity of attaching Device Templates to the vSmarts.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Configuring VPN 0 Templates for vSmarts~~](#configuring-vpn-0-templates-for-vsmarts)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the main VPN 0 template~~](#configuring-the-main-vpn-0-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the VPN 0 Interface Template~~](#configuring-the-vpn-0-interface-template)
    <br/>

- [~~Configuring VPN 512 Templates for vSmarts~~](#configuring-vpn-512-templates-for-vsmarts)

- [~~Attaching vSmarts to the Device Template and Verification~~](#attaching-vsmarts-to-the-device-template-and-verification)

<br/>

" type="primary" %}
