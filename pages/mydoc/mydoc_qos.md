---
title: Configuring Low Latency Queuing and QoS
tags: [formatting, special_layouts]
keywords: definitions, glossaries, terms, style guided
last_updated: June 2, 2020

summary: "SD-WAN allows configuration of various QoS strategies to better support your business. Configure QoS with LLQ for VoIP traffic"
sidebar: mydoc_sidebar
permalink: mydoc_qos.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Create a Localized Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Add a Class List and a QoS Map
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configure the IPv4 ACL Policy
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Complete and apply the localized policy
    <br/>
- Apply the ACL and QoS Map
<br/>
- Activity Verification
<br/>

" type="primary" %}
</br>

While Application Aware Routing allows us to choose the path taken by traffic and switch paths based on SLA parameters, QoS strategies in SD-WAN allow packets to be marked with standard DSCP values which are then utilized to prioritize packets accordingly.

Let's assume that our Corporate VPN (VPN 10) has, among other traffic, VoIP packets flowing through it. We would want to follow some QoS strategy to ensure that these VoIP (RTP, Video and Signalling) packets are placed in a Low Latency Queue, with corresponding strategies for other types of traffic.

QoS in the SD-WAN world is implemented via Localized Policies. Differences in Localized and Centralized Policies can be found [over here](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/policies/vedge/policies-book/Policy-basics.html){:target="_blank"}_.

## Create a Localized Policy

QoS in the SD-WAN world is implemented via Localized Policies. Differences in Localized and Centralized Policies can be found [over here](https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/policies/vedge/policies-book/Policy-basics.html){:target="_blank"}.

### Add a Class List and a QoS Map

1. On the vManage GUI, click on **Configuration -> Policies** and choose the **Localized Policy** tab. Click on **Add Policy**

    ![](/images/AAR_LLQ/38_addpolqos.PNG)

2. Under **Create Groups of Interest** click on **Class Map** on the left-hand side. Click on **New Class List** and specify the Class as *Voice*. The Queue should be *0*. Click on **Save**

    ![](/images/AAR_LLQ/39_vq.PNG)

    This creates our Class List for VoIP traffic and puts the traffic in Queue 0.

3. Click on **New Class List** and create 3 more Class Lists, as shown below. Remember to hit **Save** after each Class List is created

    | Class       | Queue |
    |:-------------:|:-------:|
    | Video       | 1     |
    | BIZ-Data    | 2     |
    | Best-Effort | 3     |

    Once all the Class Lists are created, click on **Next**

    ![](/images/AAR_LLQ/40_4clnext.PNG)

4. The Class Lists are referenced in QoS Maps. Under **Configure Forwarding Classes/QoS**, make sure you're on the QoS Map tab and click on **Add QoS Map**

    ![](/images/AAR_LLQ/41_qmap.PNG)

5. Give the QoS Map a Name of *WAN-QoS* and a Description of *WAN QoS*. Click on **Add Queue**. Specify the following details and click on **Save Queue**

    | Queue | Bandwidth % | Buffer % | Scheduling                | Drops | Forwarding Class       |
    |:-------:|:-------------:|:----------:|:---------------------------:|:-------:|:------------------------:|
    | 1     | 30          | 30       | Wighted Round Robin (WRR) | Tail  | Video (Auto Populated) |

    ![](/images/AAR_LLQ/42_name_q1.PNG)

6. Click on **Add Queue** and add a couple more queues as per the table given below. Remember to click on **Save Queue** after you're done setting up the Queue

    | Queue | Bandwidth % | Buffer % | Scheduling                | Drops        | Forwarding Class             |
    |:-------:|:-------------:|:----------:|:---------------------------:|:--------------:|:------------------------------:|
    | 2     | 40          | 40       | Weighted Round Robin (WRR) | Random Early | BIZ-Data (Auto Populated)    |
    | 3     | 10          | 10       | Weighted Round Robin (WRR) | Random Early | Best-Effort (Auto Populated) |

    | ![](/images/AAR_LLQ/43_q2.PNG) |
    |:---:|
    | *Queue 2* |

    | ![](/images/AAR_LLQ/44_q3.PNG) |
    |:---:|
    | *Queue 3* |

