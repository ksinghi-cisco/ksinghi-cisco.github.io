---
title: Inter VPN Routing and Service Chaining
permalink: mydoc_servicechaining.html
tags: [publishing, single_sourcing, content_types]
keywords: inter VPN, Service Chaining, Firewall
last_updated: July 21, 2020
summary: "Implementing Inter VPN Routing between Site 20 VPN 10 and Site 30 VPN 20, along with Service Chaining (Firewall)."
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
As of now, devices in different VPNs cannot communicate with each other. VPN 10 devices can talk to other VPN 10 devices but not to VPN 20. In this section, we will be setting up Inter VPN routing.

Additionally, there might be a requirement where we need to send traffic from one VPN to another through a firewall. This feature is known as Service Chaining (other devices like Load Balancers can also be part of the Service Chain) and is used widely in real-world SD-WAN Deployments.

We will be focussing on ensuring devices in Site 20 VPN 10 can communicate with devices in Site 30 VPN 20. Initially, this will be direct communication between the two VPNs. A firewall will then be inserted in the path so that all traffic between the VPNs traverses the firewall, which will be located at Site-DC in VPN 40.

Diagrammatically, our topology will look as below:

![](/images/InterVPN_ServiceChaining/125_Topo2.PNG)

> The Black arrow between Site 20 and Site 30 indicates the traffic flow when Inter VPN Routing configuration is done for the first time. Traffic flows directly between the two Sites.

> The Orange arrow is the traffic flow from Site 20 VPN 10 to Site 30 VPN 20 once Service Chaining is configured. <br> <br> Source IP: 10.20.10.2 or 10.20.10.3 <br>  Destination IP: 10.30.20.2

>The Green arrow is the traffic flow from Site 30 VPN 20 to Site 20 VPN 10 once Service Chaining is configured. <br> <br>  Source IP: 10.30.20.2 <br>  Destination IP: 10.20.10.2 or 10.20.10.3

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

We will configure VPN 40 at the DC Site and ensure connectivity between the DC-vEdges and the ASAv Firewall.

1. Log in to vCenter using the bookmark or by going to 10.2.1.50/ui from a web browser. Use the credentials for your POD

    | Username | Password |
    | :---: | :---: |
    | sdwanpodX <br> <br> (X is your POD number) | C1sco12345 |

    ![](/images/InterVPN_ServiceChaining/01_login.PNG)

2. Right click on the **DC-vEdge1-podX** VM (where X is your POD number) and go to **Edit Settings**

    ![](/images/InterVPN_ServiceChaining/02_edit.PNG)

3. Click on **Add New Device** and choose to add a new **Network Adapter**. Repeat this process to add another Network Adapter

    ![](/images/InterVPN_ServiceChaining/03_andna.PNG)

4. You should have two new network adapters. Click on the drop down next to the assigned network (Internet in the image below) for the first network adapter and click **Browse**

    ![](/images/InterVPN_ServiceChaining/04_browse.PNG)

5. Choose *SiteDC_VPN10* and click on **OK**

    ![](/images/InterVPN_ServiceChaining/124_maynetwint.PNG)

6. This takes you back to the **Edit Settings** page. Click on the drop down next to the assigned network for the second network adapter and click **Browse**. Select *SiteDC-VPN40* and click on **OK**

    ![](/images/InterVPN_ServiceChaining/05_sdc40.PNG)

7. Make sure the settings match with the image given below and click on **OK**

    ![](/images/InterVPN_ServiceChaining/06_sdcvpn40_2.PNG)

8. Log in to **DC-vEdge1** via Putty. You can use the saved session or SSH to *192.168.0.10* along with the credentials given below

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/InterVPN_ServiceChaining/07_putty.PNG)

9. Type `reboot` and then `yes` to confirm the reboot

    ![](/images/InterVPN_ServiceChaining/09_reboot.PNG)
    ```
    reboot
    yes
    ```

10. While the DC-vEdge1 vEdge is rebooting, head over to vCenter and right click on the **DC-vEdge2-podX** VM. Click on **Edit Settings**

    ![](/images/InterVPN_ServiceChaining/10_edit.PNG)

