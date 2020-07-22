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

Additionally, we will be working on the Site 30 PC which is part of an AD domain (swatsdwanlab.com). The Domain Controller is at 10.30.10.50, which is also acting as the DNS server for the Site 30 PC.

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

To facilitate communication to the Internet from Site 30, we will be enabling DIA at Site 30 for VPN 10.

1. On the vManage GUI, go to **Configuration => Policies**

    ![](/images/Umbrella_SDWAN_2/41_confpol.PNG)

2. Click on **Custom Options** in the top right-hand corner and click on **Traffic Policy**

    ![](/images/Umbrella_SDWAN_2/42_custop.PNG)

3. Click on the **Traffic Data** tab and locate the *Guest-DIA* Policy. Click on the three dots next to it and choose to **Edit**

    ![](/images/Umbrella_SDWAN_2/43_editdia.PNG)

4. Update the **Description** to *Guest DIA at Site 40 and Site 30* and make sure you're on the **Custom** Sequence Type. Click on **Sequence Rule** to add a new rule and select **Source Data Prefix** under Match (might need to use the scroll buttons so that the option becomes visible). Enter a *Source: IP Prefix* of *10.30.10.0/24* and click on **Actions**

    ![](/images/Umbrella_SDWAN_2/44_upddesc_nr_sdp.PNG)

5. Select the **Accept** radio button and choose **NAT VPN**. Click on **Save Match and Actions** to save this rule

    ![](/images/Umbrella_SDWAN_2/45_natvpn.PNG)

6. Make sure that there are two rules under the Custom Sequence Type. One rule is for Site 40 DIA and the other is for Site 30 VPN 10 (10.30.10.0/24) DIA. Click on **Save Data Policy**

    ![](/images/Umbrella_SDWAN_2/46_savedp.PNG)

7. Click on **Activate** and then **Configure Devices**. Confirm the configuration change and click on **OK**

    ![](/images/Umbrella_SDWAN_2/47_act.PNG)

    ![](/images/Umbrella_SDWAN_2/48_confdev.PNG)

    ![](/images/Umbrella_SDWAN_2/49_conf.PNG)

8. Once the configuration change has been pushed successfully, navigate to **Configuration => Policies** and click on the three dots next to the *Site40-Guest-DIA* policy. Choose to **Edit** it. Make sure you're on the **Policy Application** page and click on the **Traffic Data** tab. Click on **New Site List and VPN List**. Leave the *From Service* radio button checked and click on the **Select Site List** box. Choose *Site30*. Click on the **Select VPN List** box and choose *Corporate*. Click on **Add**. Click on **Save Policy Changes** to save the changes we just made

    ![](/images/Umbrella_SDWAN_2/50_editpol_trafficdata_newsitevpn_savepol.PNG)

9. Choose to **Activate** the configuration

    ![](/images/Umbrella_SDWAN_2/51_act.PNG)

10. Go to the Site 30 PC via your chosen connection method (Guacamole/RDP/vCenter Console) and open Command Prompt (Start => type cmd => click on Command Prompt). Type `ping 8.8.8.8` and hit Enter. Pings should work. To verify DNS resolution, type `ping www.cisco.com` and hit Enter

    ![](/images/Umbrella_SDWAN_2/52_ping.PNG)

We have enabled DIA at Site 30 for VPN 10. This will be used to showcase DNS security provided by Umbrella. Once we proceed through the lab activity and have set up Tunnels to Umbrella, the DIA configuration will be removed to force traffic out the tunnels.

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

As of now, the Site 30 PC has connectivity to the Internet and is pointing to the DNS Server of *10.30.10.50*. DNS Queries sent to this DNS Server are redirected to 8.8.8.8 or 4.2.2.2. We will run a quick check from our Site 30 PC to verify that we are NOT connected to Cisco Umbrella as of now.

1. Access the Site 30 PC via your preferred method (Guacamole/RDP/vCenter Console). [Click here](#pre-work){:target="_blank"} and go through Step 1 to review how to connect to the Site 30 PC. Open a browser of your choice (Firefox and Chrome should be available) and go to welcome.umbrella.com. You can also use the bookmark for **Umbrella Test**

    ![](/images/Umbrella_SDWAN_2/53_oops.PNG)

    The Umbrella page should display the image shown above. This is an indication that our network isn't protected by Umbrella (yet).

    If using Firefox, make sure to change the browser **Options** for Privacy and Security, setting Firefox to **Never remember history**. This will require a browser restart

    ![](/images/Umbrella_SDWAN_2/72_xffxchanges.PNG)

2. Access websites like www.amazon.com, www.ebay.com and www.yahoo.com by typing them out in the browser or by using the handy bookmarks available. All the sites should be accessible since we don't have any sort of access control/filtering enabled as of now

    ![](/images/Umbrella_SDWAN_2/55_amazon.PNG)

    ![](/images/Umbrella_SDWAN_2/54_ebay.PNG)

    ![](/images/Umbrella_SDWAN_2/56_yahoo.PNG)

3. Access internetbadguys.com by typing it out in the browser or using the bookmark. This is a website that simulates a phishing attack. Since we aren't protected, the website pops right up

    ![](/images/Umbrella_SDWAN_2/57_malware.PNG)

Life without Umbrella doesn't look too good since we are open to the simplest of phishing attacks. We will be incorporating a fundamental layer of protection in our network followed by a more elaborate DNS Policy, Cloud Delivered Firewall and Secure Web Gateway solution.

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

Let's start off by giving some basic DNS-layer Security to our devices.

1. Connect to the sdwan-ghi-ad-podX machine by logging in to Guacamole (10.2.1.20X:8080/guacamole, where X is your POD number) with the credentials given below and click on the PODX-AD option.

    Alternatively, you can RDP to 10.2.1.18X (where X is your POD number) from the Jumphost. RDP to the AD PC will only work from the Jumphost

    | Connection Method | Username | Password |
    | --- | ---- | ---- |
    | Guacamole | sdwanpod | C1sco12345 |
    | RDP | swatsdwanlab\Administrator | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/01_guac.PNG)

    ![](/images/Umbrella_SDWAN_2/57_xad.PNG)

    vCenter (accessible via the bookmark or 10.2.1.50/ui and the credentials provided for your POD) can also be used to console to the AD PC