7. The wagon wheel that shows Queue Bandwidth and Buffer allocation should change to reflect the settings in the Queues that were just created

    ![](/images/AAR_LLQ/45_bwbuff.PNG)

8. The QoS Map queues should look like the image below. Click on **Save Policy** to save your QoS Map and then click on **Next**

    ![](/images/AAR_LLQ/46_savepol.PNG)

    Notice that the Queue 0 Forwarding Class is populated as **Control**. Control network traffic (not related to VoIP) is also included in Queue0 by default.

This completes the QoS Map configuration. We will continue with building our Main Policy in the next section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Create a Localized Policy](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configure the IPv4 ACL Policy](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Complete and apply the localized policy](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Configure the IPv4 ACL Policy

1. Continuing from the QoS Map which we just built, you show now be at the **Configure Access Control Lists** page. An ACL Policy can be used for classification of traffic on the LAN. Click on **Add Access Control List Policy** and choose to **Add IPv4 ACL Policy**

    ![](/images/AAR_LLQ/47_addaclpol.PNG)

2. Give the ACL Policy a Name of *LAN-Classification* and a Description of *LAN Classification*. Click on **Add ACL Sequence** and then click on **Sequence Rule**. Make sure you're on the Match tab and click on **DSCP**. Enter a DSCP value of *46*. This specifies our match criteria

    ![](/images/AAR_LLQ/48_match.PNG)

3. Click on the **Actions** tab and make sure the **Accept** radio button is selected. Click on **Class** and select the *Voice* Class List which we created before. Click on **Save Match and Actions**

    ![](/images/AAR_LLQ/49_voicesaem.PNG)

4. Click on **Sequence Rule** and follow the same procedure to create rules as per the following table. Use the images below the table for reference (the actions tab should always have the Accept radio button selected). Make sure that you click on **Save Match and Actions** once done creating each rule

    | DSCP  | Class       |
    |:-------:|:-------------:|
    | 34    | Video       |
    | 26    | BIZ-Data    |
    |    Leave Blank   | Best-Effort |

    | ![](/images/AAR_LLQ/50_vid.PNG) |
    |:---:|
    | *Sequence Rule for Video* |

    | ![](/images/AAR_LLQ/51_biz.PNG) |
    |:---:|
    | *Sequence Rule for BIZ-Data* |

    | ![](/images/AAR_LLQ/52_be.PNG) |
    |:---:|
    | *Sequence Rule for Best-Effort* |

5. Verify that the Access Control List Policy looks like the image below (i.e. you should see 4 sequence rules, one for each Class List with the corresponding DSCP values as match conditions) and click on **Save Access Control List Policy**

    ![](/images/AAR_LLQ/53_versave.PNG)

6. Click on **Next** twice and you should be at the **Policy Overview** page, which continues in the next section.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Create a Localized Policy](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Complete and apply the localized policy](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Complete and apply the localized policy

1. Continuing from the previous section, while on the **Policy Overview** page, give your policy a Name of *QoS_Policy* and a Description of QoS Policy. Under **Policy Settings**, put a check mark next to **Application** and set the Log Frequency to 30 (this will come into play if you are going through the SD-AVC configuration section). Click on **Save Policy**

    ![](/images/AAR_LLQ/54_namesaved.PNG)

2. Navigate to **Configuration -> Templates** and locate the *cedge_dualuplink_devtemp* Device Template. Click on the three dots next to it and choose to **Edit**. Click on **Additional Templates**

    ![](/images/AAR_LLQ/55_edit.PNG)

3. Populate *QoS_Policy* in the **Policy** drop down. If you have gone through the Guest DIA configuration, note that this will break Guest DIA functionality. In the real world, the QoS Policy we configured should be included within the same policy. Click on **Update**

    ![](/images/AAR_LLQ/56_putqospol.PNG)

4. Click on **Next** and then **Configure Devices**. You can view the side by side configuration, if you want to

    ![](/images/AAR_LLQ/57_sbs.PNG)

We have completed application of the QoS Policy for our Device. This will create the QoS Maps and inject the corresponding Queues in the Scheduler.

{% include tip.html content="vManage pushes the forwarding class names as Queue0, Queue1 etc. along with the created Class Names. Queue0, Queue1 etc. are the ones which are actually used in the qos-map but the settings are based on the defined class names (e.g. Voice, Video, BIZ-Data etc. for our lab). This is expected behaviour. Additionally, you will **NOT** see Queue 2 in the QoS policy-map interface output since that is used for Best Effort traffic by default. However, if we were to map the Queues to 0 for Voice, 1 for Video, 3 for BIZ-Data and 4 for Best-Effort, all 4 queues will show up." %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Apply the ACL and QoS Map

We have created the QoS strategy for our network, the only thing that's left is to apply and test our QoS configuration.

To apply the configuration, we will be modifying the Service VPN 10 interface such that traffic is classified on the basis of the ACL we created, in the inbound direction.

The QoS Map will be applied in the outbound direction on the WAN interfaces (INET and MPLS)

1. Navigate to **Configuration -> Templates -> Feature Tab** and locate the *cedge-vpn10-int* Feature Template. Click on the three dots next to it and choose to **Copy** the Template. Give a name of *cedge-vpn10-int-qos* to the copied template with a Description of *VPN 10 Interface Template for cEdges with QoS* and click on **Copy**

    ![](/images/AAR_LLQ/99_copy_int_temp.PNG)

2. Locate the newly copied *cedge-vpn10-int-qos* Feature Template and click on the three dots next to it. Choose to **Edit** the template. Make sure the Description is updated and scroll down to the ACL/QoS section. Set **Ingress ACL - IPv4** to a Global value of **On** and enter *LAN-Classification* as the **IPv4 Ingress Access List**. This needs to match with the ACL we created (case sensitive). Click on **Update**

    ![](/images/AAR_LLQ/98_acl_vpn10.PNG)

3. Navigate to the Device tab in **Configuration -> Templates** and locate the *cedge_dualuplink_devtemp*. Click on the three dots next to it and choose **Edit**

    ![](/images/AAR_LLQ/91_vpn10int.PNG)

4. In the **Service VPN** section, click on the three dots next to the *cedge-vpn10* Template and choose **Edit**

    ![](/images/AAR_LLQ/90_servedit.PNG)

5. Change the template under **Cisco VPN Interface Ethernet** to *cedge-vpn1-int-qos* and click on **Save**

    ![](/images/AAR_LLQ/89_intsave.PNG)

6. Click on **Next** and choose to **Configure Devices**. The side-by-side configuration can be viewed and we should see the *LAN-Classification* ACL being applied on GigabitEthernet4 (Service VPN Interface for VPN 10) in the incoming direction

    ![](/images/AAR_LLQ/88_sbs.PNG)

7. Head back over to **Configuration -> Template -> Feature Tab** and locate the *cedge-vpn0-int-dual* template. Click on the three dots next to it and click **Edit**. We will be updating the VPN 0 Internet interface with the QoS Map we created before

    ![](/images/AAR_LLQ/97_vpn0.PNG)

8. Under the **ACL/QOS** section, specify the **QoS Map** as a Global value and enter *WAN-QoS* (case sensitive, should match with the QOS Map we created before). Click on **Update**

    ![](/images/AAR_LLQ/96_qos_vpn0.PNG)

9. Click on **Next** and then **Configure Devices**. If you want, inspect the side-by-side configuration before clicking on **Configure Devices** and you will notice that the WAN-QoS Policy will be applied to GigabitEthernet2 (WAN VPN 0 Interface for INET)

    ![](/images/AAR_LLQ/95_sbs.PNG)

10. Under the **Configuration -> Template -> Feature Tab** locate the *cedge-vpn0-int-dual_mpls* template. Click on the three dots next to it and click **Edit**. We will be updating the VPN 0 MPLS interface with the QoS Map we created before

    ![](/images/AAR_LLQ/94_vpn0_mpls_qos.PNG)

11. Under the **ACL/QOS** section, specify the **QoS Map** as a Global value and enter *WAN-QoS* (case sensitive, should match with the QOS Map we created before). Click on **Update**

    ![](/images/AAR_LLQ/93_mplsqos.PNG)

12. Click on **Next** and then **Configure Devices**. If you want, inspect the side-by-side configuration before clicking on **Configure Devices** and you will notice that the WAN-QoS Policy will be applied to GigabitEthernet3 (WAN VPN 0 Interface for MPLS). Check the configuration pushed by logging in to the CLI for cEdge40 via Putty and issuing `show running | sec interface Gig`. We should see the WAN_QoS policy applied under GigabitEthernet2 and GigabitEthernet3

    ![](/images/AAR_LLQ/92_qospolapply.PNG)

This completes the configuration of our QoS Policy in VPN 10 at Site 40.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [~~Apply the ACL and QoS Map~~](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Activity Verification

1. Log in to vCenter (use the bookmark or go to 10.2.1.50/ui) using the credentials provided to you. Locate the sdwan-slc/ghi-site40pc-podX VM and click on it. Open the Web Console to the Site 40 PC VM and log in. The Username is sdwan and the password is C1sco12345. Click the network icon in the top-right corner and go to Wired Settings

    ![](/images/AAR_LLQ/87_wired.PNG)

2. Click on the cog wheel/gear icon

    ![](/images/AAR_LLQ/86_gear.PNG)

3. Click on **Remove Connection Profile**

    ![](/images/AAR_LLQ/85_remprof.PNG)

4. The **+** sign should show up next to **Wired**. If you still see a cog wheel/gear icon, click on it and choose Remove Connection Profile again. Once the **+** icon is visible, click on it

    ![](/images/AAR_LLQ/84_pluswired.PNG)

5. Go to the **IPv4** tab and set the **IPv4 Method** as Manual. Enter the following details and click on **Add**

    | Address     | Netmask       | Gateway    | DNS                                    |
    |-------------|---------------|------------|----------------------------------------|
    | 10.40.10.21 | 255.255.255.0 | 10.40.10.2 | Automatic - Off <br> <br> 10.2.1.5, 10.2.1.6 |

    ![](/images/AAR_LLQ/83_staticsave.PNG)

6. Back at the vCenter screen, right click on the Site40PC (named sdwan-slc/ghi-site40pc-podX) for your POD and click on **Edit Settings** (image as an example only)

    ![](/images/AAR_LLQ/82_editsett.PNG)

7. Under **Network Adapter 1** click on the drop down and click **Browse**

    ![](/images/AAR_LLQ/81_browse.PNG)

8. Select *Site40-VPN10* from the list of Networks and click on **OK**. Click on **OK** again.

    ![](/images/AAR_LLQ/80_s40v10okok.PNG)

9. Log in to the cEdge40 CLI via Putty and issue `clear policy-map counters`. Confirm that you want to clear the counters. Now issue a `show policy-map interface Gig2` and a `show policy-map interface Gig3`. You will notice the number of packets incrementing in Queue0 (this includes VoIP packets via configuration and Control packets by default). Run the two commands given above multiple times and take notice of Queue3 and Queue0. Queue3 should not increment, whereas Queue0 will keep incrementing

    ![](/images/AAR_LLQ/79_packetsmatched.PNG)
    ```
    show policy-map interface Gig2
    show policy-map interface Gig3
    ```

10. Go back to the Site 40 PC and open Terminal. Type `ping 10.100.10.2`. Let the pings run for a few seconds, making note of how many packets did we receive a response for (look at the icmp_seq field) and then stop the pings by pressing Ctrl + C. We let the ping run for 70 packets

    ![](/images/AAR_LLQ/78_runping.PNG)

11. Issue `show policy-map interface Gig2` and `show policy-map interface Gig3` again on the cEdge40 CLI. Queue3 in one of the outputs (depends on the path taken by the packets) should reflect an increment in the number of packets

    ![](/images/AAR_LLQ/77_70pack.PNG)

Thus, traffic is being matched as per our QoS strategy. However, we won't be able to test other queues since ESXi (the VMWare environment in which our lab is running) doesn't allow packet tags to be propagated over Standard vSwitches (the virtual switch). Queue0 shows up since this traffic is generated natively by the Router in question.

An extended ping directly from the Router yields unpredictable results, with traffic usually getting matched to class class-default (optional - you can try this out).

![](/images/AAR_LLQ/76_pingfromgw.PNG)

![](/images/AAR_LLQ/75_classdef.PNG)

This completes our QoS activity verification.


{% include warning.html content="STOP!!!! Time to take a snapshot. Refer to the Testing Procedure, Step 7 of the SOP" %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [~~Apply the ACL and QoS Map~~](#apply-the-acl-and-qos-map)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
