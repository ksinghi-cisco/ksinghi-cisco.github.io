---
title: Deploying Single Uplink cEdges
tags: [getting_started, formatting, content_types]
keywords: cEdge50, cEdge51, Deploy
last_updated: Feb 25, 2016
summary: "Deploying Site 50 - contains cEdges with single uplinks to each transport"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_cEdge50_cEdge51.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>

- Creating the cEdge50 and cEdge51 VMs
<br/>
- Onboarding cEdge50 and cEdge51
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Initial Configuration - non SD-WAN mode
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying and modifying Feature Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>

" type="primary" %}

{% include important.html content="We will be deploying and onboarding cEdge50 and cEdge51 in parallel. Make sure that both VMs are deployed and operational, at the end of this activity. Screenshots will be concatenated into the same step for the two devices which means some steps will have to be repeated while going through the lab. Device specific variables in the Feature Templates will play an important role over here" %}

## Creating the cEdge50 and cEdge51 VMs

### Overview

We will be deploying two cEdges in Site 50 via vCenter. cEdge 50 will have a single uplink (Internet), as will cEdge51 (MPLS). Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface        | IP                | Gateway       |
|---------|---------------|---------|-------------------|--------------|------------------|-------------------|---------------|
| 50      | 10.255.255.51 | cEdge50-podX | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.50/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | Internet     | GigabitEthernet2 | 100.100.100.50/24 | 100.100.100.1 |
|         |               |         | Network Adapter 3 | Site50-VPN10 | GigabitEthernet3 | 10.50.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site50-VPN20 | GigabitEthernet4 | 10.50.20.2/24     |               |
|         |               |         | Network Adapter 5 | Site50-VPN30 | GigabitEthernet5 | 10.50.30.2/24     |               |
|         | 10.255.255.52 | cEdge51 | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.51/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | MPLS50       | GigabitEthernet2 | 192.1.2.22/30     | 192.1.2.21    |
|         |               |         | Network Adapter 3 | Site50-VPN10 | GigabitEthernet3 | 10.50.10.3/24     |               |
|         |               |         | Network Adapter 4 | Site50-VPN20 | GigabitEthernet4 | 10.50.20.3/24     |               |
|         |               |         | Network Adapter 5 | Site50-VPN30 | GigabitEthernet5 | 10.50.30.3/24     |               |

{% include tip.html content="Plan your sites and addressing carefully. Proper planning can prevent a number of issues and will help with a successful, early deployment." %}

{% include tip.html content="There is configuration applicable only to virtual vEdges/cEdges in some of the sections. Physical cEdges/vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options." %}

### Deploying the VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

    ![](/images/Deploying_DC_vEdge1/03_logintovcenter_usecredsprovided.PNG)
2. We should see the vEdges and cEdges from previous sections of the lab deployed.


3. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_cEdge40/04_depovf.PNG)
4. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *csr1000v-univer*. Click on Next.

    ![](/images/Deploying_cEdge50_cEdge51/01_Deploy_cEdge50.PNG)
5. Change the Virtual Machine name to **cEdge50-podX** or **cEdge51-podX**, depending on the VM being deployed and click on Next (where X is your POD number)

    {% include note.html content="We will only use the podX suffix over here to distinguish between different VMs in our Data Center. The rest of the guide will refer to these VMs as **cEdge50** and **cEdge51**." %}

    |![](/images/Deploying_cEdge50_cEdge51/02_namechange.PNG)|
    |:--:|
    | *cEdge50* |

    |![](/images/Deploying_cEdge50_cEdge51/05_Deploy_cEdge51.PNG)|
    |:--:|
    | *cEdge51* |
6. Select the host assigned to you (image shown as an example only) and click on Next

    {% include note.html content="If the screen gets stuck over here at **Validating** then close Chrome and open the vCenter in Internet Explorer, going through the same steps. Deployment should go through. This is a known issue with Google Chrome." %}

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
7. Review the details shown and click on Next.  Select the **Small** option (1 vCPU and 4 GB RAM) and click on Next

    ![](/images/Deploying_cEdge40/07_revdet.PNG)

    ![](/images/Deploying_cEdge50_cEdge51/03_smallhw.PNG)
8. Choose the Datastore and click on Next.

    ![](/images/Deploying_DC_vEdge1/10_storage_next.PNG)
9. Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    |![](/images/Deploying_cEdge50_cEdge51/04_netad.PNG)|
    |:--:|
    | Networks for cEdge50 |

    |![](/images/Deploying_cEdge50_cEdge51/06_ce51_netad.PNG)|
    |:--:|
    | Networks for cEdge51 |

