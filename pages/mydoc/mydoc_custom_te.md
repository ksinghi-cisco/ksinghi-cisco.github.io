---
title: Implementing Custom Traffic Engineering
tags: [publishing]
keywords: custom traffic engineering, path selection
last_updated: May 29, 2020
summary: "Influencing Path selection and facilitating custom traffic engineering in Cisco SD-WAN"
sidebar: mydoc_sidebar
permalink: mydoc_custom_te.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Deploying a Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Groups of Interest and Traffic Rules
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Applying and Activating the Policy
    <br/>
- Verification
<br/>

" type="primary" %}

## Overview

The Cisco SD-WAN solution builds a full mesh topology by default and there isn't any traffic engineering that is in place out of the box. The ability to steer application traffic per the network requirements via a specific path is something that can be achieved via data policies. We can leverage data policies to match specific traffic and send it via the preferred transport. To verify current functionality:

1. Log in to the vManage GUI and navigate to **Monitor -> Network**

    ![](/images/DataPol_TrafficEng/01_mon.PNG)

2. Click on **vEdge30** and scroll down the list on the left-hand side to **Troubleshooting**

    ![](/images/DataPol_TrafficEng/02_ve30.PNG)

    ![](/images/DataPol_TrafficEng/03_tshoot.PNG)

3. Click on **Simulate Flows**

    ![](/images/DataPol_TrafficEng/04_flow.PNG)

4. Enter *VPN - 10* as the VPN, *ge0/2* as the Source/Interface and *10.0.0.1* as the Destination IP. Click on **Simulate**

    ![](/images/DataPol_TrafficEng/05_sim.PNG)

    We find that general traffic uses all possible available transports to send data to the other side.

5. Keep all details the same, but this time choose **ftp** under Application. Click **Simulate**

    ![](/images/DataPol_TrafficEng/06_ftp.PNG)

    Once again, ftp traffic is also attempting to take all possible transports.

In our example, we will assume that the requirement is to send FTP traffic over the MPLS link (preferred).

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Setting up Groups of Interest and Traffic Rules](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Deploying a Policy

We begin by creating a Policy and identifying **Groups of Interest** (or interesting traffic). The policy is then expanded to encompass an ACL.

### Setting up Groups of Interest and Traffic Rules

1. On the vManage GUI, navigate to **Configuration -> Policies**.

    ![](/images/DataPol_TrafficEng/07_pol.PNG)

2. Under Centralized Policy, click on **Add Policy** to create a new Policy

    ![](/images/DataPol_TrafficEng/08_add.PNG)

3. We will be making use of the **Site30** Site List created before. Click on **Next** two times

    ![](/images/DataPol_TrafficEng/09_s30.PNG)

4. Make sure you are under **Configure Traffic Rules**. Click on the **Traffic Data** tab and choose to Add Policy. Click on **Create New**

    ![](/images/DataPol_TrafficEng/11_addpol.PNG)

5. Given the policy a name of *ftp-mpls* and a description of *FTP via MPLS*. Click on **Sequence Type** and choose **Traffic Engineering** as the Data Policy

    ![](/images/DataPol_TrafficEng/12_seq.PNG)

6. Click on **Sequence Rule** and choose **Application/Application Family List** as the match condition. Click on the drop-down for the Application/Application Family List and click on **New Application List**

    ![](/images/DataPol_TrafficEng/13_rule.PNG)

7. Give the Application List Name as *ftp* and select **File Transfer Protocol** and **File Transfer Protocol Data** under the **Select Application** drop down

    ![](/images/DataPol_TrafficEng/14_ftp.PNG)

8. Make sure the Application List looks like the image below and click on **Next**. We are defining the *interesting* traffic over here via this Application List

    ![](/images/DataPol_TrafficEng/15_save.PNG)

9. From  the **Application/Application Family List** drop down, choose the *ftp* Application List we just created

    ![](/images/DataPol_TrafficEng/16_choose.PNG)

10. Click on the **Actions** tab and choose **Accept**. Select **Local TLOC** and choose the **Local TLOC List: Color** as *mpls*. Set the Local TLOC List: Encapsulation to **IPSEC**. Click on **Save Match and Actions**

    ![](/images/DataPol_TrafficEng/18_ipsecsave.PNG)

11. Choose **Default Action** on the left-hand side and click on the pencil icon to edit the default action

    ![](/images/DataPol_TrafficEng/19_def.PNG)

12. Select **Accept** and click on **Save Match and Actions**

    ![](/images/DataPol_TrafficEng/20_accept.PNG)

13. Back at the Data Policy window, click on **Save Data Policy**

    ![](/images/DataPol_TrafficEng/21_save.PNG)

14. At the main Policy window, click on **Next**

    ![](/images/DataPol_TrafficEng/22_next.PNG)

Continue to the steps in the [next section](#applying-and-activating-the-policy).

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

### Applying and Activating the Policy

Continuing from the [Setting up Groups of Interest and Traffic Rules](#setting-up-groups-of-interest-and-traffic-rules), we will now finalize our policy and activate it.

1. Give the Policy a name of *traffic-engineering-ftp* and a description of *Traffic Engineering for FTP*. Click on the **Traffic Data** tab and click on **New Site List and VPN List**. Leave the **From Service** radio button selected and populate *Site30* in Select Site List and *Corporate* in the Select VPN List. Click on **Add** and then click on **Save Policy**

    ![](/images/DataPol_TrafficEng/23_savepol.PNG)

2. This should create our *traffic-engineering-ftp* policy. Click on the three dots next to it and choose **Activate**

    ![](/images/DataPol_TrafficEng/24_act.PNG)

    {% include tip.html content="At this point we have created multiple policies and are activating them as we go along. However, this is not a standard practice. At a time, only one policy can be active so all our Policy requirements are generally concatenated into a single policy. Separate policies have been created in the lab for simplicity." %}

3. Click on **Activate**

    ![](/images/DataPol_TrafficEng/25_confirm.PNG)

We have now deployed our Policy.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Verification

In order to verify that traffic flows have changed, we will be comparing the output in the [Overview](#overview) section to out put which will be taken here.

1. On the vManage GUI, go to **Monitor -> Network** and select vEdge30. Scroll down to **Troubleshooting** on the left-hand side and click on **Simulate Flows**

    ![](/images/DataPol_TrafficEng/02_ve30.PNG)

    ![](/images/DataPol_TrafficEng/03_tshoot.PNG)

    ![](/images/DataPol_TrafficEng/04_flow.PNG)

2. Enter *VPN - 10* for the **VPN** and *ge0/2* for the **Source/Interface**. The **Destination IP** will be *10.0.0.1*. Click on **Simulate**

    ![](/images/DataPol_TrafficEng/26_gentraffic.PNG)

    We can see that general traffic is still attempting to use all possible transports.

3. Set the **Application** to *ftp* and click on **Simulate**

    ![](/images/DataPol_TrafficEng/27_ftptraffic.PNG)

    FTP Traffic now flows via the MPLS transport, as per our requirement.

This completes the verification activity for this section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [~~Verification~~](#verification)
<br/>

" type="primary" %}