11. Like before, add two network adapters by clicking on **Add New Device** and selecting **Network Adapter**. Make sure you add two network adapters. Click on the drop down for the first Network Adapter and choose **Browse**

    ![](/images/InterVPN_ServiceChaining/11_browse.PNG)

12. Select *SiteDC_VPN10* and click on **OK**

    ![](/images/InterVPN_ServiceChaining/124_maynetwint.PNG)

13. Click on the drop down next to the second network adapter and click on browse. Select *SiteDC-VPN40_2* and click on **OK**. The network adapters should look like the image below

    ![](/images/InterVPN_ServiceChaining/13_bothok_2.PNG)

14. Log in to *DC-vEdge2* via Putty, using the credentials below

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/InterVPN_ServiceChaining/14_putty2.PNG)

15. Type `show interface ?` and notice that there are 4 "ge" interfaces

    ![](/images/InterVPN_ServiceChaining/15_b4reboot.PNG)
    ```
    show interface ?
    ```

16. Type `reboot` and then `yes` to confirm the reboot

    ![](/images/InterVPN_ServiceChaining/16_reboot.PNG)
    ```
    reboot
    yes
    ```

17. Once *DC-vEdge1* and *DC-vEdge2* are back up, log in to either device and issue `show interface ?` again. You will notice two additional interfaces - ge0/4 and ge0/5

    ![](/images/InterVPN_ServiceChaining/17_afterreboot.PNG)

18. Log in to the vManage GUI using the bookmark or by going to *192.168.0.6* on a web browser. Click on **Configuration => Templates**

    ![](/images/InterVPN_ServiceChaining/18_temp.PNG)

19. Go to the **Feature** tab and click on **Add Template**. Search for *vedge* and put a check mark next to **vEdge Cloud**. Choose **VPN** to create a VPN Template

    ![](/images/InterVPN_ServiceChaining/19_vpntemp.PNG)

20. Give a **Template Name** of *dc-vedge-vpn40* and a Description of *vEdge VPN 40 Template for Service Chaining*. Put the VPN as *40*

    ![](/images/InterVPN_ServiceChaining/20_tempdet1.PNG)

21. Scroll down to the **Advertise OMP** section and set **Static (IPv4)** and **Connected (IPv4)** to **On**

    ![](/images/InterVPN_ServiceChaining/21_tempdet2.PNG)

22. Go to the **Service** section and click on **New Service**. Select the **Service Type** as *netsvc1* and enter an **IPv4 Address** of *10.100.40.1*. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/22_svc1.PNG)

23. Click on **New Service** again and select the **Service Type** as *netsvc2*. Enter an **IPv4 Address** of *10.100.40.5*. Click on **Add** then click on **Save** to save the VPN Template configuration

    ![](/images/InterVPN_ServiceChaining/23_svc2_save.PNG)

24. At the **Configuration => Templates => Feature Tab** page, click on **Add Template**. Search for *vedge* and select **vEdge Cloud**. Choose **VPN Interface Ethernet** as the Template Type

    ![](/images/InterVPN_ServiceChaining/24_vpnint1.PNG)

25. Give a **Template Name** of *dc-vedge-vpn40-int1* with a Description of *DC vEdge VPN 40 interface*. Set **Shutdown** to *No* and the **Interface Name** as a Global value of *ge0/4*. Set the **IPv4 Address** to a Device Specific value of *vpn40_if_ipv4_address* and click on **Save**

    ![](/images/InterVPN_ServiceChaining/25_vpnint1det.PNG)

26. Go to **Configuration => Templates** on the vManage GUI and make sure you're on the **Device** tab. Locate the *DCvEdge_dev_temp* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/InterVPN_ServiceChaining/26_editdevtemp.PNG)

27. Scroll down to the **Service VPN** section and click on **Add VPN**. Move the *dc-vedge-vpn40* template to the right-hand side and click on **Next**

    ![](/images/InterVPN_ServiceChaining/27_addserv.PNG)