2. Depending on the connection method, you may need to enter credentials again to log in to the AD PC. Please enter the credentials shown below, if prompted

    | Connection Method | Username | Password |
    | --- | ---- | ---- |
    | Guacamole | Not Required | Not Required |
    | RDP | swatsdwanlab\Administrator | C1sco12345 |
    | vCenter | swatsdwanlab\Administrator | C1sco12345 |

    If using Guacamole to access the AD PC, you will be notified to press Ctrl + Alt + Del to unlock the computer. Guacamole doesn't have an option to send key combinations. We use the Guacamole virtual keyboard to send Ctrl + Alt + Del. While on the Guacamole window, press **Ctrl + Alt + Shift** together. This will open the Guacamole settings window. Choose **On-screen keyboard** under Input Method and it should display the virtual keyboard. Using the mouse, click on *Ctrl*, then *Alt*, then *Del*

    ![](/images/Umbrella_SDWAN_2/58_ad_onscreen.PNG)

    ![](/images/Umbrella_SDWAN_2/59_ctrlaltdel.PNG)

    This will bring you to the login screen. Press **Ctrl + Alt + Shift** on your keyboard to bring up the Guacamole settings window again and choose **None** for the Input Method

    ![](/images/Umbrella_SDWAN_2/60_none.PNG)

    This will remove the virtual keyboard from the screen and you can continue typing like normal to enter the password.

3. Once logged in to the AD PC, click on **Start** and search for *DNS*. Open the DNS application

    ![](/images/Umbrella_SDWAN_2/61_dns.PNG)

4. Select *ad.swatsdwanlab.com* and double-click Forwarders. There will be two Forwarders listed (*8.8.8.8 and 4.2.2.2*). Click on **Edit**

    ![](/images/Umbrella_SDWAN_2/62_fwdedit.PNG)

5. Change the Forwarder IPs to *208.67.222.222* and *208.67.220.220*. Make sure no other Forwarders are present on this window. Click on **OK**

    ![](/images/Umbrella_SDWAN_2/63_opendns.PNG)

6. Click on **Apply** and then **OK** to apply the configuration change. Click on Start and type **services.msc**. Hit Enter and look for the DNS Server service. Right click on it and restart the service

    ![](/images/Umbrella_SDWAN_2/64_appOK.PNG)

    ![](/images/Umbrella_SDWAN_2/199_services.PNG)

7. Head back to the Site 30 PC and click on the **Flush DNS** shortcut on the Desktop

    ![](/images/Umbrella_SDWAN_2/71_flushdns.PNG)

    ![](/images/Umbrella_SDWAN_2/72_res.PNG)

8. Close any open browsers and re-open the browser. Go to welcome.umbrella.com or use the Umbrella Test bookmark. We should see a **Welcome to Umbrella** page

    ![](/images/Umbrella_SDWAN_2/73_umbrellaOK.PNG)

9. Access to amazon.com and ebay.com should still be intact, since we haven't applied any policies yet

    ![](/images/Umbrella_SDWAN_2/74_amazonebay.PNG)

10. Enter internetbadguys.com in the browser and the traffic will be blocked. We have thus got a fundamental layer of security by simply pointing our DNS Server to the OpenDNS resolvers

    ![](/images/Umbrella_SDWAN_2/75_ibgblocked.PNG)

{% include note.html content="If the site still opens, Flush the DNS cache on the Site 30 PC by clicking the Flush DNS shortcut on the desktop." %}

{% include tip.html content="This is the simplest way to redirect traffic to Umbrella. However, if a user changes the DNS Server IP Address on their PCs, they can bypass the Umbrella redirect completely. It is recommended to deploy policies via vManage such that vEdges/cEdges can intercept DNS traffic destined for a manually entered DNS server (like 8.8.8.8) and redirect it to Umbrella." %}

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

The previous section ensured that DNS queries were redirected to Umbrella, giving us a basic layer of protection. To apply custom DNS policies, we will need to ensure that our setup can be uniquely identified by Umbrella, post which DNS Policies can be set up for the organization. Umbrella can be used to identify traffic coming from a public IP/IP Range. This helps with creating custom policies for a particular organization. In our lab, multiple devices will be talking to the outside world via the same Public IP, hence this approach will not work for us.

Instead, we can get extremely granular and apply a policy to a specific user/group of users based on identities used to uniquely identify them. We can also pinpoint individual workstations by leveraging Cisco AnyConnect, thereby encompassing Roaming Computers in our DNS policies.

### API Keys and AD Configuration

Three pieces of the puzzle that uniquely identify our Enterprise Network on Umbrella are given below:

* Organization (this is a numeric string, allocated by Umbrella. Not to be confused with the SD-WAN organization name)

* API Key

* Secret

1. From your Jumphost, open a browser and go to login.umbrella.com. Login using the username/password for your POD

    | Username | Password |
    | :---: | :---: |
    | ghi.pod0X@gmail.com <br> <br> X is your POD number | C1sco@12345 |

    ![](/images/Umbrella_SDWAN_2/76_login.PNG)

