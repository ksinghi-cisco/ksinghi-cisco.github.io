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

1. On the vManage GUI, navigate to **Configuration -> Templates**

    ![](/images/DC-vEdge_Templates/01_conftemp.PNG)

2. Click on the **Feature** tab and click on **Add Template**

    ![](/images/DC-vEdge_Templates/02_feataddtemp.PNG)

3. Search for *vedge* in the search box and put a check mark next to **vEdge Cloud**. This will give the options to select Feature Templates applicable to the selected device type. Click on **VPN** to start configuring a VPN Template. This is going to be our VPN Template for VPN 0

    ![](/images/DC-vEdge_Templates/03_vecloudvpn.PNG)

4. Give the Template a name of *DCvEdge-vpn0* and a description of *VPN0 for the DC-vEdges INET and MPLS link*

    ![](/images/DC-vEdge_Templates/03_ztempnamedesc.PNG)

5. Under **Basic Configuration**, specify the VPN as 0 (zero)

    ![](/images/DC-vEdge_Templates/04_tempnamedescvpn.PNG)

6. Populate the Primary and Secondary DNS Address as 10.2.1.5 and 10.2.1.6 respectively. Set the drop down to **Global** in order to enter the IPs. The option to enter the Secondary DNS server will pop up once the Primary is populated

    ![](/images/DC-vEdge_Templates/05_dns.PNG)

7. Under **IPv4 Route**, click on **New IPv4 Route** and specify the Prefix as Global. Populate *0.0.0.0/0* as the prefix and click on **Add Next Hop**

    ![](/images/DC-vEdge_Templates/07_ipv4route.PNG)

8. Click on **Add Next Hop** again in the popup window

    ![](/images/DC-vEdge_Templates/08_addhop.PNG)

9. From the drop down, set the value to Device Specific and enter the key as *vpn0_inet_next_hop*

    ![](/images/DC-vEdge_Templates/09_nexthopinet.PNG)

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

The sidebar.html file (inside the \_includes folder) contains the `.navgoco` method called on the `#mysidebar` element.

There are some options to set within the `.navgoco` method. The only noteworthy option is `accordion`. This option makes it so when you expand a section, the other sections collapse. It's a way of keeping your navigation controls condensed.

The value for `accordion` is a Boolean (`true` or `false`). By default, the `accordion` option is set as `true`. If you don't want the accordion, set it to `false`. Note that there's also a block of code near the bottom of sidebar.html that is commented out. Uncomment out that section to have the Collapse all and Expand All buttons appear.

There's a danger with setting the accordion to `false`. If you click Expand All and the sidebar expands beyond the dimensions of the browser, users will be stuck. When that happens, it's hard to collapse it. As a best practice, leave the sidebar's accordion option set to `true`.

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

The sidebar has one other feature &mdash; this one from Bootstrap. If the user's viewport is tall enough, the sidebar remains fixed on the page. This allows the user to scroll down the page and still keep the sidebar in view.

In the customsscripts.js file in the js folder, there's a function that adds an `affix` class if the height of the browser window is greater than 800 pixels. If the browser's height is less than 800 pixels, the `nav affix` class does not get inserted. As a result, the sidebar can slide up and down as the user scrolls up and down the page.

Depending on your content, you may need to adjust `800` pixel number. If your sidebar is so long that having it in a fixed position makes it so the bottom of the sidebar gets cut off, increase the `800` pixel number here to a higher number.

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

In the attributes for each sidebar item, if you use `external_url` instead of `url`, the theme will insert the link into an `a href` element that opens in a blank target.

For example, the sidebar.html file contains the following code:

{% raw %}
```liquid
{% if folderitem.external_url %}
    <li><a href="{{folderitem.external_url}}" target="_blank" rel="noopener">{{folderitem.title}}</a></li>
```
{% endraw %}

You can see that the `external_url` is a condition that applies a different formatting. Although this feature is available, I recommend putting any external navigation links in the top navigation bar instead of the side navigation bar.

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

The sidebar.html file inserts an `active` class into the sidebar element when the `url` attribute in the sidebar data file matches the page URL.

For example, the sidebar.html file contains the following code:

{% raw %}
```liquid
{% elsif page.url == folderitem.url %}
   <li class="active"><a href="{{folderitem.url | remove: "/"}}">{{folderitem.title}}</a></li>
```
{% endraw %}

If the `page.url` matches the `subfolderitem.url`, then an `active` class gets applied. If not, the `active` class does not get applied.

The `page.url` in Jekyll is a site-wide variable. If you insert {% raw %}`{{page.url}}`{% endraw %} on a page, it will render as follows: {{page.url}}. The `url` attribute in the sidebar item must match the page URL in order to get the `active` class applied.

This is why the `url` value in the sidebar data file looks something like this:

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