10. Click Next on **Customize Template** and then Click on **Finish** to deploy your cEdge50-podX and cEdge51-podX VM

    ![](/images/Deploying_cEdge40/10_nextcusttemp.PNG)

11. Once the VM is deployed, right click **cEdge50-podX** and/or **cEdge51-podX** and click Edit settings (image shown as reference only).

    ![](/images/Deploying_cEdge40/12_editsett.PNG)
12. Click on **Add New Device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 3 Network Adapters but we will need 5 for our lab). Repeat this step for a total of 5 Network Adapters. This will need to be done for each VM (cEdge50-podX and cEdge51-podX)

    |![](/images/Deploying_cEdge50_cEdge51/07_ce50addneta.PNG)|
    |:---:|
    | One network adapter added on cEdge50 |

13. After adding two new network adapters from the previous step, click on the drop down next to the first **New Network** in the list of Network Adapters and click on *Browse*

14. Choose the **Site50-VPN20** Network and click on OK. Do the same for the next network adapter, allocating it to **Site50-VPN30**. Make sure the Network Adapters match with the images below and click on OK again
    {% include warning.html content="The Network Adapter mapping might vary based on the version of cEdge being deployed. Sometimes, trial and error is the easiest way to figure out which Network Adapter maps to which interface on the cEdge" %}

    | ![](/images/Deploying_cEdge50_cEdge51/08_ce50_networks.PNG) |
    |:---:|
    | Networks on cEdge50 |

    | ![](/images/Deploying_cEdge50_cEdge51/09_ce51_addnetad.PNG) |
    |:---:|
    | Networks on cEdge51 |

15. Click on cEdge50-podX and/or cEdge51-podX and choose to power them on. Console in to the devices as well, for the next section. Wait for the cEdges to boot up completely

![](/images/Deploying_cEdge50_cEdge51/10_poweronboth.PNG)