2. Once logged in, the URL will contain your Organization ID. It will vary per POD. Copy it in a notepad file on the Jumphost since we will be needing it later

    ![](/images/Umbrella_SDWAN_2/77_org.PNG)

3. API Keys and the Secret needs to be generated on Umbrella. Navigate to **Admin => API Keys**. If the sidebar isn't visible, click on the menu icon (three horizontal lines) next to the Cisco Logo

    ![](/images/Umbrella_SDWAN_2/78_menu_admin_api.PNG)

4. Click on **Create API Key**

    ![](/images/Umbrella_SDWAN_2/79_create.PNG)

5. Select the radio button next to **Umbrella Management** and click on **Create**

    ![](/images/Umbrella_SDWAN_2/80_mgmt_create.PNG)

6. This will generate the API Key and Secret. Click on the copy icon next to each and paste it in the notepad which contains the Organization ID. Save this notepad file on the Desktop of the Jumphost, giving it any name

    {% include important.html content="Make sure that the Key and Secret are copied to notepad before proceeding since the Secret is visible on this page only." %}

    Put a check mark next to the *To keep it secure...* statement and click on **Close**

    ![](/images/Umbrella_SDWAN_2/81_key_sec.PNG)

    {% include tip.html content="If the key needs to be re-generated (usually required if the secret is misplaced), the Refresh button will allow you to generate a new API Key and Secret." %}

7. Log in to the AD PC (10.2.1.18X) via your preferred method (Guacamole/RDP/vCenter Console) and click on Start. Search for Active Directory Users and Computers and open the App

    ![](/images/Umbrella_SDWAN_2/82_aduac.PNG)

8. Make sure swatsdwanlab.com is expanded and right click on **Users**. Click on **New** and click on **User** to create a new user

    ![](/images/Umbrella_SDWAN_2/83_newuser.PNG)

9. Populate the fields as shown in the table below and click on **Next**

    | Field | Value |
    | :---: | :---: |
    | First Name | OpenDNS_Connector |
    | User logon name | OpenDNS_Connector |

    {% include note.html content="The User logon name field had to match with what is given here in previous versions of vManage. The name can now be populated as a custom value, if required, but we will use the default logon name." %}

    ![](/images/Umbrella_SDWAN_2/84_odns.PNG)

10. Enter a password of *C1sco12345* in the Password and Confirm Password fields. Uncheck *User must change password at next logon* and check *Password never expires*. If you check Password never expires directly, it will automatically uncheck User must change password at next logon but will give a notification prompt (choose OK). Click on **Next** and then **Finish**

    ![](/images/Umbrella_SDWAN_2/85_pne.PNG)

    ![](/images/Umbrella_SDWAN_2/86_fin.PNG)

11. The user we just created needs to be a part of certain Groups in order to function properly. Right click on the newly created *OpenDNS_Connector* user and click on **Add to a group**

    ![](/images/Umbrella_SDWAN_2/87_atg.PNG)

12. Add the user to the following groups and click on **OK**:

    * Event Log Readers
    * Distributed COM Users
    * Enterprise Read-only Domain Controllers

    {% include note.html content="Enter the first few characters of the Group you want to add this User to and click on *Check Names*. That should auto-populate the Group or give you a selection to choose the group." %}

    ![](/images/Umbrella_SDWAN_2/88_groups.PNG)

13. Click on **OK** to confirm the addition of the user to the Groups

    ![](/images/Umbrella_SDWAN_2/89_OK.PNG)


We have generated the API Key and Secret which will be needed later in the integration with Cisco Umbrella. We have also set up an AD User which will be required for AD Connector functionality.

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

To uniquely identify our SD-WAN network, we will be connecting AD to Umbrella and syncing AD Groups and Users. This is done by downloading and running a configuration script on the Domain Controller (all read-write DCs) and by deploying an AD Connector. A user is required for the AD Connector to work - this was created in the previous section.

1. From your **AD PC**, open a browser and go to login.umbrella.com. Login using the username/password for your POD. Go to **Deployments => Configuration => Sites and Active Directory**

    | Username | Password |
    | :---: | :---: |
    | ghi.pod0X@gmail.com <br> <br> X is your POD number | C1sco@12345 |

    ![](/images/Umbrella_SDWAN_2/90_dep_conf_sad.PNG)

2. Click on the **Download** button in the top right-hand corner and download the **Windows Configuration script for Domain Controller**. Choose to **Keep** the file, if prompted (browser specific)

    ![](/images/Umbrella_SDWAN_2/91_dwnld_dwnldconf.PNG)

    ![](/images/Umbrella_SDWAN_2/92_keep.PNG)

3. Click on Start and search for **cmd**. Click on the Command Prompt App

    ![](/images/Umbrella_SDWAN_2/93_cmd.PNG)

4. Type `cd Downloads` to access the Downloads folder and hit Enter. Enter the `cscript` command, followed by the Configuration File you just downloaded. The file name will be different from what is shown below - enter the name of the configuration file downloaded by you (type `cscript OpenDNS` and hit Tab on the keyboard - the name will auto complete) and hit **Enter**

    ![](/images/Umbrella_SDWAN_2/94_rs.PNG)

5. Enter *2* when asked to Enter the IP to be used. We will be using the 10.30.10.50 IP. This is the IP that will show up on Umbrella. Proceed through the script by Entering *y* for any other prompts that show up

    ![](/images/Umbrella_SDWAN_2/95_ip.PNG)

    ![](/images/Umbrella_SDWAN_2/96_yes.PNG)

    ![](/images/Umbrella_SDWAN_2/97_yes.PNG)

