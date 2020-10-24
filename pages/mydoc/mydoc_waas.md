---
title: Integrating Cisco WAAS with SD-WAN
keywords: waas, sdwan
last_updated: October 23, 2020
tags: [navigation]
summary: "Integrating Cisco WAAS with SD-WAN using AppNav-XE"
sidebar: mydoc_sidebar
permalink: mydoc_waas.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Adding WAAS Nodes to WCM
<br/>
- Downloading vManage certs and Enabling DIA at Site DC
<br/>
- Integrating vManage and WCM
<br/>
- Discovering the AppNav-XE Controllers
<br/>
- Setting up the AppNav Clusters
<br/>
- Verification and Testing
<br/>
" type="primary" %}

## Overview

Cisco WAAS and SD-WAN can be integrated for traffic interception and redirection to WAAS Nodes for optimization. This brings WAAS capabilities to Cisco IOS-XE SD-WAN by enabling the AppNav-XE feature on compatible devices.

Cisco SD-WAN Devices are configured with AppNav-XE redirection policies and WAAS nodes are configured with optimization policies from WAAS Central Manager (WCM).

The AppNav-XE SD-WAN Device and the WAAS Nodes together form a cluster known as an AppNav-XE cluster.

The WCM registers as a third party controller to vManage.

The components of the WAAS SD-WAN solution are:

* WAAS Central Manager (WCM) - used for centralized management of WAAS Nodes and AppNav-XE on Cisco SD-WAN Devices
<br>
* AppNav-XE Service Controller (SC) running on WAN Edges which contain redirection policies
<br>
* WAAS Nodes or Service Nodes (SN) which contain optimization policies

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Adding WAAS Nodes to WCM](#adding-waas-nodes-to-wcm)
<br/>
- [Downloading vManage certs and Enabling DIA at Site DC](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [Integrating vManage and WCM](#integrating-vmanage-and-wcm)
<br/>
- [Discovering the AppNav-XE Controllers](#discovering-the-appnav-xe-controllers)
<br/>
- [Setting up the AppNav Clusters](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Adding WAAS Nodes to WCM

1. Open the WCM GUI by navigating to the IP Address of WCM (10.100.10.100) or using the bookmark on your Jumphost and entering the credentials as enumerated below

    | Username | Password |
    | --- | --- |
    | admin | default |

    ![](/images/WaaS/image001.png)

2. Once logged in, click on **Device** and notice that there aren't any nodes added to WCM as of now. We will be adding the WAAS Nodes to WCM in this section

    ![](/images/WaaS/image003.png)

3. Open vCenter (10.2.1.50/ui or via the bookmark) and log in using the credentials provided for your POD. Locate the *sdwan-ghi-site40waas* VM and click on it. Click on the **Open Console** icon and choose **Web Console** if prompted. Click on **OK**. Initial setup of the WAAS Nodes is done via the CLI

    ![](/images/WaaS/image005.png)

    ![](/images/WaaS/image007.png)

4. Enter the username and password as enumerated below to log in to the WAAS Node

    | Username | Password |
    | --- | --- |
    | admin | default |

    ![](/images/WaaS/image009.png)

5. Type **setup** and hit Enter to begin initial setup of the WAAS Node

    ![](/images/WaaS/image011.png)

6. Press any key to continue

    ![](/images/WaaS/image013.png)

7. We will now be presented with a few ways in which the node can be configured. Type *n* to begin configuring all parameters of the WAAS Node. In some of the prompts during this setup, you **don't** need to press Enter for the input to take effect

    ![](/images/WaaS/image015.png)

8. At the *Select Interception Method* type *2* to set the interception method to AppNav. This is the only supported interception method in SD-WAN WAAS

    ![](/images/WaaS/image017.png)

9. Hit **Enter** at the *Enter Time Zone* prompt to choose the default time zone of UTC

    ![](/images/WaaS/image019.png)

10. Hit **Enter** at the *Select Management Interface* prompt to select *Virtual 1/0* as the management interface

    ![](/images/WaaS/image021.png)

11. Type *n* at the *Enable DHCP for Management Interface* to disable DHCP on the selected management interface. We will be entering an IP Address manually

    ![](/images/WaaS/image023.png)

12. Hit **Enter** at the *Enable IPv6 on Device* prompt such that IPv6 is not enabled

    ![](/images/WaaS/image025.png)

13. Type an IP Address of *10.40.10.101/24* and hit **Enter**

    ![](/images/WaaS/image027.png)

14. Type a Default Gateway address of *10.40.10.2* and hit **Enter**

    ![](/images/WaaS/image029.png)

15. Type *10.100.10.100* at the *Enter Central Manager IP Address* prompt and hit **Enter**

    ![](/images/WaaS/image031.png)

16. Hit **Enter** at the *Enter Domain Name Server IP Address* prompt. We will not be using DNS

    ![](/images/WaaS/image033.png)

17. Type *swatsdwanlab.com* at the *Enter Domain Name(s)* prompt and hit **Enter**

    ![](/images/WaaS/image035.png)

18. Type *Site40-WaaS* as the Hostname and hit **Enter**

    ![](/images/WaaS/image037.png)

19. Hit **Enter** when asked about the license to accept the default of Enterprise Licenses

    ![](/images/WaaS/image039.png)

20. Hit **Enter** to implement the configuration changes

    ![](/images/WaaS/image041.png)

21. Hit **Enter** again

    ![](/images/WaaS/image043.png)

22. Once the connectivity check to WCM passes, hit **Enter** to skip running diagnostics

    ![](/images/WaaS/image045.png)

    ![](/images/WaaS/image047.png)

23. Repeat from Step 3 for the *sdwan-ghi-site50waas* node, making necessary changes to the Hostname and IP Address/Default Gateway. All other parameters remain the same. Reference the image given below

    ![](/images/WaaS/image049.png)

24. Log in to WCM and navigate to **Devices** - you should see the two WAAS Nodes we just configured on WCM

    ![](/images/WaaS/image051.png)

    ![](/images/WaaS/image053.png)

25. If you click on **All Devices**, you will see the *Site40-WaaS* and *Site50-WaaS* nodes in an online state. If there are alarms for the Device Status, it's OK since Core Dump files are generated sometimes

    ![](/images/WaaS/image054.png)

This completes the addition of WAAS Nodes on WCM.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [Downloading vManage certs and Enabling DIA at Site DC](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [Integrating vManage and WCM](#integrating-vmanage-and-wcm)
<br/>
- [Discovering the AppNav-XE Controllers](#discovering-the-appnav-xe-controllers)
<br/>
- [Setting up the AppNav Clusters](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Downloading vManage certs and Enabling DIA at Site DC

Go through the following steps in order to configure Cloud OnRamp for SaaS in our SD-WAN network.

1. On the vManage GUI, navigate to **Administration => Settings**

    ![](/images/Cloud_OnRamp/11_set.PNG)

2. Locate the **Cloud onRamp for SaaS** section and click on **Edit**. Set the radio button to **Enabled** and click on **Save**. Cloud OnRamp for SaaS needs to be enabled system wide before it can be used

    ![](/images/Cloud_OnRamp/12_en.PNG)

3. Once enabled, click on the **Cloud** icon in the top right-hand of the screen and click on **Cloud onRamp for SaaS**

    ![](/images/Cloud_OnRamp/13_cloud.PNG)

4. Click on **Dismiss**

    ![](/images/Cloud_OnRamp/14_dism.PNG)

5. Click on **Manage Cloud onRamp for SaaS** (top right-hand corner) and click on **Applications**

    ![](/images/Cloud_OnRamp/15_app.PNG)

6. Specify a random application (example shows Amazon AWS, but you can choose something else like Oracle or Google Apps) and populate a **VPN** of *10*

    ![](/images/Cloud_OnRamp/16_aws.PNG)

7. Make sure the chosen Application shows up and click on **Save Changes**

    ![](/images/Cloud_OnRamp/17_save.PNG)

8. Click on **Cloud onRamp for SaaS** (top right-hand corner) again and click on **Direct Internet Access (DIA) Sites**

    ![](/images/Cloud_OnRamp/18_dia.PNG)

9. Click on **Attach DIA Sites** and move Site 30 to the **Selected Sites** section. Click on **Attach**

    ![](/images/Cloud_OnRamp/19_attdia.PNG)

10. Wait for the task to go through successfully. Once it is done, click on the **Cloud** icon in the top right corner and click **Cloud onRamp for SaaS**

    ![](/images/Cloud_OnRamp/20_succcloud.PNG)

11. Click on **Manage Cloud onRamp for SaaS** and choose Gateways

    ![](/images/Cloud_OnRamp/21_gw.PNG)

12. Click on **Attach Gateways** and move Site *1* to the Selected Sites. Click on **Attach**

    ![](/images/Cloud_OnRamp/22_attgw.PNG)

13. If you go to **Configuration => Cloud OnRamp for SaaS** (or click the Cloud icon and go to Cloud onRamp for SaaS), you should see the selected Application with 3 Devices attached to it. Click on the Application and the three Devices should be tagged with a vQoE Status of Bad. Their vQoE score is 0.0, indicating that information hasn't been collected to arrive at a score. We will need to wait for some time (another tea/coffee?)

    ![](/images/Cloud_OnRamp/23_waitforcalc.PNG)

14. If you refresh the screen, you should notice devices gradually showing up with their vQoE score. Notice that vEdge30 is selecting a local path to the selected Application

    ![](/images/Cloud_OnRamp/24_ve30loccalc.PNG)

    ![](/images/Cloud_OnRamp/25_allgood.PNG)

    Through the DIA configuration, we have provided vEdge30 with a local breakout to the Application and by adding Site 1 as the Gateway, traffic can be punted over the MPLS link to the DC site and sent out the Internet breakout there, in the event of the local Site30 Internet breakout facing issues.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [~~Downloading vManage certs and Enabling DIA at Site DC~~](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [Integrating vManage and WCM](#integrating-vmanage-and-wcm)
<br/>
- [Discovering the AppNav-XE Controllers](#discovering-the-appnav-xe-controllers)
<br/>
- [Setting up the AppNav Clusters](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Integrating vManage and WCM

1. Navigate to **Configuration => Template => Feature Tab** and locate the *vEdge30_INET* template. Click on the three dots next to it and choose to **Edit**

    ![](/images/Cloud_OnRamp/27_ve30inet.PNG)

2. Scroll down to the **ACL/QOS** section and specify a **Shaping Rate (Kbps)** of *1*. This will inject delay on our INET link connected to vEdge30. Click on **Update**

    ![](/images/Cloud_OnRamp/28_shaper.PNG)

3. Click on **Next/Configure Devices**. You can check the side-by-side configuration to see that the shaping rate is applied to interface ge0/0

    ![](/images/Cloud_OnRamp/29_sbs.PNG)

4. Wait for some time and traffic to the chosen Application from vEdge30 (check via Cloud icon => Cloud onRamp for SaaS => click on the Application) should have a DIA status of **gateway**, indicating that the DC Gateway is being used to contact Amazon AWS (in this example). The local/remote color is *mpls* with the system-ip of the gateway being used

    ![](/images/Cloud_OnRamp/30_redierect.PNG)

    The vQoE score might vary, as shown in the image below (it usually takes approximately 15 to 20 minutes for the expected results to show up)

    ![](/images/Cloud_OnRamp/123_vqoemightvary.PNG)

5. Go back to the *vEdge30-INET* Feature template (refer to Steps 1 and 2 of this section) and set the **Shaping Rate (Kbps)** to the Default value. Click on **Update**. Click on **Next/Configure Devices**

    ![](/images/Cloud_OnRamp/31_def.PNG)

6. Navigate to **Monitor => Network** and click on **Tunnel**. Make sure all the public-internet Tunnel Endpoints are selected. You should see the latency on the link drop

    ![](/images/Cloud_OnRamp/32_afterremoving.PNG)

7. Cloud OnRamp for SaaS takes a few minutes to converge, so monitor the **Cloud icon => Cloud onRamp for SaaS => Application** page - in time, you should see vEdge30 sending data via the local internet breakout

    ![](/images/Cloud_OnRamp/33_backtoloc.PNG)

    ![](/images/Cloud_OnRamp/34_allgood.PNG)

This completes the Cloud OnRamp for SaaS lab.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [~~Downloading vManage certs and Enabling DIA at Site DC~~](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [~~Integrating vManage and WCM~~](#integrating-vmanage-and-wcm)
<br/>
- [Discovering the AppNav-XE Controllers](#discovering-the-appnav-xe-controllers)
<br/>
- [Setting up the AppNav Clusters](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Discovering the AppNav-XE Controllers

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [~~Downloading vManage certs and Enabling DIA at Site DC~~](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [~~Integrating vManage and WCM~~](#integrating-vmanage-and-wcm)
<br/>
- [~~Discovering the AppNav-XE Controllers~~](#discovering-the-appnav-xe-controllers)
<br/>
- [Setting up the AppNav Clusters](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Setting up the AppNav Clusters

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [~~Downloading vManage certs and Enabling DIA at Site DC~~](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [~~Integrating vManage and WCM~~](#integrating-vmanage-and-wcm)
<br/>
- [~~Discovering the AppNav-XE Controllers~~](#discovering-the-appnav-xe-controllers)
<br/>
- [~~Setting up the AppNav Clusters~~](#setting-up-the-appnav-clusters)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Verification and Testing

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Adding WAAS Nodes to WCM~~](#adding-waas-nodes-to-wcm)
<br/>
- [~~Downloading vManage certs and Enabling DIA at Site DC~~](#downloading-vmanage-certs-and-enabling-dia-at-site-dc)
<br/>
- [~~Integrating vManage and WCM~~](#integrating-vmanage-and-wcm)
<br/>
- [~~Discovering the AppNav-XE Controllers~~](#discovering-the-appnav-xe-controllers)
<br/>
- [~~Setting up the AppNav Clusters~~](#setting-up-the-appnav-clusters)
<br/>
- [~~Verification and Testing~~](#verification-and-testing)
<br/>
" type="primary" %}