<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [Onboarding cEdge50 and cEdge51](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Initial Configuration - non SD-WAN mode](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying and modifying Feature Templates](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating and Attaching Device Templates](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying the Bootstrap file and converting to SD-WAN IOS-XE mode](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>


" type="primary" %}

## Onboarding cEdge50 and cEdge51

### Initial Configuration - non SD-WAN mode

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface        | IP                | Gateway       |
|---------|---------------|---------|-------------------|--------------|------------------|-------------------|---------------|
| 50      | 10.255.255.51 | cEdge50 | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.50/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | Internet     | GigabitEthernet2 | 100.100.100.50/24 | 100.100.100.1 |
|         |               |         | Network Adapter 3 | Site50-VPN10 | GigabitEthernet3 | 10.50.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site50-VPN20 | GigabitEthernet4 | 10.50.20.2/24     |               |
|         |               |         | Network Adapter 5 | Site50-VPN30 | GigabitEthernet5 | 10.50.30.2/24     |               |
|         | 10.255.255.52 | cEdge51 | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.51/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | MPLS50       | GigabitEthernet2 | 192.1.2.22/30     | 192.1.2.21    |
|         |               |         | Network Adapter 3 | Site50-VPN10 | GigabitEthernet3 | 10.50.10.3/24     |               |
|         |               |         | Network Adapter 4 | Site50-VPN20 | GigabitEthernet4 | 10.50.20.3/24     |               |
|         |               |         | Network Adapter 5 | Site50-VPN30 | GigabitEthernet5 | 10.50.30.3/24     |               |

{% include tip.html content="Starting from IOS-XE 17.2, the cEdge platforms use a Universal image. One can switch from non SD-WAN mode to SD-WAN mode via a command" %}

1. We will first console in to the cEdges and set up an IP Address with basic routing to ensure that cEdge50 and cEdge51 can reach vManage and the Jumphost. This is done by issuing `ip route 0.0.0.0 0.0.0.0 192.168.0.1` followed by `interface GigabitEthernet1` and giving an IP Address to the interface through `ip address 192.168.0.50 255.255.255.0` for cEdge50 and `ip address 192.168.0.51 255.255.255.0` for cEdge51. Make sure you `no shut` the interface.

    Additionally, we will be SCP'ing files over to the cEdges (root certificates) from vManage
    ![](/images/Deploying_cEdge50_cEdge51/11_cedge50_nonsdwan.PNG)

    ![](/images/Deploying_cEdge50_cEdge51/12_cedge51_nonsdwan.PNG)

    **Configuration for cEdge50**
    ```
    enable
    conf t
    interface GigabitEthernet1
     ip address 192.168.0.50 255.255.255.0
     no shut
     exit
    ip route 0.0.0.0 0.0.0.0 192.168.0.1
    ip scp server enable
    username admin priv 15 sec admin
    line vty 0 4
     login local
     do wr
    ```

    **Configuration for cEdge51**
    ```
    enable
    conf t
    interface GigabitEthernet1
     ip address 192.168.0.51 255.255.255.0
     no shut
     exit
    ip route 0.0.0.0 0.0.0.0 192.168.0.1
    ip scp server enable
    username admin priv 15 sec admin
    line vty 0 4
     login local
     do wr
    ```

2. Verify connectivity to the vManage and the JumpHost (IP of the Jumphost might vary) by pinging **192.168.0.6** and/or the IP Address of your Jumphost from the console session of both devices

    ![](/images/Deploying_cEdge40/21_verifconn_samplejump.PNG)

<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [Onboarding cEdge50 and cEdge51](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Initial Configuration - non SD-WAN mode~~](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying and modifying Feature Templates](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating and Attaching Device Templates](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying the Bootstrap file and converting to SD-WAN IOS-XE mode](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>


" type="primary" %}

### Copying and Modifying Feature Templates

{% include note.html content="The Feature and Device Templates enumerated here and in the next section might already be created for you. However, it is a good practice to go through the steps below and validate the settings in the templates. This will help in familiarization with the lab setup and with fixing any deltas that might exist. If you don't see them in the configuration, please add the templates and follow the steps as enumerated below." %}

Templates are the key configuration components of the Cisco SD-WAN solution. They help with deploying large scale solutions with minimal effort. While there is quite a lot of initial configuration that goes into setting up these templates, their usefulness is highlighted when we're looking at onboarding multiple devices in a quick and efficient manner, reusing generic templates for devices.

We will make use of the templates that were created for cEdge40, repurposing them for cEdge50 and cEdge51 through the use of Device Specific parameters.

1. On the vManage GUI, navigate to **Configuration (the cog wheel icon on the left) -> Templates**

    ![](/images/Deploying_cEdge40/22_gototemp.PNG)

2. Click on the Feature tab to access the Feature templates. Click on the three dots next to the  *cEdge_VPN0_dual_uplink* template and click on **Copy**

    ![](/images/Deploying_cEdge50_cEdge51/15_copytemp.PNG)

3. Rename the template to *cEdge_VPN0_single_uplink* and change the description to *cEdge VPN 0 Template for Single Uplinks*. Click on **Copy**. Click on the 3 dots next to the newly copied template and click on **Edit**. The name, description and VPN should be as shown below

    ![](/images/Deploying_cEdge50_cEdge51/16_templupdnamedesc.PNG)

4. Navigate to the **IPv4 Route** section. A route should be populated there. Click on the pencil icon to edit the route and click on **1 next hop**. Make sure the next hop is a Device Variable named *vpn0_next_hop_ip_address_0* (should already be the case, from our previous use of the parameter).

    ![](/images/Deploying_cEdge50_cEdge51/17_editipv4route.PNG)

    ![](/images/Deploying_cEdge50_cEdge51/18_clicknh.PNG)

5. Click on **Update** in the lower portion of the screen to update the changes

6. On the Feature Template page, click on the three dots next to *cedge-vpn0-int-dual* and click on **Copy**. Enter the Template Name as *cedge-vpn0-int-single* and a description of *cedge VPN 0 Interface Template for devices with a single uplink*. Click on **Copy**

    ![](/images/Deploying_cEdge50_cEdge51/19_inttempup.PNG)

7. Click on the three dots next to the newly copied template and choose to **Edit** it. Verify the Template Name and Description and update parameters as per the following table

    | Section                    | Field                        | Global or Device Specific (drop down) | Value                                                             |
    |----------------------------|------------------------------|---------------------------------------|-------------------------------------------------------------------|
    |                            | Template Name                | NA                                    | *cedge-vpn0-int-single*                                           |
    |                            | Description                  | NA                                    | *cedge VPN 0 Interface Template for devices with a single uplink* |
    | Basic Configuration        | Interface Name               | Device Specific                       | *vpn0_if_name*                                                    |
    | Basic Configuration - IPv4 | IPv4 Address / prefix-length | Device Specific                       | *vpn0_ipv4_address*                                               |
    | Tunnel                     | Tunnel Interface             | Global                                | On                                                                |
    | Tunnel                     | Color                        | Device Specific                       | *vpn0_if_tunnel_color_value*                                      |
    | Tunnel                     | Restrict                     | Device Specific                       | *vpn0_if_tunnel_color_restrict*                                   |
    | Tunnel - Allow Service     | All                          | Global                                | On                                                                |

    ![](/images/Deploying_cEdge50_cEdge51/19_inttname.PNG)

    ![](/images/Deploying_cEdge50_cEdge51/20_varupd_ip_col_rest.PNG)

We are done with creating feature templates for the initial onboarding of cEdge50 and cEdge51. Notice, this was less work than before since we could simply copy the template already created for cEdge40.

<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [Onboarding cEdge50 and cEdge51](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Initial Configuration - non SD-WAN mode~~](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying and modifying Feature Templates~~](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating and Attaching Device Templates](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying the Bootstrap file and converting to SD-WAN IOS-XE mode](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>


" type="primary" %}

### Creating and Attaching Device Templates

The feature templates created in the previous sections are referenced in Device Templates. Devices are then attached to Device Templates which pushes configuration to them, in line with the settings in the Feature templates. The general workflow for templates is given below
![](/images/Deploying_cEdge40/99_DevTemp_Flow.PNG)

1. Go to **Configuration -> Templates** and click on the three dots next to the Device Template named *cEdge_dualuplink_devtemp*. Click on **Copy**

    ![](/images/Deploying_cEdge50_cEdge51/95_copydevtemp.PNG)

2. Change the template name to *cEdge-single-uplink* and the description to *Single Uplink cEdge Device Template*. Click on **Copy**

    ![](/images/Deploying_cEdge50_cEdge51/94_copy.PNG)

3. Click on the three dots next to the newly copied template and choose to **Edit** it. Make sure the details under **Transport and Management VPN** are populated as below, updating VPN0 and VPN0 Interface templates with the ones we just created. **VPN512 templates are being re-used from before**. Click on **Update** once done

    {% include tip.html content="You can create templates on the fly if the template hasn't already been created. This can be done via the **Create Template** hyperlink from the drop down menu" %}

    {% include important.html content="To get the option of selecting a **Cisco VPN Interface Ethernet** as shown below, click on **Cisco VPN Interface Ethernet** on the right hand side under the **Additional Templates** portion of the screen. This applies to both the VPN 0 and the VPN 512 sections" %}

    | Section                      | Field         | Sub Field                    | Value (Drop Down)        |
    |------------------------------|---------------|------------------------------|--------------------------|
    | Transport and Management VPN | Cisco VPN 0   |                              | cEdge_VPN0_single_uplink   |
    | Transport and Management VPN | Cisco VPN 0   | Cisco VPN Interface Ethernet | cedge-vpn0-int-single      |
    | Transport and Management VPN | Cisco VPN 512 |                              | cEdge_VPN512_dual_uplink |
    | Transport and Management VPN | Cisco VPN 512 | Cisco VPN Interface Ethernet | cedge-vpn512-int-dual    |



    ![](/images/Deploying_cEdge50_cEdge51/21_reuse512.PNG)

4. Once created, the Device Template will need to be attached to a Device for it to take effect. Click on the three dots (right-hand side) of the *cEdge-single-uplink* template and click on **Attach Devices**

    ![](/images/Deploying_cEdge50_cEdge51/22_attachtempl.PNG)

5. We will be presented with a list of devices that can be associated with this template. Choose any two available devices that have not been attached to a template before, making note of the Name (e.g. the device with a name ending in **38F4** has been selected for cEdge50 and the one ending in **FBA3** has been selected for cEdge51). Click on **Attach**

    ![](/images/Deploying_cEdge50_cEdge51/23_notethename.PNG)

6. This should take you to a page which shows the attached devices. Click on the three dots (right-hand side) next to the cEdge50 Device (whatever name was selected before) and click on **Edit Device Template**. Also, make note of the cross mark next to the device name, on the left-hand side. This is the point where we need to enter details for the device specific values populated in the Feature Templates.

7. Enter details as per the screenshot below (these can be found in the table referenced at the beginning of this page) and click on **Update**. Once all the data is entered correctly, there should be a green check mark next to the corresponding device

    | ![](/images/Deploying_cEdge50_cEdge51/24_ce50.PNG) |
    |:---:|
    | *Details to be entered for cEdge50* |

    {% include note.html content="We have selected a color for our Tunnel over here. Other devices have tunnels with the default color as of now. When we bring them into vManage mode, a color will be set on them as well" %}

    | ![](/images/Deploying_cEdge50_cEdge51/25_ce51.PNG) |
    |:---:|
    | *Details to be entered for cEdge51* |

    {% include note.html content="The IP Address/Default Gateway of the VPN 0 Interface for cEdge51 is of the MPLS link. The corresponding color has been selected here, setting it to restrict. This means that the tunnel will only build tunnels with TLOCs of the same color" %}

8. Click on the entry in the Device List to view the configuration that will be pushed to the device. Notice that the vBond IP and the Organization Name have been populated. These are taken from the vManage **Administration -> Settings** page, where they need to be populated. Click on **Configure** to configure the device.

Since this isn't a device that exists (as of now), the configuration push is scheduled for later, when a device is associated with this Device Name (the one ending in 73F2). This is done in the next section


<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [Onboarding cEdge50 and cEdge51](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Initial Configuration - non SD-WAN mode~~](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying and modifying Feature Templates~~](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating and Attaching Device Templates~~](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Copying the Bootstrap file and converting to SD-WAN IOS-XE mode](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>


" type="primary" %}

### Copying the Bootstrap file and converting to SD-WAN IOS-XE mode

We will be generating a Bootstrap file for each cEdge and placing it in the flash of the device we want to bring up. The devices should come up and establish control connections with vManage, along with establishing BFD sessions with other devices. cEdge51 will not establish BFD sessions since it has the *restrict* keyword and there isn't any other tunnel with the same color of mpls.

{% include note.html content="While we are placing the Bootstrap file in flash for the lab, this can be put on a USB drive and plugged into the cEdge. This is usually done at a staging facility, post which the device is shipped to the customer site. Once they plug it in and power it on, the bootstrap configuration file allows the device to come up and establish control connections" %}

1. Go to **Configuration -> Devices**

    ![](/images/Deploying_cEdge40/52_config_dev_edit.PNG)

2. Identify the **Chassis Number** that was selected before, while attaching a Device to the Template. In this case, it ended in **38F4** for cEdge50 and **FBA3** for cEdge51. Click on the three dots in the right-hand side and click on **Generate Bootstrap Configuration** for the cEdge50 device. Choose **Cloud-Init** and click on OK.

    ![](/images/Deploying_cEdge50_cEdge51/26_ce50genboot.PNG)

    ![](/images/Deploying_cEdge40/54_cloudinit.PNG)

3. Download the bootstrap file (will get saved to the Downloads folder by default). It should be a file beginning with CSR...

    ![](/images/Deploying_cEdge50_cEdge51/27_ce50down.PNG)

4. From the Jumphost Desktop, start TFTPD64. Click on **Browse** and choose the Downloads folder (or wherever the renamed .cfg file has been stored). Make sure the 192.168.0.X IP is chosen from the Server Interface drop down in TFTPD64. Copy the name of the file (beginning with CSR) by going to the file location. You can click on copy in TFTPD64 itself, but the name doesn't get copied sometimes. Click on **show dir** to view the files in the currently chosen folder. Clicking on **copy** copies the highlighted file name, but it is a bit flaky on the newer versions of TFTD64

    {% include tip.html content="On bootup, a cEdge looks for a file on it's USB port (if a bootable USB drive is connected) and in bootflash:. The file name must match as above for Cloud type devices (i.e. CSR1K). For physical devices, the file name should be *ciscosdwan.cfg*. If the file is present on the USB drive and in bootflash:, the one in bootflash: takes precedence" %}

    ![](/images/Deploying_cEdge50_cEdge51/28_tftpfile.PNG)

5. Log in to the CLI of cEdge50 (we can log in via Putty now, using the saved session or by SSH'ing to 192.168.0.50) and issue `copy tftp: bootflash:`. Specify a Remote Host IP of your Jumphost (192.168.0.121 in this case). The source file name should be the one we downloaded for cEdge50 (beginning with CSR) and destination file name should be *ciscosdwan_cloud_init.cfg*. The file should get copied over to bootflash: successfully

    ![](/images/Deploying_cEdge50_cEdge51/29_puttyce50.PNG)

    ![](/images/Deploying_cEdge50_cEdge51/30_tftpfiletoce50.PNG)
    ```
    copy tftp: bootflash:
    ```

6. **Repeat steps 1 to 5** for cEdge51, downloading the bootstrap file for it (starting with CSR) and TFTP'ing it over to cEdge51.

    ![](/images/Deploying_cEdge50_cEdge51/31_tftpfiletoce51.PNG)

7. Log in to the CLI of vManage (again, via the saved Putty session or by SSH'ing to 192.168.0.6) and issue the following commands to SCP the ROOTCA.pem file over to cEdge50 and cEdge51

    ![](/images/Deploying_cEdge50_cEdge51/14_copypemtoces.PNG)
    ```
    vshell
    scp ROOTCA.pem admin@192.168.0.50:ROOTCA.pem
    yes
    admin

    scp ROOTCA.pem admin@192.168.0.51:ROOTCA.pem
    yes
    admin
    ```
    The last **admin** over there is the password of cEdge50/cEdge51

8. Go back to the CLI of cEdge50 and cEdge51 and issue `controller-mode enable` from privilege mode. **Confirm** and this should lead to the devices rebooting

    ![](/images/Deploying_cEdge50_cEdge51/32_controllermodeenonce.PNG)
    ```
    controller-mode enable

    ```

We have completed this section of the lab and will now need to wait for the cEdges to reboot. On rebooting, they should pick up the configuration file from bootflash: and connect to the vManage/vSmarts/other vEdges. This will be verified in the next section.

<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [~~Onboarding cEdge50 and cEdge51~~](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Initial Configuration - non SD-WAN mode~~](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying and modifying Feature Templates~~](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating and Attaching Device Templates~~](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying the Bootstrap file and converting to SD-WAN IOS-XE mode~~](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>


" type="primary" %}

## Onboarding Verification

1. On the vManage GUI, go to **Monitor -> Network**. You should see cEdge50 and cEdge51 successfully added on vManage.

    ![](/images/Deploying_cEdge50_cEdge51/93_cedge5051vm.PNG)

2. Clicking on cEdge50 or cEdge51 and going to **Troubleshooting -> Control Connections (Live View)** will show us that the cEdges have established control connections with vManage and the vSmarts. We can check this via the CLI `show sdwan control connections` as well

    | ![](/images/Deploying_cEdge50_cEdge51/33_ce50shconcon.PNG) |
    |:---:|
    | *Control Connections for cEdge50* |

    | ![](/images/Deploying_cEdge50_cEdge51/34_ce51shconconnect.PNG) |
    |:---:|
    | *Control Connections for cEdge51* |

    ```
    show sdwan control connections
    ```

3. Navigate to **Dashboards -> Main Dashboard** and we will see 4 Sites with Full WAN connectivity and 8 WAN Edges

    ![](/images/Deploying_cEdge50_cEdge51/92_maindash.PNG)

    {% include tip.html content="We should be seeing 5 sites with full WAN connectivity, but one of the WAN Edges (cEdge51) has not been able to establish any BFD sessions yet, hence the site itself doesn't have full WAN connectivity" %}

4. The previous observation can be seen if we click on **Full WAN connectivity**. Notice that Site 50 is missing from this list. Once we ensure that there are BFD sessions with cEdge51, the issue should be resolved

    ![](/images/Deploying_cEdge50_cEdge51/91_4sites.PNG)

5. Issue `show sdwan bfd sessions` and we should see that cEdge50 has established BFD sessions, whereas cEdge51 has not

    | ![](/images/Deploying_cEdge50_cEdge51/36_ce50shbfdsess.PNG) |
    |:---:|
    | *BFD sessions - cEdge50* |

    | ![](/images/Deploying_cEdge50_cEdge51/35_shbfdsess.PNG) |
    |:---:|
    | *BFD sessions - cEdge51* |

    ```
    show sdwan bfd sessions
    ```

At this point, we have completed onboarding verification

{% include warning.html content="STOP!!!! Time to take a snapshot. Refer to the Testing Procedure, Step 2 of the SOP. Also, go through the 2 sections we skipped after you take the snapshots." %}

<br>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Creating the cEdge50 and cEdge51 VMs~~](#creating-the-cedge50-and-cedge51-vms)
<br/>
- [~~Onboarding cEdge50 and cEdge51~~](#onboarding-cedge50-and-cedge51)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Initial Configuration - non SD-WAN mode~~](#initial-configuration---non-sd-wan-mode)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying and modifying Feature Templates~~](#copying-and-modifying-feature-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating and Attaching Device Templates~~](#creating-and-attaching-device-templates)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Copying the Bootstrap file and converting to SD-WAN IOS-XE mode~~](#copying-the-bootstrap-file-and-converting-to-sd-wan-ios-xe-mode)
    <br/>
- [~~Onboarding Verification~~](#onboarding-verification)
<br/>


" type="primary" %}
