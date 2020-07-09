---
title: Integrating Cisco SD-WAN and Umbrella
tags: [special_layouts]
keywords: knowledge base, support portal, grid, doc portal
last_updated: June 3, 2020
summary: "Cisco SD-WAN Security with Umbrella integration."
sidebar: mydoc_sidebar
permalink: mydoc_umbrella.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Pre-Work
<br/>
- Enabling Site 30 for DIA
<br/>
- Life without Cisco Umbrella
<br/>
- Basic Configuration for Umbrella
<br/>
- Making Umbrella Ours
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- API Keys and AD Configuration
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- DC Configuration Download
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- AD Connectors
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Roaming Computer Configuration
    <br/>
- Building a DNS Policy
<br/>
- Setting up IPSEC Tunnels
<br/>
- Configuring a Firewall Policy
<br/>
- Configuring a Web Policy
<br/>

" type="primary" %}

## Overview

Cisco Umbrella offers flexible, cloud-delivered security when and how you need it. It combines multiple security functions into one solution, so you can extend protection to devices, remote users, and distributed locations anywhere. Umbrella is the easiest way to effectively protect your users everywhere in minutes.

The Umbrella portfolio includes, among others, the following Security functions:

* DNS Layer Security

* Cloud-delivered Firewall (IPSEC Tunnel)

* Secure Web Gateway (IPSEC Tunnel)

