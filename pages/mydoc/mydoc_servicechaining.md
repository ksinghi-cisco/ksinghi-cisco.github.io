---
title: Inter VPN Routing and Service Chaining
permalink: mydoc_servicechaining.html
tags: [publishing, single_sourcing, content_types]
keywords: inter VPN, Service Chaining, Firewall
last_updated: July 21, 2020
summary: "Implementing Inter VPN Routing and Service Chaining (Firewall)."
sidebar: mydoc_sidebar
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Configure VPN 40 on DC-vEdges
<br/>
- Configuration Cleanup and Routing Verification
<br/>
- Setting up VPN Lists
<br/>
- Inter VPN Routing Policies
<br/>
- Inter VPN Routing Verification
<br/>
- Policies for Service Chaining
<br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview
Since we have users on the Guest network accessing the Internet through the DIA VPN, we might want to lock down what they can/cannot access. Cisco SD-WAN has an in-built Zone Based Firewall which can perform Deep Packet Inspection, allowing and/or blocking/inspecting traffic as need be. While this is a slightly stripped down version of a ZBF, it is quite robust in functionality and offers an intuitive GUI (in the form of vManage) for deploying Firewall Rules.

In this section we will be configuring and deploying a Zone Based Firewall in our network. Guest users will be able to access most Web content but they won't be able to access Web based emails (like Gmail). We will see the corresponding activity on the ZBF in the CLI and on the GUI.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Configure VPN 40 on DC-vEdges](#configure-vpn-40-on-dc-vedges)
<br/>
- [Configuration Cleanup and Routing Verification](#configuration-cleanup-and-routing-verification)
<br/>
- [Setting up VPN Lists](#setting-up-vpn-lists)
<br/>
- [Inter VPN Routing Policies](#inter-vpn-routing-policies)
<br/>
- [Inter VPN Routing Verification](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Configure VPN 40 on DC-vEdges

We start off by configuring a few Lists that form the building blocks of our ZBF. The following lists will be created

* Zone List for identifying the Guest and Outside zones
* Application List for identifying webmail traffic and allowing all other TCP traffic to ports 80 and 443

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [Configuration Cleanup and Routing Verification](#configuration-cleanup-and-routing-verification)
<br/>
- [Setting up VPN Lists](#setting-up-vpn-lists)
<br/>
- [Inter VPN Routing Policies](#inter-vpn-routing-policies)
<br/>
- [Inter VPN Routing Verification](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Configuration Cleanup and Routing Verification

1. On the vManage GUI, go to **Configuration => Security**

    ![](/images/AppFW_DIA/01_sec.PNG)

2. Click on **Custom Options** in the top right corner of the screen and click on **Lists**

    ![](/images/AppFW_DIA/02_lists.PNG)

3. Click on **Zones** on the left-hand side and choose to create a **New Zone List**. Give the Zone List Name as *Guest* and Add VPN as *30*. Click on **Add**

    ![](/images/AppFW_DIA/03_zl.PNG)

4. Click on **New Zone List** again and give the Zone List Name as *Outside*. Specify the Add VPN as *0*. Click on **Add**

    ![](/images/AppFW_DIA/04_out.PNG)

5. Make sure that there are two Zone Lists in the configuration and move to the next section of the guide (while staying on the same page)

    ![](/images/AppFW_DIA/05_2zl_app.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [Setting up VPN Lists](#setting-up-vpn-lists)
<br/>
- [Inter VPN Routing Policies](#inter-vpn-routing-policies)
<br/>
- [Inter VPN Routing Verification](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Setting up VPN Lists

1. From the previous section, click on **Application** in the top left corner of the screen after verifying that both the Zone Lists are visible

    ![](/images/AppFW_DIA/05_2zl_app.PNG)

2. Once Application is selected, click on **New Application List** and give the Application List Name of *Guest-Inspect*. Choose *Webmail* from the drop down, making sure all the sub-items under webmail are selected as well

    ![](/images/AppFW_DIA/06_app.PNG)

3. Click on **Add** to add this Application List

    ![](/images/AppFW_DIA/07_add.PNG)

We have created an Application List which can potentially identify Gmail, Mail.ru etc. traffic. We will now create our policy.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [~~Setting up VPN Lists~~](#setting-up-vpn-lists)
<br/>
- [Inter VPN Routing Policies](#inter-vpn-routing-policies)
<br/>
- [Inter VPN Routing Verification](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Inter VPN Routing Policies

1. On the vManage GUI, navigate to **Configuration => Security** and click on **Add Security Policy**

    ![](/images/AppFW_DIA/08_addsec.PNG)

2. Choose **Guest Access** and click on Proceed

    ![](/images/AppFW_DIA/09_guest.PNG)

3. Under Firewall, choose to **Add Firewall Policy**. Click on *Create New*

    ![](/images/AppFW_DIA/10_fw.PNG)

4. Click on **Apply Zone Pairs**

    ![](/images/AppFW_DIA/11_azp.PNG)

5. Set the **Source Zone** as *Guest* and the **Destination Zone** as *Outside*. Click on **Save**

    ![](/images/AppFW_DIA/12_szp.PNG)

6. Ensure that *Guest* appears under Sources and *Outside* appears under Destinations. Give the Policy a name of *Guest-FW* and a Description of *Guest Traffic Firewall**. Click on **Add Rule**

    ![](/images/AppFW_DIA/13_seq.PNG)

7. Click on **Source Data Prefix** and choose *Guest-Site40* as the **IPv4 Prefix List**. Click on the Green **Save** button (be careful, don't click on the Blue Save button)

    ![](/images/AppFW_DIA/14_sdp.PNG)

8. Click on **Application List** and select the *Guest-Inspect* list we created. Click on the Green **Save** button (again, please don't click on the Blue Save button)

    ![](/images/AppFW_DIA/15_applist.PNG)

9. Give the Firewall Rule a name of *Inspect Web App Guest* and set the Action as **Inspect**. Click on **Save** (this time, we click the Blue Save button). Ensure that the Source Data Prefix and the Application List is populated

    ![](/images/AppFW_DIA/16_ins.PNG)

10. Click on **Add Rule** again and select the **Source Data Prefix** IPv4 Prefix List as *Guest-Site40*. Click on the Green **Save** button

    ![](/images/AppFW_DIA/20_sdp.PNG)

11. Click on **Destination Ports** and set the Destination Ports as *80 443* (there is a space between the port numbers). Click on the Green **Save** button

    ![](/images/AppFW_DIA/18_port.PNG)

12. Make sure the Firewall Rule looks like the image below and specify a Name of *TCP Guest Pass Web*. Specify the **Action** as *Pass* and put a check mark against Log. Click on the Blue **Save** button

    ![](/images/AppFW_DIA/21_save.PNG)

13. Make sure the Firewall Policy looks as below and click on **Save Firewall Policy**

    ![](/images/AppFW_DIA/22_savefwpol.PNG)

14. Click on **Next** and then **Next** again at the URL Filtering and TLS/SSL Decryption sections

    ![](/images/AppFW_DIA/23_next.PNG)

    ![](/images/AppFW_DIA/24_next_urlflater.PNG)

    ![](/images/AppFW_DIA/25_next.PNG)

15. At the Policy Summary page, give a Security Policy Name of *Site40-Guest-DIA* and a Description of *Guest Policy for Site 40*. Under Additional Policy Settings set the TCP SYN Flood Limit to Enabled and 5000. Enable **Audit Trail** as well and click on **Save Policy**

    ![](/images/AppFW_DIA/26_save.PNG)

This completes the process of creating the Security Policy.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [~~Setting up VPN Lists~~](#setting-up-vpn-lists)
<br/>
- [~~Inter VPN Routing Policies~~](#inter-vpn-routing-policies)
<br/>
- [Inter VPN Routing Verification](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Inter VPN Routing Verification

1. Go to **Configuration => Templates** and click on the three dots next to the *cEdge_dualuplink_devtemp* Device Template. Choose to **Edit** it

    ![](/images/AppFW_DIA/27_devtemp.PNG)

2. Under the **Additional Templates** section, populate the **Security Policy** as *Site40-Guest-DIA* and click on **Update**

    ![](/images/AppFW_DIA/28_secpol_upd.PNG)

3. Choose **Next** and then **Configure Devices** to push the Security Policy to cEdge40

    ![](/images/AppFW_DIA/29_next.PNG)

    ![](/images/AppFW_DIA/30_sbs.PNG)

4. Open the Console session to the Site 40 PC (log in to vCenter => locate the site40pc VM and open the Web Console) and navigate to www.facebook.com. It should work indicating that Web Traffic is allowed. Log in to the cEdge40 CLI via Putty and issue a `show logg`. We should see some activity there

    ![](/images/AppFW_DIA/31_fb.PNG)

    ![](/images/AppFW_DIA/32_snipshlog.PNG)

5. Open up a few tabs on the Site 40 PC (2 to 3 of them) and try to access www.gmail.com on all tabs. This should fail

    ![](/images/AppFW_DIA/33_gmailnw.PNG)

6. On the vManage GUI, navigate to **Dashboard => Security** and you should see spikes in the Firewall Enforcement dashlet (continue with the lab and check back after approximately 15 minutes to see this)

    ![](/images/AppFW_DIA/34_dashsec.PNG)

Thus, our ZBF is working as expected, blocking webmail traffic on the Guest VPN while allowing other traffic on ports 80 and 443.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [~~Setting up VPN Lists~~](#setting-up-vpn-lists)
<br/>
- [~~Inter VPN Routing Policies~~](#inter-vpn-routing-policies)
<br/>
- [~~Inter VPN Routing Verification~~](#inter-vpn-routing-verification)
<br/>
- [Policies for Service Chaining](#policies-for-service-chaining)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Policies for Service Chaining

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [~~Setting up VPN Lists~~](#setting-up-vpn-lists)
<br/>
- [~~Inter VPN Routing Policies~~](#inter-vpn-routing-policies)
<br/>
- [~~Inter VPN Routing Verification~~](#inter-vpn-routing-verification)
<br/>
- [~~Policies for Service Chaining~~](#policies-for-service-chaining)
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
- [~~Configure VPN 40 on DC-vEdges~~](#configure-vpn-40-on-dc-vedges)
<br/>
- [~~Configuration Cleanup and Routing Verification~~](#configuration-cleanup-and-routing-verification)
<br/>
- [~~Setting up VPN Lists~~](#setting-up-vpn-lists)
<br/>
- [~~Inter VPN Routing Policies~~](#inter-vpn-routing-policies)
<br/>
- [~~Inter VPN Routing Verification~~](#inter-vpn-routing-verification)
<br/>
- [~~Policies for Service Chaining~~](#policies-for-service-chaining)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
