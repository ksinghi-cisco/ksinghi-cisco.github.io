---
title: Updating Device Templates with Service Side VPNs
tags: [formatting]
keywords: labels, buttons, bootstrap, api methods
last_updated: May 26, 2020
summary: "Associate the Service Side VPN Templates with the Device Templates"
sidebar: mydoc_sidebar
permalink: mydoc_devtemp_update.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>

- Updating vEdge Device Templates for Service Side VPNs
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the DC-vEdge Device Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the Site 20 Device Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the Site 30 Device Template
    <br/>
- Updating cEdge Device Templates for Service Side VPNs
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the Site 40 Device Template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Updating the Site 50 Device Template
    <br/>

" type="primary" %}

## Updating vEdge Device Templates for Service Side VPNs

Since our Feature Templates for Service Side VPNs are ready, we will now update the Device Templates to push the corresponding configuration to the Devices.

### Updating the DC-vEdge Device Template

1. On the vManage GUI, go to **Configuration => Templates**. You should be on the **Device** tab. Locate the *DCvEdge_dev_temp* and click on the 3 dots next to it. Choose to **Edit** the template

    ![](/images/Update_Dev_Temp_SSV/01_editdc.PNG)

2. Scroll down to the **Service VPN** section and click on **Add VPN**. Move *vedge-vpn10* to the list of **Selected VPN Templates** and click on **Next**

    ![](/images/Update_Dev_Temp_SSV/02_addvpntemp.PNG)

3. Under **Additional VPN Templates** on the left-hand side, click on **VPN Interface**

    ![](/images/Update_Dev_Temp_SSV/03_vpnint2.PNG)

4. Choose the *vedge-vpn10-int* template from the drop down and click on **Add**.

    ![](/images/Update_Dev_Temp_SSV/04_addsave.PNG)

5. Click on **Add VPN** under **Service VPN** again (to add the VPN 20 service VPN) and move *vedge-vpn20* under **Selected VPN Templates**. Click on **Next**

    ![](/images/Update_Dev_Temp_SSV/05_addvpntemp20_2.PNG)

6. Click on **VPN Interface** under **Additional VPN Templates** and select the *vedge-vpn20-int* template from the drop down. Click on **Add**

    ![](/images/Update_Dev_Temp_SSV/06_vpnint20.PNG)

7. Make sure the Device Template **Service VPN** section looks as below, and click on **Update**

    ![](/images/Update_Dev_Temp_SSV/07_upd.PNG)

8. Enter the details as shown in the figure below and click on **Next**. These details can be found in the Overview => Topology and IP Addressing section of the guide

    ![](/images/Update_Dev_Temp_SSV/08_dcdet.PNG)

9. Check the side by side configuration to see the commands that will be added and click on **Configure Devices**. Confirm the change and click on **OK**

    ![](/images/Update_Dev_Temp_SSV/09_confdev.PNG)

    ![](/images/Update_Dev_Temp_SSV/10_confirm.PNG)