6. The configuration script should complete successfully

    ![](/images/Umbrella_SDWAN_2/98_succ.PNG)

7. Head over to the Umbrella page and refresh the Sites and Active Directory page. The DC just added should show up. The status sometimes takes an hour to get updated

    ![](/images/Umbrella_SDWAN_2/99_refpage.PNG)

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

AD Connectors allow Umbrella to see your AD structure and reference AD Groups/Users in Policies.

1. From the AD PC, make sure you are logged in to Umbrella and navigate to **Deployment => Configuration => Sites and Active Directory**. Click on the **Download** button in the top right-hand corner and download the **Windows Service (Active Directory Connector)**

    ![](/images/Umbrella_SDWAN_2/100_dwnldadc.PNG)

2. This will download a .zip file named *OpenDNS-Windows-Service.zip*. Click on the up arrow next to the downloaded file and choose to Open File Location (browser specific - Firefox has a folder icon in the list of downloads which takes you to the location)

    ![](/images/Umbrella_SDWAN_2/101_zip_ocf.PNG)

3. Right click on the file and choose **Extract All**

    ![](/images/Umbrella_SDWAN_2/102_ext.PNG)

4. The file will be extracted to the path shown in the image by default. Click on **Extract**

    ![](/images/Umbrella_SDWAN_2/103_extract.PNG)

5. Once extracted, the contents of the .zip will open in a new window. Double click **Setup** to start the AD Connector Installer

    ![](/images/Umbrella_SDWAN_2/104_folder_setupmsi.PNG)

6. Click on **Next** at the Welcome and Destination folder screens. Enter a password of *C1sco12345*, leaving the Username at the default of *OpenDNS_Connector*. These should match with the user we created in Active Directory. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/105_next.PNG)

    ![](/images/Umbrella_SDWAN_2/106_next.PNG)

    ![](/images/Umbrella_SDWAN_2/107_pwdnext.PNG)

7. The credentials should be validated successfully. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/108_next.PNG)

8. Click on **Install** to begin the installation and **Finish** once the installation is complete

    ![](/images/Umbrella_SDWAN_2/109_install.PNG)

    ![](/images/Umbrella_SDWAN_2/110_fin.PNG)

9. On the AD PC, click on Start and search for *services.msc*. Click on the **Services** Desktop app

    ![](/images/Umbrella_SDWAN_2/111_services.PNG)

10. Right click on **Active Directory Domain Services** and choose to *Restart* the service. Select **Yes** to restart other related services as well

    ![](/images/Umbrella_SDWAN_2/112_restart.PNG)

    ![](/images/Umbrella_SDWAN_2/113_yes.PNG)

11. Once the services have restarted, locate the **OpenDNS Connector** service. Right click it and *Restart* this service as well

    ![](/images/Umbrella_SDWAN_2/114_ods.PNG)

12. Head over to Umbrella and navigate to **Deployments => Configuration => Sites and Active Directory**. Refresh the page if you're already on it and the AD Connector will show up over there. Don't worry if you don't see a green check mark (it takes time to reflect correctly)

    ![](/images/Umbrella_SDWAN_2/115_refreshsad.PNG)

13. On the Umbrella GUI, go to **Policies => Management => DNS Policies** and click on **Add** to create a new DNS Policy. We won't be adding the policy right now but will just check if our AD schema is visible on Umbrella

    ![](/images/Umbrella_SDWAN_2/116_check1.PNG)

14. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/117_next.PNG)

15. You should see **AD Groups** and **AD Users** under *All Identities*, with a number next to it (13 and 3 respectively in this screenshot). A number is an indication that Umbrella can now see our AD configuration

    ![](/images/Umbrella_SDWAN_2/118_adg_adu.PNG)

