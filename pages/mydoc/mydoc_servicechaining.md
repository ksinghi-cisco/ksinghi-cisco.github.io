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