28. Click on **VPN Interface** under **Additional VPN Templates** and select *dc-vedge-vpn40-int1* under the VPN Interface drop down. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/28_addintvpn40.PNG)

29. Make sure the Service VPN section shows the addition of the VPN 40 Template and click on **Update**

    ![](/images/InterVPN_ServiceChaining/29_upd.PNG)

30. Enter the **IPv4 Address** field for *vpn40_if_ipv4_address* as *10.100.40.2/30* (for DC-vEdge1) and *10.100.40.6/30* (for DC-vEdge2). Click on **Next**

    ![](/images/InterVPN_ServiceChaining/30_enterdetnext.PNG)

31. Click on **Configure Devices**. You can choose to view the side by side configuration, if required, noting the addition of vpn 40 with the corresponding service addresses

    ![](/images/InterVPN_ServiceChaining/31_sbs.PNG)

32. Confirm the configuration change by clicking on the check box and clicking on **OK**

    ![](/images/InterVPN_ServiceChaining/32_confir.PNG)

33. Once the configuration update goes through, log in to the CLI of **DC-vEdge1** and **DC-vEdge2** via Putty and issue the following commands. You should see successful ping responses:

    On DC-vEdge1 - `ping vpn 40 10.100.40.1`
    On DC-vEdge2 - `ping vpn 40 10.100.40.5`

    ![](/images/InterVPN_ServiceChaining/33_ping.PNG)

This completes the configuration needed for adding VPN 40 to the DC-vEdges.

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

1. On the vManage GUI, go to **Configuration => Templates => Feature Tab**. Locate the *vedge-vpn20-DC* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/InterVPN_ServiceChaining/34_editdevtemp.PNG)

2. Scroll down to the IPv4 Route section and delete the route populated (it should be a null route) by clicking on the **trash icon**. Click on **Update**. Click **Next** and **Configure Devices** to push the update out

    ![](/images/InterVPN_ServiceChaining/35_delnull.PNG)

3. To check the current routing tables for VPN 10 and VPN 20, navigate to **Monitor => Network**

    ![](/images/InterVPN_ServiceChaining/36_mon.PNG)

4. Click on **vEdge20**

    ![](/images/InterVPN_ServiceChaining/37_ve20.PNG)

5. Go to **Real Time** in the left menu and enter *ip route* in the **Device Options** field. Click on *IP Routes* to see the current routes and choose **Show Filters**

    ![](/images/InterVPN_ServiceChaining/38_ipr.PNG)

    ![](/images/InterVPN_ServiceChaining/39_sf.PNG)

6. Enter a **VPN ID** of *10* and click on **Search** to filter the routes for VPN 10 on vEdge20

    ![](/images/InterVPN_ServiceChaining/40_vpn10.PNG)

7. Since Inter VPN Routing hasn't been configured yet, we will see routes that are part of VPN 10 only. Subnets from other VPNs will not show up over here. We can thus infer that there won't be inter VPN connectivity as of now

    ![](/images/InterVPN_ServiceChaining/41_ninterroute.PNG)

8. Click on **Select Devices** (top left-hand corner) and choose **vEdge30** from the drop down. Click on **Show Filters**

    ![](/images/InterVPN_ServiceChaining/42_ve30.PNG)

    ![](/images/InterVPN_ServiceChaining/43_sf.PNG)

9. Enter *20* in the **VPN ID** and click on **Search**

    ![](/images/InterVPN_ServiceChaining/44_vpn20.PNG)

10. This shows all the routes learnt by vEdge30 in VPN 20. There aren't any routes subnets in other VPNs, as of now

    ![](/images/InterVPN_ServiceChaining/45_ninterroute.PNG)

11. On the left hand slide, click on Troubleshooting and select Traceroute (note that this is being done on vEdge30)

    ![](/images/InterVPN_ServiceChaining/46_tr.PNG)

12. Enter a **Destination IP** of *10.20.10.2* and select *VPN 20* from the **VPN** drop down. Populate the **Source/Interface** as *ge0/3* and click on **Start**

    ![](/images/InterVPN_ServiceChaining/47_tracetovpn10.PNG)

