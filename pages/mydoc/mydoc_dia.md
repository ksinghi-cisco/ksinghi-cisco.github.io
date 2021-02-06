---
title: Implementing Direct Internet Access
tags: [publishing]
keywords: direct internet access, guest users, dia
last_updated: June 01, 2020
summary: "Setting up a Direct Internet Access policy for Guest Users at Site 40"
sidebar: mydoc_sidebar
permalink: mydoc_dia.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Creating and Activating a Policy
<br/>
- Verification
<br/>

" type="primary" %}

## Overview
We will now shift focus to setting up our DIA site at Site40. Guest users will connect on VPN 30 and we need to ensure they have access to the Internet. We will first verify that the PC at Site 40 does not have Internet access. The WAN Interface at Site 40 on *public-internet* will then be updated for NAT and a Policy will be applied (which will include a Data Prefix list and a Data Policy) to allow users on VPN 30 to access the Internet.

1. Click on the bookmark for vCenter in Google Chrome or navigate to https://10.2.1.50/ui if connected to the GHI DC and 10.1.1.50/ui if connected to the SJC DC. Enter the credentials provided for your POD and click on **Login**

    ![](/images/DataPol_DIA/01_vc.PNG)

2. Locate the Site40 PC (it will be named *sdwan-YYY-site40pc-podX* where YYY are some characters and X is your POD number, image uses Site40_PC). Click on it and click on the icon to open a console session. Choose to open the Web Console, if prompted

    ![](/images/DataPol_DIA/02_console.PNG)

3. Navigate to the console window/tab and click on the *sdwan* user to log in. The password is *C1sco12345*

    {% include note.html content="If the machine hangs at the login window and doesn't show the Ubuntu Desktop, please power off and power on the Site40PC VM for your POD from vCenter." %}

    ![](/images/DataPol_DIA/03_ubuntu.PNG)

4. Click on the Ubuntu equivalent of the Start button - it's the button in the bottom left hand corner and search for **terminal**. Open the terminal application

    ![](/images/DataPol_DIA/04_terminal.PNG)

5. Type `ping 8.8.8.8` and hit Enter. Pings should fail

    ![](/images/DataPol_DIA/05_noint.PNG)

We have thus verified that the Guest VPN user (with an IP of 10.40.30.21) doesn't have internet access.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Creating and Activating a Policy](#creating-and-activating-a-policy)
<br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Creating and Activating a Policy
<br/>

We will start by enabling NAT on the Internet interface and then continue with our Policy.

1. On the vManage GUI, navigate to **Configuration => Templates => Feature Tab**. Locate the *cedge-vpn0-int-dual* template created before and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/DataPol_DIA/06_edittemp.PNG)

2. Scroll down to the **NAT** section and set NAT to a Global value of *On*. Click on **Update**

    ![](/images/DataPol_DIA/07_nat.PNG)

3. Click on **Next** since we don't need to change anything on the device settings and then click on **Configure Devices**. You can view the side-by-side configuration if you want to

    ![](/images/DataPol_DIA/08_next.PNG)

    ![](/images/DataPol_DIA/09_sbs.PNG)

    NAT should now be enabled on the public-internet transport

4. Navigate to **Configuration => Policies** on the vManage GUI and click on **Add Policy**

    ![](/images/DataPol_DIA/10_addpol.PNG)

5. Select **Data Prefix List** on the left-hand side under Create Groups of Interest and choose **New Data Prefix List**. Give it a name of *Guest-Site40* and specify the **Add Data Prefix** as *10.40.30.0/24*. Click on **Add** and then click on **Next** (please click on Add BEFORE clicking on Next else the Data Prefix List will not get added)

    ![](/images/DataPol_DIA/11_datapref.PNG)

    Click on **Next** on the **Configure Topology and VPN Membership** screen.

6. On the **Configure Traffic Rules** screen, click on the **Traffic Data** tab and choose **Add Policy**. Click on **Create New**

    ![](/images/DataPol_DIA/12_addpol.PNG)

7. Give the Data Policy a name of *Guest-DIA* with a Description of *Guest DIA at Site 40*. Click on **Sequence Type** and choose **Custom**

    ![](/images/DataPol_DIA/13_seq.PNG)

8. Click on **Sequence Rule** and select **Source Data Prefix** under Match. Populate *Guest-Site40* in the Source Data Prefix List (we just created this Data Prefix list)

    ![](/images/DataPol_DIA/14_match.PNG)

9. Click on the **Actions** tab and choose the **Accept** radio button. Select **NAT VPN** and click on **Save Match and Actions**

    ![](/images/DataPol_DIA/15_natvpn.PNG)

10. Click on **Default Action** over on the left-hand side and choose **Accept**. Click on **Save Match and Actions**

    ![](/images/DataPol_DIA/16_def.PNG)

11. Click on **Save Data Policy**

    ![](/images/DataPol_DIA/17_save.PNG)

12. Make sure the Data Policy we just added shows up and click on **Next**

    ![](/images/DataPol_DIA/18_next.PNG)

13. Enter the Policy Name as *Site40-Guest-DIA* and a Description of *DIA Policy for Site 40 Guests*. Click on the **Traffic Data** tab and choose **New Site List and VPN List**. Leave the radio button on *From Service* and choose *Site40* under Select Site List. Choose *Guest* under Select VPN List. Click on **Add**. Once added, click on **Save Policy**

    ![](/images/DataPol_DIA/19_savepol.PNG)

14. Locate your *Site40-Guest-DIA* and click on the three dots next to it. Choose to Activate the policy

    ![](/images/DataPol_DIA/20_act.PNG)

This completes the configuration of our DIA Policy.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating a Policy~~](#creating-and-activating-a-policy)
<br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Verification

1. To verify, log in to vCenter and Console to the Site40 PC, as enumerated in the [Overview](#overview) section. On Terminal, enter `ping 8.8.8.8`. The pings should succeed

    ![](/images/DataPol_DIA/21_intworks.PNG)

2. Click on the Mozilla Firefox icon on the Site40 PC and try to browse to sdwan-docs.cisco.com (or any other website). It should work

    ![](/images/DataPol_DIA/22_inetworks2.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating a Policy~~](#creating-and-activating-a-policy)
<br/>
- [~~Verification~~](#verification)
<br/>

" type="primary" %}