16. Click on **AD Users** (click on the word AD Users, don't click on the checkbox next to it) and you will see 3 Users, imported from AD indicating that AD and Umbrella have been successfully linked. Click on **Cancel**

    ![](/images/Umbrella_SDWAN_2/119_aducancel.PNG)

This completes the configuration needed for linking AD with Umbrella. While we can reference the AD Groups/Users in our DNS Policies, it is possible to become even more granular and link individual workstations to Umbrella, thereby encompassing the remote workers use case. We will configure this in the next section.

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

Cisco AnyConnect is used to identify Roaming Computers and include them within our DNS Policies. This is what will be leveraged in our lab environment to build and apply a DNS Policy.

1. Access the Site 30 PC via your preferred method (Guacamole/RDP/vCenter Console) and log in. [Click here](#pre-work){:target="_blank"} and go through Step 1 to review how to connect to the Site 30 PC. Open the **AnyConnect** folder on the Desktop and double-click **Setup** to start installing AnyConnect

    ![](/images/Umbrella_SDWAN_2/120_anyconnect.PNG)

2. Enter the following credentials when prompted for a username/password and click on **Yes**

    | Username | Password |
    | :---: | :---: |
    | administrator | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/121_unpwd.PNG)

3. Remove the check mark against all modules *except* **Core & VPN**, **Umbrella Roaming Security** and **Diagnostic And Reporting Tool**. Click on **Install Selected** to install the selected modules

    ![](/images/Umbrella_SDWAN_2/122_services.PNG)

4. Click on **OK** and **Accept** the License Agreement

    ![](/images/Umbrella_SDWAN_2/123_OK.PNG)

    ![](/images/Umbrella_SDWAN_2/124_accept.PNG)

5. Once installation is complete, click on **OK**

    ![](/images/Umbrella_SDWAN_2/125_ok.PNG)

6. Open **Cisco AnyConnect Secure Mobility Client** by clicking on Start (it will show up in the *Recently Added* section). Notice that Roaming Security is flagged as unprotected by Umbrella. We will need to copy a profile unique to our Organization so that this workstation shows up on Umbrella as a Roaming Computer

    ![](/images/Umbrella_SDWAN_2/126_any_noumb.PNG)

7. From the **Site 30 PC**, log in to Umbrella. [Click here](#api-keys-and-ad-configuration){:target="_blank"} and reference Step 1 to review the login procedure, but make sure you log in to Umbrella via the Site 30 PC and **not** the AD PC. Go to **Deployments => Core Identities => Roaming Computers** and click on **Roaming Client** in the top right-hand corner

    ![](/images/Umbrella_SDWAN_2/127_rc_rc.PNG)

8. Click on **Download Module Profile**

    ![](/images/Umbrella_SDWAN_2/128_download.PNG)

9. This will download a file called *OrgInfo.json*. Click on the arrow next to the file download and choose **Show in folder** (again, browser specific - Firefox has a folder icon to go to the download location)

    ![](/images/Umbrella_SDWAN_2/129_shinf.PNG)

10. Right click on *OrgInfo.json* and click on **Copy**

    ![](/images/Umbrella_SDWAN_2/130_copy.PNG)

11. Open Windows Explorer and enter the following path (you will not be able to see this folder since it's hidden by default. There is an option to view hidden files and folders in Windows, but we can browse directly to the location)- *C:\ProgramData\Cisco\Cisco AnyConnect Secure Mobility Client\Umbrella*

    ![](/images/Umbrella_SDWAN_2/131_path.PNG)

12. Paste the file we copied before (OrgInfo.json)

    ![](/images/Umbrella_SDWAN_2/132_paste.PNG)

13. Click on **Continue**

    ![](/images/Umbrella_SDWAN_2/133_cont.PNG)

14. Enter the username/password as shown below

    | Username | Password |
    | :---: | :---: |
    | administrator | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/134_creds.PNG)

15. Once the file is placed in the folder, it should auto-generate another folder called **data**. If this doesn't show up, close Cisco AnyConnect and re-open

    ![](/images/Umbrella_SDWAN_2/135_org_dataauto.PNG)

16. AnyConnect should now show that you are protected by Umbrella

    ![](/images/Umbrella_SDWAN_2/136_umb.PNG)

17. Back at the Umbrella GUI, refresh the **Roaming Computers** page. The Site 30 PC will show up as a Roaming Computer

    ![](/images/Umbrella_SDWAN_2/137_refresh.PNG)

We will use the Roaming Computer as an Identity to enforce DNS Policies (the next section).

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

1. Log in to the Cisco Umbrella GUI (you can now log in from your own workstation since Umbrella is on the Cloud). [Click here](#api-keys-and-ad-configuration){:target="_blank"} and reference Step 1 to review the login procedure. Navigate to **Policies => Policy Components => Destination Lists**. You will notice a few default Lists already created

    ![](/images/Umbrella_SDWAN_2/137_udl.PNG)

2. Click on **Add** in the top right-hand corner and give your List a name of *BlockAmazon*. Leave the **This destination list is applied to** field at *DNS Policies*

    ![](/images/Umbrella_SDWAN_2/137_v_add_baz.PNG)

3. Scroll down to the **Destinations in this list should be** field and make sure it is set to **Blocked**. Type amazon.com in the *Enter a domain or URL* box and hit Enter (or click on Add). This should place amazon.com in the list (blocked). Click on **Save**

    ![](/images/Umbrella_SDWAN_2/137_wazcomsave.PNG)

4. Navigate to **Policies => Management => DNS Policies** and click on **Add** to add a new DNS Policy

    ![](/images/Umbrella_SDWAN_2/138_dnspol.PNG)

5. Scroll down on the **How would you like to be protected?** page and click on **Next** without making any changes

    ![](/images/Umbrella_SDWAN_2/139_next.PNG)

6. On the **What would you like to protect?** page, click on **Roaming Computers**. Don't click on the checkbox next to it, but on the actual phrase itself

    ![](/images/Umbrella_SDWAN_2/140_rc.PNG)

7. Put a check mark next to *site30pc* and it should show up in the right-hand window. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/141_next.PNG)

8. Click **Next** in the Security Settings

    ![](/images/Umbrella_SDWAN_2/142_next.PNG)

9. Select **Moderate** on the **Limited Content Access** page and make note of the categories that are being blocked. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/143_mod.PNG)

10. Search for *ebay* in the Search Box on the **Control Applications** page under **Applications to Control** and put a check mark next to eBay. Make sure it is set to **Block** and click on **Next**. Click on **Proceed** on the Application Control Change Summary page

    ![](/images/Umbrella_SDWAN_2/144_blebay.PNG)

    ![](/images/Umbrella_SDWAN_2/145_proc.PNG)

11. Put a check mark next o **BlockAmazon** on the **Apply Destination Lists** page. This will apply the List we created before to the policy being built right now. You should see BlockAmazon on the right hand-side under **2 Block Lists Applied**. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/146_blaznext.PNG)

12. Click on **Next** on the **File Analysis** and **Set Block Page Settings** pages without making any changes

    ![](/images/Umbrella_SDWAN_2/147_next.PNG)

    ![](/images/Umbrella_SDWAN_2/148_next.PNG)