13. As expected, the traceroute should fail

    ![](/images/InterVPN_ServiceChaining/48_fail.PNG)

14. Click on **Select Device** in the top left-hand corner and choose *vEdge20*. Run the traceroute again, changing the **Destination IP** to *10.30.20.2*, **VPN** to *VPN 10* and the **Source/Interface** to *ge0/2*. Click on **Start** and this should fail as well

    ![](/images/InterVPN_ServiceChaining/49_trve20.PNG)

    ![](/images/InterVPN_ServiceChaining/50_fail.PNG)

We have established that Inter VPN communication is not happening between Site 20 and Site 30 as of now.

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

In order to facilitate inter VPN connectivity, we will be setting up VPN Lists that can be used in our Policies.

1. On the vManage GUI, go to **Configuration => Policies**

    ![](/images/InterVPN_ServiceChaining/51_confpol.PNG)

2. Click on **Custom Options** in the top right-hand corner and click on **Lists**

    ![](/images/InterVPN_ServiceChaining/52_list.PNG)

3. Select **VPN** and click on **New VPN List**. Enter a **VPN List Name** of *FW* and put *40* for the **Add VPN** field. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/53_fwvpn.PNG)

4. Click on **New VPN List** again and Put a **VPN List Name** of *Corp_FW*. Put *10,40* in the **Add VPN** field. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/54_cfw.PNG)

5. Click on **New VPN List** again and Put a **VPN List Name** of *PoS_FW*. Put *20,40* in the **Add VPN** field. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/55_pfw.PNG)

6. Make sure that the following VPN lists show up, before proceeding

    ![](/images/InterVPN_ServiceChaining/56_finvpn.PNG)

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

1. Navigate to **Configuration => Policies** and locate the *Site40-Guest-DIA* Policy. Click on the three dots next to it and choose to **Edit** the policy

    ![](/images/InterVPN_ServiceChaining/57_edit.PNG)

2. Click on the **Topology** tab (top of the screen) and click on **Add Topology**. Choose to add a *Custom Control (Route & TLOC)* policy

    ![](/images/InterVPN_ServiceChaining/58_toptab_add.PNG)

3. Give the policy a **Name** of *vpn10-inter-vpn20-40* with a Description of *Control Policy for Inter VPN Routing from VPN 10 to VPNs 20 and 40*. Click on **Sequence Type** and choose **Route**

    ![](/images/InterVPN_ServiceChaining/59_name_route.PNG)

4. Click on **Sequence Rule** and add a **VPN** match. Select *Corporate* from the **VPN List** drop down

    ![](/images/InterVPN_ServiceChaining/60_vpn10.PNG)

5. Click on the **Actions** tab and select the **Accept** radio button. Click on **Export To** and select *PoS_FW* from the drop down under Actions. Click on **Save Match And Actions**

    ![](/images/InterVPN_ServiceChaining/61-action.PNG)

6. Select **Default Action** on the left-hand side and click on the **pencil** icon to edit the Default Action

    ![](/images/InterVPN_ServiceChaining/62_pencil.PNG)

7. Click on **Accept** and then **Save Match And Actions**

    ![](/images/InterVPN_ServiceChaining/63_acceptall.PNG)

8. Click **Save Control Policy**

    ![](/images/InterVPN_ServiceChaining/64_savecp.PNG)

9. Click on **Add Topology** and add another *Custom Control (Route & TLOC)* policy. Give it a **Name** of *vpn20-inter-vpn10-40* with a Description of *Control Policy for Inter VPN routing between VPN 20 and VPNs 10 and 40*. Click on **Sequence Type** and select **Route**

    ![](/images/InterVPN_ServiceChaining/65_route.PNG)

10. Click on **Sequence Rule** and select VPN as the match. Select *PoS* from the **VPN List**

    ![](/images/InterVPN_ServiceChaining/66_sr.PNG)

