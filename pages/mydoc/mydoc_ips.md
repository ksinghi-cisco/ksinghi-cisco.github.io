---
title:  Installing and Configuring the IPS module on cEdges
tags: [content_types]
keywords: groups, api, structure
last_updated: June 3, 2020
summary: "Installing an IPS Engine on cEdges and testing signature detection for DIA Guest users"
sidebar: mydoc_sidebar
permalink: mydoc_ips.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Initial Configuration
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Revert Site 40 PC changes and enable DIA
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Upload Image to vManage
    <br/>
- Add the Security Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Firewall Policy Update
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Add the IPS Policy and Finalize the Security Policy
    <br/>
- Updating the Application List and Device Template
<br/>
- Verifying installation and performing signature updates
<br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview

An Intrusion Prevention System (IPS) allows the network to detect anomalies based on known signatures and block/report them. The IPS module in Cisco SD-WAN can be deployed on Cisco IOS-XE SD-WAN Devices, working in Detect or Prevention mode. This solution is an on-prem on-box feature providing PCI compliance.

Snort is leveraged on Cisco SD-WAN IOS-XEW Devices for IPS and IDS capabilities.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Initial Configuration](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Revert Site 40 PC changes and enable DIA](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Upload Image to vManage](#upload-image-to-vmanage)
    <br/>
- [Add the Security Policy](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Firewall Policy Update](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Add the IPS Policy and Finalize the Security Policy](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [Updating the Application List and Device Template](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Initial Configuration
<br>
We will be performing some initial configuration in the network before it can support the IPS module. Key points to be noted:

* The cEdge should have a minimum of 4 vCPUs and 8 GB RAM (already done)
* Site 40 PC settings will be reverted
* Images uploaded to vManage for deployment

### Revert Site 40 PC changes and enable DIA

1. Log in to vCenter via the bookmark in Chrome, or via the URL (10.2.1.50/ui). Use the credentials provided for your POD. Click on **Login**

    ![](/images/SD-WAN_Security_IPS/99_vc.PNG)

2. Locate your Site 40 PC (image below shows Site40_PC, VM name for your POD should be sdwan-slc/ghi-site40pc-podX) and choose to open the console. Select Web Console, if prompted

    ![](/images/SD-WAN_Security_IPS/98_zconsole.PNG)

3. Log in to the PC and click on the network icon in the top-right corner. Expand **Wired Connected** and click on **Wired Settings**

    ![](/images/SD-WAN_Security_IPS/98_wired.PNG)

4. Click on the cog wheel/gear icon

    ![](/images/SD-WAN_Security_IPS/97_cog.PNG)

5. Click on **Remove Connection Profile**

    ![](/images/SD-WAN_Security_IPS/96_remcp.PNG)

6. Click on the *+* next to Wired

    ![](/images/SD-WAN_Security_IPS/95_plus.PNG)

7. Go to the **IPv4** tab and click on **Manual** for the IPv4 Method. Enter details as given below and click on **Add**. Over here, y is *1* if you're connected to the SLC DC and *2* if you're connected to the GHI DC. The email sent with lab access details should enumerate which DC you're POD is on

    | Address     | Netmask       | Gateway    | DNS                                    |
    |-------------|---------------|------------|----------------------------------------|
    | 10.40.30.21 | 255.255.255.0 | 10.40.30.2 | Automatic - Off <br> <br> 10.y.1.5, 10.y.1.6 |

    ![](/images/SD-WAN_Security_IPS/94_ip.PNG)

8. On the vManage GUI, go to **Configuration -> Policies** and locate the *Site40-Guest-DIA*. Click on the three dots next to it and choose to **Activate**. Confirm the Activation

    ![](/images/SD-WAN_Security_IPS/01_act.PNG)

    ![](/images/SD-WAN_Security_IPS/02_conf.PNG)

9. Go back to the console for the Site 40 PC and open Terminal. (**Start -> search for terminal -> click on the icon**). Type `ping 8.8.8.8` and hit Enter to verify Internet connectivity

    ![](/images/SD-WAN_Security_IPS/03_int.PNG)

We have set the Site 40 PC back to what it was, before our QoS section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Initial Configuration](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Upload Image to vManage](#upload-image-to-vmanage)
    <br/>
- [Add the Security Policy](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Firewall Policy Update](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Add the IPS Policy and Finalize the Security Policy](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [Updating the Application List and Device Template](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Upload Image to vManage

1. On the vManage GUI, go to **Maintenance -> Software Repository**

    ![](/images/SD-WAN_Security_IPS/04_sw.PNG)

2. Click on the **Virtual Images** tab and then click **Upload Virtual Image**. Choose **vManage**

    ![](/images/SD-WAN_Security_IPS/05_up.PNG)

3. Click on **Browse** and make sure you're in the *SD-WAN Deployment Files* folder. This folder can be found on the Desktop of your Jumphost. Select the file starting with *secapp-utd...* and click on **Open**

    ![](/images/SD-WAN_Security_IPS/06_brose.PNG)

    ![](/images/SD-WAN_Security_IPS/07_utd.PNG)

4. Click on **Upload**

    ![](/images/SD-WAN_Security_IPS/08_upl.PNG)

5. Once the file is uploaded, it should show up under Virtual Images

    ![](/images/SD-WAN_Security_IPS/09_uplded.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [Add the Security Policy](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Firewall Policy Update](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Add the IPS Policy and Finalize the Security Policy](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [Updating the Application List and Device Template](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Add the Security Policy

A Security Policy will be applied to the Device Template for cEdge40 to trigger IPS installation and functionality. We will be setting up the policy over here, including the previously created Firewall Policy in our overarching Security Policy.

### Firewall Policy Update

1. On the vManage GUI, navigate to **Configuration -> Security** and choose **Add Security Policy**. Select **Custom** and click on **Proceed**

    ![](/images/SD-WAN_Security_IPS/10_custom.PNG)

2. Under **Firewall**, click on **Add Firewall Policy** and choose **Copy from Existing**. We already have a Firewall Policy in place but the Security Policy type chosen for it was Guest Access, which doesn't have an option of including an IPS policy. Hence, we will create a new custom policy which will include the Firewall Policy created before

    ![](/images/SD-WAN_Security_IPS/11_copyfe.PNG)

3. Select *Guest-FW* as the Policy and specify the Policy Name as *Guest-FW_concat*. Give a Description of *Guest Traffic Firewall with IPS*. Click on **Copy**

    ![](/images/SD-WAN_Security_IPS/12_cop.PNG)

4. The Firewall Policy we just copied should show up. Click on **Next**

    ![](/images/SD-WAN_Security_IPS/13_next.PNG)

Configuration of the Security Policy continues in the next section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [Add the Security Policy](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Firewall Policy Update~~](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Add the IPS Policy and Finalize the Security Policy](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [Updating the Application List and Device Template](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Add the IPS Policy and Finalize the Security Policy

1. Under the **Intrusion Prevention** page, click on **Add Intrusion Prevention** and choose **Create New**

    ![](/images/SD-WAN_Security_IPS/14_ipspol.PNG)

2. Click on **Target VPNs** and enter a VPN of *30*. Click on **Save Changes**

    ![](/images/SD-WAN_Security_IPS/15_vpn30.PNG)

3. Under the Intrusion Prevention - Policy Rule Configuration, enter the following details and click on **Save Intrusion Prevention Policy**

    | Policy Name | Signature Set | Inspection Mode | Alerts Log Level |
    |-------------|---------------|-----------------|------------------|
    | *Guest-IPS* | Security      | Detection       | Info             |

    ![](/images/SD-WAN_Security_IPS/16_pol.PNG)

4. Back at the main Security Policy page, click on **Next** 5 times

    ![](/images/SD-WAN_Security_IPS/17_next5.PNG)

5. Enter the details as shown in the table below and click on **Save Policy**

    | Security Policy Name | Security Policy Description | TCP SYN Flood Limit | Audit Trail |
    |----------------------|-----------------------------|---------------------|-------------|
    | *Guest-FW-IPS-DIA*   | Guest Firewall and IPS DIA  | Enabled <br> 5000   | On          |

    ![](/images/SD-WAN_Security_IPS/18_save.PNG)

This completes the configuration of our Security Policy.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [~~Add the Security Policy~~](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Firewall Policy Update~~](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add the IPS Policy and Finalize the Security Policy~~](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [Updating the Application List and Device Template](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Updating the Application List and Device Template

The Application List attached to the Firewall Policy that we had earlier will need to be instantiated again before we can use it. For that, we will make a dummy modification to the Application List

1. On the vManage GUI, go to **Configuration -> Security**. Click on **Custom Lists** (top right-hand corner) and choose **Lists**

    ![](/images/SD-WAN_Security_IPS/93_list.PNG)

2. Click on the three dots next to the *Guest-Inspect* Application List and click on the **pencil** icon to edit it. Under **Select Application**, check **X Font Server** (or any application that you want, this is a dummy entry)

    ![](/images/SD-WAN_Security_IPS/18_xdummyapplistupdate.PNG)

3. Scroll down the list and **uncheck** Webmail, but check all the other Applications under Webmail

    ![](/images/SD-WAN_Security_IPS/92_app.PNG)

4. Click outside the box and choose to **Save** the Application List

    ![](/images/SD-WAN_Security_IPS/18_ydummyapplsagain.PNG)

5. Go to **Configuration -> Templates** and click on the three dots next to *cedge_dualuplink_devtemp*. Click on **Edit**

    ![](/images/SD-WAN_Security_IPS/19_editdevtemp.PNG)

6. Navigate to the **Additional Templates** section and populate the **Security Policy** field with the policy we just created - *Guest-FW-IPS-DIA*. Click on **Update**

    ![](/images/SD-WAN_Security_IPS/20_edittempup.PNG)

7. Click on **Next** and you can choose to view the side-by-side configuration. Click on **Configure Devices**. If you do choose to view the configuration, notice the UTD related commands being pushed by vManage - they are for the IPS module

    ![](/images/SD-WAN_Security_IPS/21_sbs.PNG)

8. The status of this change will show up as **Done - Scheduled**. This is expected since the IPS engine has to be installed on the cEdge

    ![](/images/SD-WAN_Security_IPS/22_appinstsched.PNG)

9. Navigate to **Configuration -> Devices** and locate the cEdge40 Device. You will notice that the Device Status is **Service Install Pending** (might have to scroll to the right or remove columns to see this)

    ![](/images/SD-WAN_Security_IPS/23_devstat_removecolumns.PNG)

Since it takes approximately 5 minutes for the install process to go through, this will be a perfect time to grab a cup of tea/coffee! We will validate the installation in the next section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [~~Add the Security Policy~~](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Firewall Policy Update~~](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add the IPS Policy and Finalize the Security Policy~~](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [~~Updating the Application List and Device Template~~](#updating-the-application-list-and-device-template)
<br/>
- [Verifying installation and performing signature updates](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Verifying installation and performing signature updates

1. After you're done with the cup of tea/coffee, check the **Configuration -> Devices** page again. cEdge40 should now be **In Sync**

    ![](/images/SD-WAN_Security_IPS/24_insync.PNG)

2. Log in to the CLI of cEdge40 via Putty and enter the `show utd engine standard status` command. The **Overall system status** should be *Green* and the Engine should be *Running*. If the **Signature** is version *29.0.c*, proceed to the next step else skip to [Activity Verification](#activity-verification)

    ![](/images/SD-WAN_Security_IPS/25_utdstat.PNG)
    ```
    show utd engine standard status
    ```

3. To update the signatures, run the command `copy scp: bootflash:`. Details to be entered are given below, confirm the signature update

    | Address or name of remote host | Source username | Source filename                   | Destination filename              | Password |
    |:--------------------------------:|:-----------------:|:-----------------------------------:|:-----------------------------------:|:----------:|
    | 100.100.100.1                  | admin           | UTD-STD-SIGNATURE-29130-115-S.pkg | UTD-STD-SIGNATURE-29130-115-S.pkg | admin    |

    ![](/images/SD-WAN_Security_IPS/26_copysigcentgw.PNG)

    ![](/images/SD-WAN_Security_IPS/27_instsign.PNG)
    ```
    copy scp: bootflash:
    ```

4. Run `show utd engine standard status` to check if the signature package version matches with the image below

    ![](/images/SD-WAN_Security_IPS/28_succ.PNG)
    ```
    show utd engine standard status
    ```

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [~~Add the Security Policy~~](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Firewall Policy Update~~](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add the IPS Policy and Finalize the Security Policy~~](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [~~Updating the Application List and Device Template~~](#updating-the-application-list-and-device-template)
<br/>
- [~~Verifying installation and performing signature updates~~](#verifying-installation-and-performing-signature-updates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}




## Activity Verification

1. Log in to vCenter and console in to your Site 40 PC again, like before ([click here](#revert-site-40-pc-changes-and-enable-dia) to review the process). Open **Terminal** and type `ping 8.8.8.8` to verify that Internet connectivity is still there

    ![](/images/SD-WAN_Security_IPS/29_pingworking.PNG)

2. Still in Terminal, run `.\ips.sh` to trigger a few HTTP connections which will trigger the IPS

    ![](/images/SD-WAN_Security_IPS/30_ipssh.PNG)

3. Back at the cEdge40 CLI, issue `show utd engine standard logging events`. You should see alerts triggered as a result of running the ips.sh file (this file attempts to download some simulated malware). Thus, our IPS engine is working as expected

    ![](/images/SD-WAN_Security_IPS/31_sign.PNG)

4. We can view this information on the vManage GUI as well. Go to **Dashboard -> Security** and you should see some Signature hits. The dashboard does take some time to get populated, though (it's never too soon for another cup of tea/coffee!)

    ![](/images/SD-WAN_Security_IPS/32_signhits.PNG)

This completes the verification activity.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Initial Configuration~~](#initial-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Revert Site 40 PC changes and enable DIA~~](#revert-site-40-pc-changes-and-enable-dia)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Upload Image to vManage~~](#upload-image-to-vmanage)
    <br/>
- [~~Add the Security Policy~~](#add-the-security-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Firewall Policy Update~~](#firewall-policy-update)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add the IPS Policy and Finalize the Security Policy~~](#add-the-ips-policy-and-finalize-the-security-policy)
    <br/>
- [~~Updating the Application List and Device Template~~](#updating-the-application-list-and-device-template)
<br/>
- [~~Verifying installation and performing signature updates~~](#verifying-installation-and-performing-signature-updates)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