13. Once on the **Policy Summary** page, give your Policy a Name of *DNSPolicy1*. Click on **Save**

    ![](/images/Umbrella_SDWAN_2/149_dnspol1save.PNG)

14. Our DNS Policy is now created. It might take 5 minutes for the policy to be applied. Click on the *DNSPolicy1* policy and enable **SSL Decryption**. Scroll down and click on **Save**

    ![](/images/Umbrella_SDWAN_2/150_finalpol.PNG)

    ![](/images/Umbrella_SDWAN_2/150_veditpolssldec.PNG)

15. We are now going to test our DNS Policy, but before doing so, the Cisco Umbrella root certificate will need to be downloaded and installed on the Site 30 PC. Head over to the Site 30 PC via your preferred connection method (Guacamole/RDP/vCenter Console). [Click here](#pre-work){:target="_blank"} and go through Step 1 to review how to connect to the Site 30 PC. Double-click the **Flush DNS** icon on the Desktop to clear the DNS cache

    ![](/images/Umbrella_SDWAN_2/151_flush.PNG)

16. Log in to Umbrella on the Site 30 PC (login.umbrella.com). [Click here](#api-keys-and-ad-configuration){:target="_blank"} and reference Step 1 to review the login procedure, but make sure you log in to Umbrella via the Site 30 PC. Navigate to **Deployment => Configuration => Root Certificate**

    ![](/images/Umbrella_SDWAN_2/152_deprootcert.PNG)

17. Expand **Cisco Root Certificate Authority** and download the root CA certificate

    ![](/images/Umbrella_SDWAN_2/153_dwnld_install.PNG)

18. Click on Keep, if prompted and open the downloaded file. Choose **Open** in the Security Warning

    ![](/images/Umbrella_SDWAN_2/154_keepopen.PNG)

19. Click on **Install Certificate**

    ![](/images/Umbrella_SDWAN_2/155_install.PNG)

20. Select **Local Machine** and click on **Next**. Enter the credentials shown below and click on **Yes**

    | Username | Password |
    | :---: | :---: |
    | administrator | C1sco12345 |

    ![](/images/Umbrella_SDWAN_2/156_locmacnext.PNG)

    ![](/images/Umbrella_SDWAN_2/157_unpwd.PNG)

21. Choose the radio button next to **Place all certificates in the following store** and click on **Browse**. Click on **Trusted Root Certification Authorities** and hit **OK**

    ![](/images/Umbrella_SDWAN_2/158_trusted.PNG)

22. Click on **Finish** and then **OK**. Close the browser you were using and re-open before proceeding to the next step

    ![](/images/Umbrella_SDWAN_2/159_fin_ok_restbrowser.PNG)

23. On the browser, go to yahoo.com. The page should open since we haven't applied any policy for it

    ![](/images/Umbrella_SDWAN_2/160_yahooworks.PNG)

24. Now try going to amazon.com. We will find that it is blocked with the text **The site is blocked** indicating this has been done by the administrator via a Block List. Amazon was opening before, but our company policy doesn't allow it and we have thus leveraged Cisco Umbrella's DNS Policy functionality to block specific destinations

    ![](/images/Umbrella_SDWAN_2/161_amzbl.PNG)

25. Try to browse to ebay.com. This will also be blocked but the text will read **This site is blocked due to content filtering**. This is because we blocked eBay in the Control Applications section of our policy

    ![](/images/Umbrella_SDWAN_2/162_ebaybl.PNG)

26. Try to go to poker.com. This will also be blocked (with the same text as the previous step). Over here, our **Limited Content Access** level of *Moderate* is coming in to play. Note the subtext mentioning *This site was blocked due to the following categories: Gambling*

    ![](/images/Umbrella_SDWAN_2/163_pokerbl.PNG)

This completes the DNS Security part of our configuration. We have successfully deployed a DNS Policy, blocking sites that are not allowed by our company policy.

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

The main focus of SD-WAN and Umbrella integration is around Secure Internet Gateway (SIG) functionality. So far, we have run through a DNS policy which is the first layer of security  in the network. For deeper packet inspection, we can utilize Umbrella and SD-WAN's SIG functionality which will create IPSEC tunnels between our vEdges/cEdges and Cisco Umbrella. Traffic will be sent to Umbrella over the IPSEC tunnels and will be subject to Firewall and Web policies.

1. Open a browser and log in to Cisco Umbrella from your Jumphost. [Click here](#api-keys-and-ad-configuration){:target="_blank"} and reference Step 1 to review the login procedure, but make sure you log in to Umbrella via the **Jumphost** and **not** any other workstation. The main overview page will show that we have 1/1 Active Roaming Client and no Active Network Tunnels

    ![](/images/Umbrella_SDWAN_2/165_notunn.PNG)

2. Log in to the vManage GUI via the bookmark (or go to 192.168.0.6) with the Username and Password given below. Navigate to **Configuration => Templates => Feature Tab** and click **Add Template**. Search for *vedge* and select the **vEdge Cloud** device. Click on **SIG Credentials** under Other Templates

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/Umbrella_SDWAN_2/166_sigcred.PNG)

3. Put the **Template Name** as *SIG-Creds* and a Description of *SIG Credentials*. Enter the Organization ID, Registration Key (i.e. API Key) and Secret copied and saved to notepad before. Click on **Save**

    ![](/images/Umbrella_SDWAN_2/167_detsave.PNG)

4. Back at the Templates page, make sure you're still on the **Feature Tab** and click on **Add Template**. Search for vedge and select **vEdge Cloud**. Click on **Secure Internet Gateway (SIG)** under VPN

    ![](/images/Umbrella_SDWAN_2/168_sigtemp.PNG)

5. Give it a **Template Name** of *SIG-Template* and a Description of *SIG Template*

    ![](/images/Umbrella_SDWAN_2/169_sigtemplname.PNG)

6. Click on **Add Tunnel** and enter the details given in the table below. Click on **Add** once done

    | Parameter               | Global or Device Specific (Drop Down) | Value    |
    |-------------------------|---------------------------------------|----------|
    | Interface Name (1..255) | Global                                | *ipsec1* |
    | Source Interface        | Global                                | *ge0/0*  |
    | Data-Center             | NA                                | Primary  |

    ![](/images/Umbrella_SDWAN_2/170_addtunnadd.PNG)

7. Click on **Add Tunnel** again to add a second IPSEC Tunnel. Enter the details given below and click on **Add**

    | Parameter               | Global or Device Specific (Drop Down) | Value    |
    |-------------------------|---------------------------------------|----------|
    | Interface Name (1..255) | Global                                | *ipsec2* |
    | Source Interface        | Global                                | *ge0/0*  |
    | Data-Center             | NA                                | Secondary  |

    ![](/images/Umbrella_SDWAN_2/171_addsecsave.PNG)

8. Populate *ipsec1* under Active and *ipsec2* under Backup. Click on **Save**

    ![](/images/Umbrella_SDWAN_2/172_hasave.PNG)

9. Log in to vEdge30 via the saved Putty session. Enter `ping global-a.vpn.sig.umbrella.com`. Pings should be successful. Press Ctrl + c to stop the pings

    | Username | Password |
    | :----: | :---: |
    | admin | admin |

    ![](/images/Umbrella_SDWAN_2/173_ping.PNG)
    ```
    ping global-a.vpn.sig.umbrella.com
    ```

10. Back on the vManage GUI, navigate to **Configuration => Templates**. Under the Device tab, locate the *vedge30_dev_temp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/Umbrella_SDWAN_2/174_editdevtemp.PNG)

11. Go to the **Transport & Management VPN** section click on **Secure Internet Gateway** under **Additional VPN 0 Templates**. Select the *SIG-Template* from the drop down

    ![](/images/Umbrella_SDWAN_2/175_sigtemp.PNG)

12. Scroll down to the **Additional Templates** section and populate *SIG-Creds* for the **SIG Credentials**. Click on **Update**

    ![](/images/Umbrella_SDWAN_2/176_sigcredsupdate.PNG)

13. Click on **Next**. You can view the side-by-side configuration if required. Make note of the *secure-internet-gateway* and *ha-pairs* configuration

    ![](/images/Umbrella_SDWAN_2/177_sbs1.PNG)

14. If you scroll down, *interface ipsec1* and *interface ipsec2* configuration can be viewed. Click on  **Configure Devices**

    ![](/images/Umbrella_SDWAN_2/178_sbs2conf.PNG)

15. Wait for a couple of minutes and log in to the Putty session for *vedge30*. Issue the command `show ipsec ike sessions`. You will see 2 sessions which should be in a state of **IKE_UP_IPSEC_UP**. If the sessions are in any other state, wait for a couple more minutes and issue the same command again

    ![](/images/Umbrella_SDWAN_2/179_sh.PNG)

16. Log in to the Umbrella GUI. On the main overview page, you should see **Active Network Tunnels 2/2 Active**

    ![](/images/Umbrella_SDWAN_2/180_2tunn.PNG)

    This is an indication that our IPSEC Tunnels to Umbrella are up.

17. Head over to the Site 30 PC and open a web browser. Click on the *Outgoing Port Tester (444)* bookmark or go to http://portquiz.net:444. The page should load correctly

    ![](/images/Umbrella_SDWAN_2/164_pqwork.PNG)

18. Head back over to the vManage GUI and go to **Configuration => Templates => Feature Tab**. Locate the *vedge30-vpn10* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/Umbrella_SDWAN_2/180_aeditvpn10.PNG)