11. Click on the **Actions** tab and select the **Accept** radio button. Click on **Export To** and select the *Corp_FW* VPN list in the **Export To** drop down under Actions. To save the rule, click on **Save Match And Actions**

    ![](/images/InterVPN_ServiceChaining/67_save.PNG)

12. Click on **Default Action** on the left-hand side and click the **Pencil** icon to edit the Default Action

    ![](/images/InterVPN_ServiceChaining/67_y.PNG)

13. Select **Accept** and click **Save Match And Actions**

    ![](/images/InterVPN_ServiceChaining/67_z.PNG)

14. Click on **Save Control Policy**

    ![](/images/InterVPN_ServiceChaining/68_savecp.PNG)

15. You should be back at the main policy screen. Click on the **Policy Application** tab and make sure you're under the **Topology** sub-tab (should not be under the main Topology tab). Click on **New Site List** under the entry for *vpn10-inter-vpn20-40* and select the **Inbound Site List** as *Site20*. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/71_polapp_site20add.PNG)

16. Click on **New Site List** under the entry for *vpn20-inter-vpn10-40* and select the **Inbound Site List** as *Site30*. Click on **Add**. Click on **Save Policy Changes**

    ![](/images/InterVPN_ServiceChaining/72_site30_savepol.PNG)

17. Click on **Activate** to push the changes to the vSmarts

    ![](/images/InterVPN_ServiceChaining/73_act.PNG)

We have set up the policies for Inter VPN Routing.

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

1. On the vManage GUI, navigate to **Monitor => Network** and click on **vEdge20**. Scroll down along the left-hand side menu and click on **Real Time**. Enter *IP Routes* in the **Device Options** and select IP Routes when it pops up. Choose **Show Filters** and enter a **VPN ID** of *10*. Click on **Search**. The Routing Table for VPN 10 on vEdge20 should show routes to subnets at Site 30 VPN 20

    ![](/images/InterVPN_ServiceChaining/74_vpn20leak.PNG)

2. Click on **Select Device** in the top left-hand corner and click on **vEdge30**

    ![](/images/InterVPN_ServiceChaining/75_ve30.PNG)

3. Click **Show Filters** and enter a **VPN ID** of *20*. Click on **Search**

    ![](/images/InterVPN_ServiceChaining/76_sf.PNG)

    ![](/images/InterVPN_ServiceChaining/77_vpn20search.PNG)

4. You should see routes for Site 20 VPN 10

    ![](/images/InterVPN_ServiceChaining/78_vpn10leak.PNG)