In this section, we will deploy DNS Layer Security as an Umbrella feature and then see how SD-WAN can simplify Tunnel creation and Cloud-Delivered Firewall/SWG functionality.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Pre-Work](#pre-work)
<br/>
- [Enabling Site 30 for DIA](#enabling-site-30-for-dia)
<br/>
- [Life without Cisco Umbrella](#life-without-cisco-umbrella)
<br/>
- [Basic Configuration for Umbrella](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [API Keys and AD Configuration](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Pre-Work

We will need to change a few settings with respect to the DNS servers to ensure that the Umbrella infrastructure isn't utilized by the SD-WAN solution. As of now, all DNS traffic is being queried via the Umbrella resolvers.

1. Connect to the Site 30 PC to verify that Site to Site communication is operational but the Internet cannot be accessed. Log in to Guacamole (10.2.1.20X:8080/guacamole, where X is your POD number) with the credentials given below and click on the PODX-Site30PC option.

    Alternatively, you can RDP to 10.2.1.16X (where X is your POD number) from the Jumphost. RDP to the Site 30 PC will only work from the Jumphost

    | Connection Method | Username | Password |
    | --- | ---- | ---- |
    | Guacamole | sdwanpod | C1sco12345 |
    | RDP | swatsdwanlab\sdwan | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/01_guac.PNG)

    ![](/images/Umbrella_SDWAN_2/01_site30pc.PNG)

    vCenter (accessible via the bookmark or 10.2.1.50/ui and the credentials provided for your POD) can also be used to console to the Site30 PC

    ![](/images/Umbrella_SDWAN_2/01_vcc.PNG)

2. Depending on the connection method, you may need to enter credentials again to log in to the Site 30 PC. Please enter the credentials shown below, if prompted

    | Connection Method | Username | Password |
    | --- | ---- | ---- |
    | Guacamole | Not Required | Not Required |
    | RDP | swatsdwanlab\sdwan | C1sco12345 |
    | vCenter | swatsdwanlab\sdwan | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/02_login.PNG)

3. Click on **Start** and type **cmd**. Click on the *Command Prompt* App that pops up in the search results

    ![](/images/Umbrella_SDWAN_2/03_cmd.PNG)

4. Type `ipconfig` and Hit Enter. Also, type `ping 10.0.0.1` and Hit Enter. The pings should work. On typing `ping 8.8.8.8`, the pings should fail indicating that there is no Internet connectivity

    ![](/images/Umbrella_SDWAN_2/04_ipping.PNG)

    ![](/images/Umbrella_SDWAN_2/05_noint.PNG)

    ```
    ipconfig
    ping 10.0.0.1
    ping 8.8.8.8
    ```

5. Go to the vManage GUI and navigate to **Configuration => Templates**

    ![](/images/Umbrella_SDWAN_2/06_temp.PNG)

6. Click on the **Feature** tab and locate the *vEdge30-vpn0* Feature Template. Click on the three dots next to it and choose to **Edit**

    ![](/images/Umbrella_SDWAN_2/07_edit.PNG)

7. Scroll to the **DNS** section and update the **Primary DNS Address (IPv4)** to *8.8.8.8* and the **Secondary DNS Address (IPv4)** to *4.2.2.2*

    ![](/images/Umbrella_SDWAN_2/08_dns.PNG)

8. Locate the **IPv4 Route** section and click on the pencil icon to edit the **0.0.0.0/0** route

    ![](/images/Umbrella_SDWAN_2/09_pencil.PNG)

9. Click on **2 Next Hop** and remove the *vpn0_mpls_next_hop* option by clicking on the red minus icon

    ![](/images/Umbrella_SDWAN_2/10_2nh.PNG)

    ![](/images/Umbrella_SDWAN_2/11_remmpls.PNG)

10. Click on **Save Changes**

    ![](/images/Umbrella_SDWAN_2/12_save.PNG)

11. Ensure that the **Update IPv4 Route** window shows **1 Next Hop** and click on **Save Changes**

    ![](/images/Umbrella_SDWAN_2/13_save.PNG)

12. Click on **New IPv4 Route** and enter a Prefix of *192.0.2.0/24*. Click on **Add Next Hop**

    ![](/images/Umbrella_SDWAN_2/14_nr.PNG)

13. Click on **Add Next Hop** again

    ![](/images/Umbrella_SDWAN_2/15_anh.PNG)

14. Enter a Global value of *192.0.2.13* in the **Address** field and click on **Add**

    ![](/images/Umbrella_SDWAN_2/16_nh.PNG

15. Click on **Add** again to add the route

    ![](/images/Umbrella_SDWAN_2/17_add.PNG)

16. We will be adding 2 more routes. Repeat steps 12 to 15 for the routes enumerated below, using the images as reference

    | Field                  | Global or Device Specific (Drop Down) | Value        |
    |------------------------|---------------------------------------|--------------|
    | Prefix                 | Global                                | 192.1.2.0/24 |
    | Add Next Hop - Address | Global                                | 192.0.2.13   |

    | Field                  | Global or Device Specific (Drop Down) | Value        |
    |------------------------|---------------------------------------|--------------|
    | Prefix                 | Global                                | 192.168.26.0/24 |
    | Add Next Hop - Address | Global                                | 192.0.2.13   |

    ![](/images/Umbrella_SDWAN_2/18_another.PNG)

    ![](/images/Umbrella_SDWAN_2/19_nh.PNG)

17. Make sure there are 4 routes created, as shown below and click on **Update**

    ![](/images/Umbrella_SDWAN_2/20_allroutesupdate.PNG)

18. Click on **Next** and then **Configure Devices**. You can view the side by side configuration difference, if required. Notice that the default route pointing to the MPLS next hop is being removed and 3 routes are being added in place of it

    ![](/images/Umbrella_SDWAN_2/21_sbs.PNG)

19. Navigate to the **Configuration => Templates => Feature tab** and click on the three dots next to *vedge30_MPLS*. Click on **Edit**

    ![](/images/Umbrella_SDWAN_2/22_edit.PNG)

20. Under Tunnel, set the **Control Connection** to *Off* and click on **Update**. Click on **Next** and then **Configure Devices**

    ![](/images/Umbrella_SDWAN_2/23_ccoff.PNG)

21. Back at the **Configuration => Templates => Feature tab**, locate the *vEdge30_INET* Feature Template. Click on the three dots next to it and choose to **Edit**. Set **NAT** to a Global value of *On* and click on **Update**. Click **Next** and **Configure Devices** on the corresponding screens, viewing the side by side configuration difference if required

    ![](/images/Umbrella_SDWAN_2/24_inetnat.PNG)

    ![](/images/Umbrella_SDWAN_2/25_nat.PNG)

    ![](/images/Umbrella_SDWAN_2/26_sbs.PNG)

22. We will now add a VPN 10 Template for vEdge30 since there will be settings applicable just to this Site for Umbrella connectivity. On **Configuration => Templates => Feature tab** locate the *vedge-vpn10* Template. Click on the three dots next to it and choose **Copy**

    ![](/images/Umbrella_SDWAN_2/27_copyvpn10.PNG)

23. Rename the Template to *vedge30-vpn10* and update the description accordingly. Click on **Copy**

    ![](/images/Umbrella_SDWAN_2/28_ren.PNG)

24. Click on the three dots next to the newly copied template and choose to **Edit**

    ![](/images/Umbrella_SDWAN_2/29_editvpn10.PNG)

25. Update the **DNS** entries to *8.8.8.8* for the **Primary DNS Address (IPv4)** and *4.2.2.2* for the **Secondary DNS Address (IPv4)**. Click on **Update**.

    ![](/images/Umbrella_SDWAN_2/30_dns.PNG)

26. On the vManage GUI, navigate to **Configuration => Templates => Device Tab** and locate the *vEdge30_dev_temp* Template. Click on the three dots next to it and choose to **Edit** the template

    ![](/images/Umbrella_SDWAN_2/33_editdevtemp.PNG)

27. In the **Service VPN** section, select the *vedge-vpn10* Template Name entry and click on **Remove VPN**. Confirm the removal

    ![](/images/Umbrella_SDWAN_2/34_remvpn.PNG)

    ![](/images/Umbrella_SDWAN_2/35_confirm.PNG)

28. Click on **Add VPN** under Service VPN and move the *vedge30-vpn10* Template to the right hand side. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/36_addvpn.PNG)

29. Under **Additional VPN Templates** click on *VPN Interface* and select *vedge-vpn10-int* in the **VPN Interface** drop-down. Click on **Add**

    ![](/images/Umbrella_SDWAN_2/37_vpnintadd.PNG)

30. Back at the Device Template, click on **Update** followed by **Next** and **Configure Devices**

    ![](/images/Umbrella_SDWAN_2/38_upd_nxt_conf.PNG)

31. Log in to the CentralGW via the saved Putty session (or SSH to 192.168.0.1) using the credentials below. Enter `config t` followed by `interface gig 2.31` and then `ip nat inside` to allow the VPN 10 subnet at Site 30 to be NAT'd. Type `do wr` to save the configuration done on the CentralGW

    | Username | Password |
    | --- | --- |
    | admin | admin |

    <br>

    ![](/images/Umbrella_SDWAN_2/39_cgw.PNG)

    ![](/images/Umbrella_SDWAN_2/40_natins.PNG)

    ```
    config t
    interface gig 2.31
    ip nat inside
    do wr
    ```

This completes the pre-work that we needed to do at Site 30.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [Enabling Site 30 for DIA](#enabling-site-30-for-dia)
<br/>
- [Life without Cisco Umbrella](#life-without-cisco-umbrella)
<br/>
- [Basic Configuration for Umbrella](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [API Keys and AD Configuration](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Enabling Site 30 for DIA

To facilitate 

1. On the vManage GUI, go to **Monitor => Network** and click on cEdge40. Choose **Troubleshooting** from the left-hand column and click on **Simulate Flows**. Enter the VPN as *VPN - 10* and the Source/Interface as *GigabitEthernet4*. Set a Destination IP of *10.100.10.2* and click on **Simulate**. We find that traffic is taking all possible transports, just like before. This is expected since we haven't defined anything for regular traffic

    ![](/images/AAR_LLQ/13_regtraf.PNG)

2. On the same screen, click on **Advanced Options** and set the DSCP to *46*. Click on **Simulate**

    ![](/images/AAR_LLQ/14_dscptraf.PNG)

    VoIP Traffic is now traversing the MPLS link as the preferred route. We see just one path in the image above - if 10.255.255.11 (DC-vEdge1) is rebooted and this tested again, traffic will go through 10.255.255.12 (DC-vEdge2). If Simulate is clicked multiple times, traffic does take different paths.

3. We will now check the current network statistics. On the left-hand side go to **Tunnel** and put a check mark against all the *mpls* Tunnel Endpoints. Click on Real-Time after scrolling up to the chart and make sure Packet Loss/Latency is checked under **Chart Options**. We may see negligible packet loss occurring (let the chart run for 5 minutes before analysing, it should get updated every few seconds)

    ![](/images/AAR_LLQ/16_minorloss.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [Life without Cisco Umbrella](#life-without-cisco-umbrella)
<br/>
- [Basic Configuration for Umbrella](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [API Keys and AD Configuration](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Life without Umbrella

In order to simulate impairment in the network (Packet Loss and Latency), we can use a Policer and a Shaper. Over here, we will configure a Policer which will be applied to the MPLS link in order to simulate Packet Loss.

Later on, we will leverage a Shaper to simulate Latency.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [Basic Configuration for Umbrella](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [API Keys and AD Configuration](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Basic Configuration for Umbrella

1. On the vManage GUI, navigate to **Configuration => Policies**. Click on **Custom Options** (top right-hand corner). Under **Localized Policy** click on **Lists**

    ![](/images/AAR_LLQ/17_lppolicer.PNG)

2. Click on **Policer** (left-hand side) to create Policer configuration which will simulate network impairment on our MPLS link (Packet Loss). Click on **New Policer List** and give it a name of *AAR-Impair-Policer-PL*. Specify the **Burst** as *15000* and **Exceed** as *Drop*. The **Rate** should be *7000*. Click on **Add**

    | Field             | Value                 |
    |-------------------|-----------------------|
    | Policer List Name | AAR-Impair-Policer-PL |
    | Burst (bps)       | 15000                 |
    | Exceed            | Drop                  |
    | Rate (bps)        | 7000                  |

    ![](/images/AAR_LLQ/18_polic.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [API Keys and AD Configuration](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Making Umbrella ours

1. Go to the **Localized Policy** tab and click on **Add Policy**

    ![](/images/AAR_LLQ/19_addpol.PNG)

2. Click **Next** till you are at the **Configure Access Control Lists** page. Click on **Add Access Control List Policy** and choose **Add IPv4 ACL Policy**

    ![](/images/AAR_LLQ/20_acl.PNG)

3. Enter a name of *Impair-PL-AAR* with a Description of *Impairment ACL*. Click on **Add ACL Sequence** and click on **Sequence Rule**. Go to the **Actions** tab and make sure the Accept radio button is selected. Choose **Policer** and select the *AAR-Impair-Policer-PL* we created before. Click on **Save Match and Actions**. Refer to the table and image below

    | Step | Field                  | Value                 |
    |------|------------------------|-----------------------|
    | 1    | Name                   | Impair-PL-AAR         |
    |      | Description            | Impairment ACL        |
    | 2    | Add ACL Sequence       |                       |
    | 3    | Sequence Rule          |                       |
    | 4    | Actions                |                       |
    | 5    | Policer                |                       |
    | 6    | Policer                | AAR-Impair-Policer-PL |
    | 7    | Save Match and Actions |                       |

    ![](/images/AAR_LLQ/21_polacl.PNG)

4. Click on **Save Access Control List Policy**

    ![](/images/AAR_LLQ/22_save.PNG)

5. On the **Policy Overview** page (this is our Main Policy), enter a Policy Name of *Policer-AAR-Impairment* and a Description of Injecting Impairment for AAR via a Policer - Packet Loss. Click on **Save Policy**

    ![](/images/AAR_LLQ/23_savepol.PNG)

We have completed configuration of our Policer. It needs to be applied to a link in order to simulate network impairment.

### API Keys and AD Configuration

1. Navigate to **Configuration => Templates => Feature Tab** and locate the *cedge-vpn0-int-dual_mpls* VPN Interface template. Click on the 3 dots next to it and choose to **Copy**

    ![](/images/AAR_LLQ/26_copyint.PNG)

2. Rename it t0 *cedge-vpn0-int-dual_mpls-impair* and a Description *cEdge VPN 0 Interface Template for Devices with a dual uplink - MPLS with Impairment*. Click on **Copy**

    ![](/images/AAR_LLQ/27_copyname.PNG)

3. Click on the three dots next to this newly copied template and click on **Edit**

    ![](/images/AAR_LLQ/28_edit.PNG)

4. Navigate to the ACL/QoS section and modify the following fields. Click on **Update**

    | Field                    | Global or Device Specific (drop down) | Value         |
    |--------------------------|---------------------------------------|---------------|
    | Ingress ACL - IPv4       | Global                                | On            |
    | IPv4 Ingress Access List | Global                                | Impair-PL-AAR |
    | Egress ACL - IPv4        | Global                                | On            |
    | IPv4 Egress Access List  | Global                                | Impair-PL-AAR |

    ![](/images/AAR_LLQ/29_aclupd.PNG)

5. Under **Configuration => Templates** go to the **Device** tab and locate the *cedge_dualuplink_devtemp* template. Click on the three dots next to it and choose to **Edit**

    ![](/images/AAR_LLQ/30_editdevtemp.PNG)

6. Under Transport & Management VPN, update the **Cisco VPN Interface Ethernet** from *cedge-vpn0-int-dual_mpls* to *cedge-vpn0-int-dual_mpls-impair*. Make sure this is done on the VPN interface for the MPLS link

    ![](/images/AAR_LLQ/31_mplsimpair.PNG)

7. Scroll down to the **Additional Templates** section and update the **Policy** to *Policer-AAR-Impairment*. Click on **Update**. Click on **Next**

    ![](/images/AAR_LLQ/25_update_next_confdev.PNG)

8. You can choose to view the Side by Side or simply click on **Configure Devices**

    ![](/images/AAR_LLQ/32_sbs.PNG)

This completes the implementation of our Policer on the MPLS link to simulate network impairment.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [DC Configuration Download](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

### DC Configuration Download

1. Navigate to **Monitor => Network** and click on cEdge40. Click on **Tunnel** on the left-hand side and make sure all the **MPLS** Tunnel Endpoint entries are selected, with the public-internet entries being unchecked. Click on **Real Time** (top right corner) and the Chart Options drop-down (top left corner) is set to Loss Percentage/FEC Loss Recovery Rate. Let this run for a few minutes - you will notice a spike in Packet Loss

    ![](/images/AAR_LLQ/33_pl.PNG)

2. Head over to **Troubleshooting** (left-hand side, might need to scroll down) and click on **Simulate Flows**. Enter the **VPN** as *VPN - 10*, the **Source/Interface** as *GigabitEthernet4* and the **Destination IP** as *10.100.10.2*. Click on **Simulate**. There should be no change in traffic flow for General traffic, which will still use all available transports

    ![](/images/AAR_LLQ/34_regtrafsame.PNG)

3. Under **Advanced Options**, set DSCP to a value of *46* and click on **Simulate**. You will notice that VoIP traffic (i.e. DSCP 46) is now taking the Internet path since MPLS doesn't conform to the SLA requirements that we defined. Compare the current traffic flow with the one in Step 2 [over here](#viewing-modified-traffic-flows-and-current-network-statistics)

    ![](/images/AAR_LLQ/35_plaar.PNG)

4. We will now revert the configuration to what it was pre-impairment. Go to **Configuration => Templates** and locate the *cEdge_dualuplink_devtemp*. Click on the three dots next to it and **Edit**. Change the Cisco VPN Interface Ethernet value under **Transport & Management VPN** back to *cedge-vpn0-int-dual_mpls* and click on **Update**

    ![](/images/AAR_LLQ/36_rev.PNG)

5. Wait for approximately 3 minutes and head over to **Monitor => Network => cEdge40 => Troubleshooting => Traffic Flows**. Enter the same details as in Step 3 above and click on **Simulate**. VoIP traffic should traverse over the MPLS link again

    ![](/images/AAR_LLQ/37_waitmpls.PNG)

This completes the Application Aware Routing section of the lab.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [AD Connectors](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

### AD Connectors

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [Making Umbrella Ours](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Roaming Computer Configuration](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

### Roaming Computer Configuration

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [~~Making Umbrella Ours~~](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Roaming Computer Configuration~~](#roaming-computer-configuration)
    <br/>
- [Building a DNS Policy](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Building a DNS Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [~~Making Umbrella Ours~~](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Roaming Computer Configuration~~](#roaming-computer-configuration)
    <br/>
- [~~Building a DNS Policy~~](#building-a-dns-policy)
<br/>
- [Setting up IPSEC Tunnels](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Setting up IPSEC Tunnels

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [~~Making Umbrella Ours~~](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Roaming Computer Configuration~~](#roaming-computer-configuration)
    <br/>
- [~~Building a DNS Policy~~](#building-a-dns-policy)
<br/>
- [~~Setting up IPSEC Tunnels~~](#setting-up-ipsec-tunnels)
<br/>
- [Configuring a Firewall Policy](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Configuring a Firewall Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [~~Making Umbrella Ours~~](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Roaming Computer Configuration~~](#roaming-computer-configuration)
    <br/>
- [~~Building a DNS Policy~~](#building-a-dns-policy)
<br/>
- [~~Setting up IPSEC Tunnels~~](#setting-up-ipsec-tunnels)
<br/>
- [~~Configuring a Firewall Policy~~](#configuring-a-firewall-policy)
<br/>
- [Configuring a Web Policy](#configuring-a-web-policy)
<br/>

" type="primary" %}

## Configuring a Web Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Pre-Work~~](#pre-work)
<br/>
- [~~Enabling Site 30 for DIA~~](#enabling-site-30-for-dia)
<br/>
- [~~Life without Cisco Umbrella~~](#life-without-cisco-umbrella)
<br/>
- [~~Basic Configuration for Umbrella~~](#basic-configuration-for-umbrella)
<br/>
- [~~Making Umbrella Ours~~](#making-umbrella-ours)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~API Keys and AD Configuration~~](#api-keys-and-ad-configuration)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~DC Configuration Download~~](#dc-configuration-download)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~AD Connectors~~](#ad-connectors)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Roaming Computer Configuration~~](#roaming-computer-configuration)
    <br/>
- [~~Building a DNS Policy~~](#building-a-dns-policy)
<br/>
- [~~Setting up IPSEC Tunnels~~](#setting-up-ipsec-tunnels)
<br/>
- [~~Configuring a Firewall Policy~~](#configuring-a-firewall-policy)
<br/>
- [~~Configuring a Web Policy~~](#configuring-a-web-policy)
<br/>

" type="primary" %}