19. Scroll down to the **Service Route** section and click on **New Service Route**. Enter a global **Prefix** for *0.0.0.0/0* and click on **Add**. Click on **Update** followed by **Next** and **Configure Devices**

    ![](/images/Umbrella_SDWAN_2/180_bservrouteaddupdatenextconfdev.PNG)

    This will ensure that all the traffic hitting VPN 10 on vEdge30 is punted over the newly established IPSEC Tunnels to Umbrella.

20. On the Umbrella GUI, click on **Active Network Tunnels** and you will see the naming convention automatically populated for our 2 Tunnels. Both tunnels should be in an **Active** state (if the status is unknown, wait for some time and revisit this page)

    ![](/images/Umbrella_SDWAN_2/181_nettunnact.PNG)

    {% include tip.html content="The naming convention can be broken down as the Site ID, followed by the word SYS (for System IP) and then the System IP of the device in question with the dots replaced by x. The last few characters reference the Interface (IF) followed by the Interface Name (ipsec1 and ipsec2 in our case)." %}

We have completed IPSEC Tunnel configuration for our vEdge30 device. Through the Service Route, we have ensured that all traffic is punted over the Tunnels to Umbrella (this is not in effect yet, more changes will be made to force traffic over the Tunnels).

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

1. Log in to Cisco Umbrella from your Jumphost, if not already logged in. Navigate to **Policies => Management => Firewall Policy** and click on **Add** in the top right-hand corner

    ![](/images/Umbrella_SDWAN_2/183_addfwpol.PNG)

2. Enter the rule name as *block444*. We will be blocking TCP traffic to port 444 via this Firewall Policy

    ![](/images/Umbrella_SDWAN_2/184_name.PNG)