<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Updating vEdge Device Templates for Service Side VPNs](#updating-vedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the DC-vEdge Device Template~~](#updating-the-dc-vedge-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 20 Device Template](#updating-the-site-20-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 30 Device Template](#updating-the-site-30-device-template)
    <br/>
- [Updating cEdge Device Templates for Service Side VPNs](#updating-cedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 40 Device Template](#updating-the-site-40-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 50 Device Template](#updating-the-site-50-device-template)
    <br/>

" type="primary" %}

### Updating the Site 20 Device Template

Follow the same steps as the previous section, making changes as required.

1. From **Configuration => Templates** locate the *vedge_Site20_dev_temp* Device Template and click on the three dots. Choose to **Edit**.

2. Scroll to the **Service VPN** section and click on **Add VPN**. Move *vedge-vpn10* to the list of Selected VPN Templates and click on **Next**

3. Click on **VPN Interface** under **Additional VPN Templates** and select *vedge-vpn10-int* from the drop down. Click on **Add**

4. Repeat Steps 1 to 3, choosing the *vedge-vpn20* VPN Template and the *vedge-vpn20-int* VPN Interface Template as applicable. Your final Device Template page should look like the image below. Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/11_site20.PNG)

5. Enter the details as shown below and click on **Next**. Click on **Configure Devices** and confirm the selection. You can also reference the table in the Overview => Topology and IP Addressing section of the guide for the device details

    ![](/images/Update_Dev_Temp_SSV/12_site20det.PNG)



<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Updating vEdge Device Templates for Service Side VPNs](#updating-vedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the DC-vEdge Device Template~~](#updating-the-dc-vedge-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 20 Device Template~~](#updating-the-site-20-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 30 Device Template](#updating-the-site-30-device-template)
    <br/>
- [Updating cEdge Device Templates for Service Side VPNs](#updating-cedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 40 Device Template](#updating-the-site-40-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 50 Device Template](#updating-the-site-50-device-template)
    <br/>

" type="primary" %}

### Updating the Site 30 Device Template

Follow the same steps as the previous section, making changes as required.

1. From **Configuration => Templates** locate the *vedge30_dev_temp* Device Template and click on the three dots. Choose to **Edit**.

2. Scroll to the **Service VPN** section and click on **Add VPN**. Move *vedge-vpn10* to the list of Selected VPN Templates and click on **Next**

3. Click on **VPN Interface** under **Additional VPN Templates** and select *vedge-vpn10-int* from the drop down. Click on **Add**

4. Repeat Steps 1 to 3, choosing the *vedge-vpn20* VPN Template and the *vedge-vpn20-int* VPN Interface Template as applicable. Your final Device Template page should look like the image below. Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/13_site30.PNG)

5. Enter the details as shown below and click on **Next**. Click on **Configure Devices**. You can also reference the table in the Overview => Topology and IP Addressing section of the guide for the device details

    ![](/images/Update_Dev_Temp_SSV/14_site30det.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Updating vEdge Device Templates for Service Side VPNs~~](#updating-vedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the DC-vEdge Device Template~~](#updating-the-dc-vedge-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 20 Device Template~~](#updating-the-site-20-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 30 Device Template~~](#updating-the-site-30-device-template)
    <br/>
- [Updating cEdge Device Templates for Service Side VPNs](#updating-cedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 40 Device Template](#updating-the-site-40-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 50 Device Template](#updating-the-site-50-device-template)
    <br/>

" type="primary" %}

## Updating cEdge Device Templates for Service Side VPNs

The cEdges will have 3 Service Side VPNs associated (VPN 10, VPN 20 and VPN 30) with them. We have already created the Feature Templates for these and are now going to update the Device Templates for the cEdges to reflect these Feature Templates.

### Updating the Site 40 Device Template

1. While on **Configuration => Templates**, click on the three dots next to *cEdge_dualuplink_devtemp* and choose to **Edit**. Scroll down to the **Service VPN** section and click on **Add VPN**. Move *cedge-vpn10* to the list of Selected VPN Templates. Click on **Next**

    ![](/images/Update_Dev_Temp_SSV/15_cedgevpn10.PNG)

2. Click on **Cisco VPN Interface Ethernet** under Additional Cisco VPN Templates and choose *cedge-vpn10-int* in the drop down. Click on **Add**

    ![](/images/Update_Dev_Temp_SSV/16_v10int.PNG)

3. Repeat steps 1 and 2 for *cedge-vpn20*, *cedge-vpn20-int* and then for *cedge-vpn30*, *cedge-vpn30-int*. Reference the images given below

    | ![](/images/Update_Dev_Temp_SSV/17_vpn20.PNG) |
    |:---:|
    | Adding cedge-vpn20 |

    | ![](/images/Update_Dev_Temp_SSV/18_vpn20int.PNG) |
    |:---:|
    | Adding cedge-vpn20-int |

    | ![](/images/Update_Dev_Temp_SSV/19_vpn30.PNG) |
    |:---:|
    | Adding cedge-vpn30 |

    | ![](/images/Update_Dev_Temp_SSV/20_vpn30int.PNG) |
    |:---:|
    | Adding cedge-vpn30-int |

4. Click on **Update** once done adding all three VPNs. The final Device Template page should look like this

    ![](/images/Update_Dev_Temp_SSV/21_upd.PNG)

5. Click on the three dots next to the device and choose **Edit Device Template**. Enter the details as shown (details are also available in the Overview => Topology and IP Addressing section of the lab guide). Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/22_editdevtemp.PNG)

6. Choose side-by-side config diff if you want to view the configuration changes being made. Click on **Configure Devices**

    ![](/images/Update_Dev_Temp_SSV/23_confdev.PNG)

This completes the configuration of the Site 40 cEdges for Service Side VPNs.


<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Updating vEdge Device Templates for Service Side VPNs~~](#updating-vedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the DC-vEdge Device Template~~](#updating-the-dc-vedge-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 20 Device Template~~](#updating-the-site-20-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 30 Device Template~~](#updating-the-site-30-device-template)
    <br/>
- [Updating cEdge Device Templates for Service Side VPNs](#updating-cedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 40 Device Template~~](#updating-the-site-40-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Updating the Site 50 Device Template](#updating-the-site-50-device-template)
    <br/>

" type="primary" %}

### Updating the Site 50 Device Template

1. From **Configuration => Templates**, choose to **Edit** the *cEdge-single-uplink* Template

    ![](/images/Update_Dev_Temp_SSV/24_singleuplinkedit.PNG)

2. Under **Service VPN**, choose **Add VPN** and move *cedge-vpn10* to the list of **Selected VPN Templates** and click on Next

    ![](/images/Update_Dev_Temp_SSV/25_vpn10.PNG)

3. Click on **Cisco VPN Interface Ethernet** under Additional Cisco VPN Templates and choose *cedge-vpn10-int* in the drop down. Click on **Add**

    ![](/images/Update_Dev_Temp_SSV/25_vpn10int.PNG)

4. Perform Steps 2 and 3 for *cedge-vpn20*, *cedge-vpn20-int* and *cedge-vpn30*, *cedge-vpn30-int*. The final Device Template should look like the image below. Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/26_upd.PNG)

5. Choose to **Edit Device Template** next to cEdge50 and enter the details as shown below. Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/27_ce50det.PNG)

6. Choose to **Edit Device Template** next to cEdge51 and enter the details as shown below. Click on **Update**

    ![](/images/Update_Dev_Temp_SSV/28_ce51upd.PNG)

7. Click on **Next** and choose to **Configure Devices**. Confirm the change.

8. For verification, open a Putty session to **vEdge20** and try to ping some of the Service VPN IPs. Enter `ping vpn 10 10.100.10.2` and then `ping vpn 10 10.50.10.2`. The pings should be successful

    ![](/images/Update_Dev_Temp_SSV/29_ver.PNG)
    ```
    ping vpn 10 10.100.10.2
    ping vpn 10 10.50.10.2
    ```

This completes the configuration of our Service Side VPNs for the vEdges and cEdges in our network.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Updating vEdge Device Templates for Service Side VPNs~~](#updating-vedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the DC-vEdge Device Template~~](#updating-the-dc-vedge-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 20 Device Template~~](#updating-the-site-20-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 30 Device Template~~](#updating-the-site-30-device-template)
    <br/>
- [~~Updating cEdge Device Templates for Service Side VPNs~~](#updating-cedge-device-templates-for-service-side-vpns)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 40 Device Template~~](#updating-the-site-40-device-template)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Updating the Site 50 Device Template~~](#updating-the-site-50-device-template)
    <br/>

" type="primary" %}