5. Click on **Troubleshooting** on the left-hand side and make sure you have **vEdge20** as the selected device. Enter a **Destination IP** of *10.30.20.2* with a **VPN** of *VPN - 10*. Select a **Source/Interface** of *ge0/2* (once again, verify that you're at the vEdge20 device. If not, click on the Select Device drop down from the top left-hand corner and select vEdge20). Click on **Start**. Notice that we now have direct Inter VPN connectivity from Site 20 VPN 10 to Site 30 VPN 20

    ![](/images/InterVPN_ServiceChaining/79_leak1.PNG)

6. Click on **Select Device** in the top left-hand corner and select **vEdge30**. Enter a **Destination IP** of *10.20.10.2* with a **VPN** of *VPN - 20* and a **Source/Interface** of *ge0/3*. Click on **Start**. Notice that we now have direct Inter VPN Connectivity from Site 30 VPN 20 to Site 20 VPN 10

    ![](/images/InterVPN_ServiceChaining/80_leak2.PNG)

This completes the verification of our Inter VPN Routing configuration.

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

Direct connectivity between two VPNs might not be a desirable scenario. There might be a requirement to enforce certain rules when two VPNs are communicating with each other. That's where Service Chaining comes into the picture, where we route Inter VPN traffic through an intermediary device (like a Firewall) to enforce our policies/rules. To reiterate, the traffic flow should look like the diagram below at the end of this section vs. the direct connectivity that we have between VPNs right now.

![](/images/InterVPN_ServiceChaining/125_Topo2.PNG)

> The Black arrow between Site 20 and Site 30 indicates the traffic flow when Inter VPN Routing configuration is done for the first time. Traffic flows directly between the two Sites.

> The Orange arrow is the traffic flow from Site 20 VPN 10 to Site 30 VPN 20 once Service Chaining is configured. <br> <br> Source IP: 10.20.10.2 or 10.20.10.3 <br>  Destination IP: 10.30.20.2

>The Green arrow is the traffic flow from Site 30 VPN 20 to Site 20 VPN 10 once Service Chaining is configured. <br> <br>  Source IP: 10.30.20.2 <br>  Destination IP: 10.20.10.2 or 10.20.10.3

1. On the vManage GUI, go to **Configuration => Policies**. Locate the *Site40-Guest-DIA* policy and click on the three dots next to it. Choose to **Edit** the policy. Make sure you're on the **Topology** tab and click on **Add Topology**. Choose to add a *Custom Control (Route and TLOC)* topology

    ![](/images/InterVPN_ServiceChaining/81_addsc.PNG)

2. Give the Custom Control Policy a **Name** of *site20-fw-site30* and a Description of *Traffic from Site 20 to Site 30 via the Firewall*. Click on **Sequence Type** and choose **Route**

    ![](/images/InterVPN_ServiceChaining/82_namestroute.PNG)

3. Click on **Sequence Rule** and select **Site** for a Match Condition. Click on the **Site List** drop down and choose *Site 30*. Click on the **Actions** tab

    ![](/images/InterVPN_ServiceChaining/83_srsite30.PNG)

4. Select the **Accept** radio button and choose **Service**. Under Actions select the **Service: Type** as *Net Service 1* and specify a **Service: VPN** of *40*. Select an **Encapsulation** of *IPSEC* and click on **Save Match And Actions** to save this rule

    ![](/images/InterVPN_ServiceChaining/84_action.PNG)

5. Click on **Default Action** on the left-hand side and click the pencil icon. Select Accept and then **Save Match And Actions**. The Default Action should change to **Accept** *Enabled*. Click on **Save Control Policy**

    ![](/images/InterVPN_ServiceChaining/85_defactionen.PNG)

6. Make sure you're on the **Topology** tab and click on **Add Topology**. Choose to add a *Custom Control (Route and TLOC)* topology. Give the Custom Control Policy a **Name** of *site30-fw-site20* and a Description of *Site 30 to Site 20 via the firewall*. Click on **Sequence Type** and choose **Route**

    ![](/images/InterVPN_ServiceChaining/86_s30to20route.PNG)

7. Click on **Sequence Rule** and then select **Site**. Choose *Site 20* in the **Site List** under **Match Conditions**. Click on **Actions**

    ![](/images/InterVPN_ServiceChaining/87_s20match.PNG)

8. Select the **Accept** radio button and choose **Service**. Under Actions select the **Service: Type** as *Net Service 2* and specify a **Service: VPN** of *40*. Select an **Encapsulation** of *IPSEC* and click on **Save Match And Actions** to save this rule

    ![](/images/InterVPN_ServiceChaining/88_sr.PNG)

9. Click on **Default Action** on the left-hand side and click the pencil icon. Select Accept and then **Save Match And Actions**. The Default Action should change to **Accept** *Enabled*. Click on **Save Control Policy**

    ![](/images/InterVPN_ServiceChaining/89_defactionsave.PNG)

10. Go to the **Policy Application** tab and locate the *site30-fw-site20* and *site20-fw-site30* entries. For *site30-fw-site20*, click on **New Site List** and choose *Site30* in the out direction. Click on **Add**. Similarly, for *site20-fw-site30*, click on **New Site List** and choose *Site20* in the out direction. Click on **Add**. Click on **Save Policy Changes**. **Activate** the change when prompted to do so

    ![](/images/InterVPN_ServiceChaining/90_polapp_sitelist_save_act.PNG)

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

1. Log in to the CLI of **vEdge20** via Putty (username and password given below) and enter `ping vpn 10 10.100.40.2` to test connectivity between Site 20 VPN 10 and Site DC VPN 40. The pings should fail

    | Username | Password |
    | :---: | :---: |
    | admin | admin |

    ![](/images/InterVPN_ServiceChaining/90_q.PNG)

    This is due to the fact that we haven't set up inter VPN connectivity between VPN 10/VPN 20 and VPN 40. It is vital to ensure that the source and destination VPNs can access the Service Subnet.

2. On the vManage GUI, navigate to **Configuration => Policies**. Click on **Custom Options** on the top right-hand corner and select **Lists**. Click on **VPN** in the left-hand menu and then **New VPN List**. Enter a **VPN List Name** of *Corp_PoS* and put *10,20* in the **Add VPN** field. Click on **Add**

    ![](/images/InterVPN_ServiceChaining/90_r.PNG)

3. Go to **Configuration => Policies** and locate the *Site40-Guest-DIA* Policy. Click on the three dots next to it and choose to **Edit** the policy. Click on the **Topology** tab (top of the screen) and click on **Add Topology**. Choose to add a *Custom Control (Route & TLOC)* policy. Give the policy a **Name** of *vpn40-inter-vpn10-20* with a Description of *Control Policy for Inter VPN Routing from VPN 40 to VPNs 10 and 20*. Click on **Sequence Type** and choose **Route**

    ![](/images/InterVPN_ServiceChaining/199_vpn40inter.PNG)

4. Click on **Sequence Rule** and add a **VPN** match. Select *FW* from the **VPN List** drop down

    ![](/images/InterVPN_ServiceChaining/198_vpninterrule.PNG)

5. Click on the **Actions** tab and select the **Accept** radio button. Click on **Export To** and select *Corp_PoS* from the drop down under Actions. Click on **Save Match And Actions**

    ![](/images/InterVPN_ServiceChaining/196_actionvpn40inter.PNG)

6. Select **Default Action** on the left-hand side and click on the **pencil** icon to edit the Default Action. Click on **Accept** and then **Save Match And Actions**. Click **Save Control Policy**

    ![](/images/InterVPN_ServiceChaining/195_defaction.PNG)

7. You should be back at the main policy screen. Click on the **Policy Application** tab and make sure you're under the **Topology** sub-tab (should not be under the main Topology tab). Click on **New Site List** under the entry for *vpn40-inter-vpn10-20* and select the **Inbound Site List** as *DC*. Click on **Add**. Click on **Save Policy Changes**. Click on **Activate** to push the changes to the vSmarts

    ![](/images/InterVPN_ServiceChaining/194_polapp_sitedc_savepolch.PNG)

8. Head back over to the CLI of vEdge20 and type `ping vpn 10 10.100.40.2`. The pings should now be successful. Type `ping vpn 10 10.100.40.1` to ping the Firewall. This should also work

    ![](/images/InterVPN_ServiceChaining/192_afterinter40.PNG)

9. On the vManage GUI, go to **Monitor => Network** and select **vEdge20**. Click on **Troubleshooting** along the left-hand menu and choose **Traceroute**. Enter a **Destination IP** of *10.30.20.2* and a **VPN** of *VPN - 10*. Set the **Source/Interface** as *ge0/2* and click on **Start**. We are thus doing a traceroute from Site 20 VPN 10 to Site 30 VPN 20

    ![](/images/InterVPN_ServiceChaining/191_traceroute.PNG)

    Notice that traffic doesn't flow directly between the sites. Instead, it traverses the Firewall (IP of 10.100.40.1 in this case) and then goes to Site 30 VPN 20.

10. Click on **Select Device** in the top left-hand corner and select **vEdge30**. Enter a **Destination IP** of *10.20.10.2* and a **VPN** of *VPN - 20*. Specify a **Source/Interface** of *ge0/3* and click on **Start**. We are doing a traceroute from Site 30 VPN 20 to Site 20 VPN 10

    ![](/images/InterVPN_ServiceChaining/190_tragain.PNG)

    In this case as well, traffic traverses the Firewall (IP of 10.100.40.5) and then goes to Site 20 VPN 10.

This completes the Service Chaining lab activity.

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