3. Scroll down and set the Protocol to TCP. Set the **Destination Ports** to **Specify Port** and enter the port number 444

    ![](/images/Umbrella_SDWAN_2/185_rule.PNG)

4. Set the **Rule Action** to *Block Traffic* and Enable Logging

    ![](/images/Umbrella_SDWAN_2/186_blocktraf_log.PNG)

5. Under **Rule Schedule** set the **Start Date** to the earliest available and make sure **Does Not Expire** is checked. Click on **Save**

    ![](/images/Umbrella_SDWAN_2/186_onedayb4.PNG)

6. The Firewall Policy of *block444* should show up above the **Default Rule**

    ![](/images/Umbrella_SDWAN_2/187_finalfw.PNG)

7. On the Site 30 PC, open a browser and go to whatismyip.com. The Public IPv4 address should show up as **14.140.162.5**. We will remove DIA configuration at Site 30 and check the Public IP again

    ![](/images/Umbrella_SDWAN_2/187_fwhatismyip.PNG)

8. On the vManage GUI, navigate to **Configuration => Policies** and click on the three dots next to the *Site40-Guest-DIA* policy. Click on **Edit**. Under the **Policy Application** page, click on the **Traffic Data** tab. Delete the Site30 Site List/VPN List and click on **Save Policy Changes**. Choose to **Activate** the configuration, if prompted

    ![](/images/Umbrella_SDWAN_2/187_gpolrems30.PNG)

9. Once the policy changes have been pushed successfully, go back to the Site 30 PC and use a browser to go to whatismyip.com again. The Public IPv4 address should now be in the 146.112.A.B address space - this is the Singapore Umbrella Server

    ![](/images/Umbrella_SDWAN_2/188_disabledia.PNG)

10. Use the bookmark to navigate to **Outgoing Port Tester (444)** or go to http://portquiz.net:444. The site will not load

    ![](/images/Umbrella_SDWAN_2/187_hpquizfail.PNG)

11. Try to access http://portquiz.net:450 and the site should load right up, indicating that TCP connections to port 444 are being blocked (in line with our Firewall Policy)

    ![](/images/Umbrella_SDWAN_2/187_iport450succ.PNG)

12. Other than using the Cloud Delivered Firewall to block specific ports, we can also block ICMP packets. Open a command prompt on the Site 30 PC and type `ping cisco.com`. Hit Enter. The pings should be successful

    ![](/images/Umbrella_SDWAN_2/188_icmpworking.PNG)

13. Go to the Umbrella GUI and navigate to **Policies => Management => Firewall Policy**. Click on **Add** to add a new policy and name it *blockicmp*

    ![](/images/Umbrella_SDWAN_2/189_blockicmp1.PNG)

14. Set the **Protocol** as ICMP

    ![](/images/Umbrella_SDWAN_2/189_blockicmp2.PNG)

15. Make sure the Start Date is the earliest available and the Rule Action is set to block traffic, with logging enabled. Click on **Save** to save the firewall policy

    ![](/images/Umbrella_SDWAN_2/189_blockicmp3.PNG)

    ![](/images/Umbrella_SDWAN_2/189_blockicmpfinsave.PNG)

16. Wait for approximately 5 minutes and try to ping cisco.com from the Site 30 PC again. Pings should now be blocked

    ![](/images/Umbrella_SDWAN_2/191_icmpblock.PNG)

We have thus used a Firewall Policy to block traffic to a particular destination port and block a certain protocol. This completes our configuration of a Cloud Delivered Firewall.

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

We will now apply a Web Policy to all traffic traversing the IPSEC Tunnels.

1. On the Umbrella GUI, navigate to **Policies => Policy Components => Destination Lists** and click on **Add**. Name the list *blockyahoo* and make sure that the **Blocked** radio button is selected. Enter *yahoo.com* in the **Enter a domain, URL, IPv4 or CIDR** box and click on **Add**. Once yahoo.com shows up in the lower half of the screen, click on **Save**

    ![](/images/Umbrella_SDWAN_2/192_wp3.PNG)

2. Go to **Policies => Management => Web Policies** and click on **Add**. Click **Next** on the **How would you like to be protected?** window and put a check mark next to **Tunnels** in the **What would you like to protect?** window. Click on **Next**

    ![](/images/Umbrella_SDWAN_2/192_wp1.PNG)

3. Click the Radio Button next to **Decrypt Blocked Traffic Only** on the **HTTP Inspection** window and click on **Next**

    ![](/images/Umbrella_SDWAN_2/192_wp2.PNG)

4. Click **Next** for **Security Settings**, **Limit Content Access**, **Tenant Controls** and **Control Applications**. Put a check mark next to the **blockyahoo** Destination List and click on **Next**

    ![](/images/Umbrella_SDWAN_2/192_wp4.PNG)

5. Click **Next** on **File Analysis**, **File Type Control** and **Set Block Page Settings**. Give the Policy a name of *Webblockyahoo* and click on **Save**. The policy should show up above the *Default Web Policy*

    ![](/images/Umbrella_SDWAN_2/192_wp5.PNG)

6. Wait for a few minutes and head over to the Site 30 PC. Click on the **Flush DNS** icon on the Desktop and open a new browser window. Try to access yahoo.com (you can use the bookmark). Traffic to Yahoo, which was working before, should now be blocked. Make note of the subtext *This site was blocked by the Cisco Umbrella proxy*

    ![](/images/Umbrella_SDWAN_2/194_yahoobl.PNG)

We have completed integration and configuration of Umbrella with our SD-WAN environment.


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
