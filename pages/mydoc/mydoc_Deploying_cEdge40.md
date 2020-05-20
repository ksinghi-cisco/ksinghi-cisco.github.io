---
title: Deploying a Dual Uplink cEdge
tags: [getting_started, formatting, content_types]
keywords: cEdges, cEdge40, Site40, Deploy
last_updated: May 19, 2020
summary: "Deploying Site 40 with a single cEdge which has both transport uplinks"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_cEdge40.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Verifying the current lab setup
<br/>
- Creating the cEdge40 VM
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Initial Configuration - non SD-WAN mode
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Feature Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Verifying the existing lab setup

The vManage, vBond and vSmarts have been deployed along with Sites 1, 20 and 30. We will start by verifying the existing setup.

1. Log in to vManage by clicking on the bookmark or navigating to https://192.168.0.6. Use the following credentials:

    Username | Password
    ------------ | -------------
    admin | admin

    ![](/images/Deploying_DC_vEdge1/01_logintovmanage.PNG)

2. On logging in, you should see 2 vSmarts, 1 vBond and 1 vManage along with 5 WAN Edges. 7 control planes should be up and 3 sites should have WAN connectivity.

    ![](/images/Deploying_cEdge40/01_dash.PNG)

3. Open and log in to the vManage via the CLI - fire up Putty and double click the saved session for vManage or SSH to 192.168.0.6. Use the same credentials as the GUI.

    ![](/images/Deploying_DC_vEdge1/03_cliver.PNG)
4. Issue `show control connections` and you should see the vManage talking to the vSmarts, vBond and vEdges. Note the **System IP** and the fact that all the connections are **up**

    ![](/images/Deploying_cEdge40/02_vercontvm.PNG)

    Look at the System IP to see which device has the vManage established a control connection with. There should be 5 connections to vEdges. This completes the verification activity.
<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
- Creating the cEdge40 VM
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Initial Configuration - non SD-WAN mode
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Feature Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Creating the cEdge40 VM

### Overview

We will be deploying a cEdge in Site 40 via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface        | IP              | Gateway       |
|---------|---------------|---------|-------------------|--------------|------------------|-----------------|---------------|
| 40      | 10.255.255.41 | cEdge40 | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.40/24 | 192.168.0.1   |
|         |               |         | Network Adapter 2 | Internet     | GigabitEthernet2 | 100.100.100.40  | 100.100.100.1 |
|         |               |         | Network Adapter 3 | MPLS40       | GigabitEthernet3 | 192.1.2.18/30   | 192.1.2.17    |
|         |               |         | Network Adapter 4 | Site40-VPN10 | GigabitEthernet4 | 10.40.10.2/24   |               |
|         |               |         | Network Adapter 5 | Site40-VPN20 | GigabitEthernet5 | 10.40.20.2/24   |               |
|         |               |         | Network Adapter 6 | Site40-VPN30 | GigabitEthernet6 | 10.40.30.2/24   |               |

{% include tip.html content="Plan your sites and addressing carefully. Proper planning can prevent a number of issues and will help with a successful, early deployment." %}

{% include tip.html content="There is configuration applicable only to virtual vEdges/cEdges in some of the sections. Physical cEdges/vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options." %}

### Deploying the VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

    ![](/images/Deploying_DC_vEdge1/03_logintovcenter_usecredsprovided.PNG)
2. We should see the vEdges from previous sections of the lab deployed.

    ![](/images/Deploying_cEdge40/03_vcdep.PNG)
3. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_cEdge40/04_depovf.PNG)
4. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *csr1000v-univer*. Click on Next.

    ![](/images/Deploying_cEdge40/05_csrimage.PNG)
5. Change the Virtual Machine name to **cEdge40** and click on Next.

    ![](/images/Deploying_cEdge40/06_renamecedge.PNG)
6. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
7. Review the details shown and click on Next.  Select the **Large** option (4 vCPUs and 4 GB RAM) and click on Next

    ![](/images/Deploying_cEdge40/07_revdet.PNG)

    ![](/images/Deploying_cEdge40/08_largedep.PNG)
8. Choose the Datastore and click on Next.

    ![](/images/Deploying_DC_vEdge1/10_storage_next.PNG)
9. Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_cEdge40/09_netad.PNG)
10. Click Next on **Customize Template** and then Click on **Finish** to deploy your cEdge40 VM

    ![](/images/Deploying_cEdge40/10_nextcusttemp.PNG)

    ![](/images/Deploying_cEdge40/11_summfin.PNG)
11. Once the VM is deployed, right click **cEdge40** and click Edit settings.

    ![](/images/Deploying_cEdge40/12_editsett.PNG)
12. Change the memory to 8 GB and choose to **Add a new device** (top right corner). Select Network Adapter to add one (since our deployed VM has only 3 Network Adapters but we will need 6 for our lab). Do this twice more for a grand total of 6 Network Adapters

    ![](/images/Deploying_cEdge40/13_mem8gb.PNG)

    ![](/images/Deploying_cEdge40/14_adddevnad.PNG)

    ![](/images/Deploying_cEdge40/15_total3netad.PNG)
13. Click on the drop down next to the first **New Network** and click on *Browse*

    ![](/images/Deploying_cEdge40/16_dropbrowse.PNG)
14. Choose the **Site40-VPN10** Network and click on OK. Do the same for the next two network adapters, allocating them to **Site40-VPN20** and **Site40-VPN30** respectively. Make sure the Network Adapters match with the second image below and click on OK again

    {% include warning.html content="The Network Adapter mapping might vary based on the version of cEdge being deployed. Sometimes, trial and error is the easiest way to figure out which Network Adapter maps to which interface on the cEdge" %}
    ![](/images/Deploying_cEdge40/17_site40vpn10.PNG)

    ![](/images/Deploying_cEdge40/18_allnetads.PNG)
15. Click on cEdge40 and choose to power it on

<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
~~- Creating the cEdge40 VM~~
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Initial Configuration - non SD-WAN mode
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Feature Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Onboarding cEdge40

### Initial Configuration - non SD-WAN mode

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface        | IP              | Gateway       |
|---------|---------------|---------|-------------------|--------------|------------------|-----------------|---------------|
| 40      | 10.255.255.41 | cEdge40 | Network Adapter 1 | Management   | GigabitEthernet1 | 192.168.0.40/24 | 192.168.0.1   |
|         |               |         | Network Adapter 2 | Internet     | GigabitEthernet2 | 100.100.100.40  | 100.100.100.1 |
|         |               |         | Network Adapter 3 | MPLS40       | GigabitEthernet3 | 192.1.2.18/30   | 192.1.2.17    |
|         |               |         | Network Adapter 4 | Site40-VPN10 | GigabitEthernet4 | 10.40.10.2/24   |               |
|         |               |         | Network Adapter 5 | Site40-VPN20 | GigabitEthernet5 | 10.40.20.2/24   |               |
|         |               |         | Network Adapter 6 | Site40-VPN30 | GigabitEthernet6 | 10.40.30.2/24   |               |

{% include tip.html content="Starting from IOS-XE 17.2, the cEdge platforms use a Universal image. One can switch from non SD-WAN mode to SD-WAN mode via a command" %}

1. We will first console in to the cEdge and set up an IP Address with basic routing to ensure that the cEdge can reach vManage and the Jumphost. This is done by issuing `ip route 0.0.0.0 0.0.0.0 192.168.0.1` followed by `interface GigabitEthernet1` and giving an IP Address to the interface through `ip address 192.168.0.40 255.255.255.0`. Make sure you `no shut` the interface.

    Additionally, we will be SCP'ing files over to the cEdge (root certificates) from vManage
    ![](/images/Deploying_cEdge40/20_nonsdwaninit.PNG)

    ![](/images/Deploying_cEdge40/60_enablescp_putpass.PNG)
    ```
    enable
    conf t
    interface GigabitEthernet1
     ip address 192.168.0.40 255.255.255.0
     no shut
     exit
    ip route 0.0.0.0 0.0.0.0 192.168.0.1
    ip scp server enable
    username admin priv 15 sec admin
    line vty 0 4
     login local
     do wr
    ```

2. Verify connectivity to the vManage and the JumpHost (IP of the Jumphost might vary) by pinging **192.168.0.6** and/or the IP Address of your Jumphost

    ![](/images/Deploying_cEdge40/21_verifconn_samplejump.PNG)

<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
~~- Creating the cEdge40 VM~~
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Initial Configuration - non SD-WAN mode~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Feature Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

### Setting up Feature Templates

Templates are the key configuration components of the Cisco SD-WAN solution. They help with deploying large scale solutions with minimal effort. While there is quite a lot of initial configuration that goes into setting up these templates, their usefullness is highlighted when we're looking at onboarding multiple devices in a quick and efficient manner, reusing generic templates for devices.

1. On the vManage GUI, navigate to **Configuration (the cog wheel icon on the left) -> Templates**

    ![](/images/Deploying_cEdge40/22_gototemp.PNG)

2. Click on the Feature tab to access the Feature templates. Click on **Add Template**

    ![](/images/Deploying_cEdge40/23_Feature.PNG)

3. Search for csr and select CSR1000v on the left-hand side. This should give the option to select a template from the right. Choose **Cisco VPN** template

    ![](/images/Deploying_cEdge40/24_csr_vpn.PNG)

4. Name your template *cEdge_VPN0_dual_uplink* and give a description of *cEdge VPN 0 Template for Dual Uplinks*. Enter the VPN as 0.

    ![](/images/Deploying_cEdge40/25_tempname.PNG)

5. Click on **IPv4 Route** and then choose **New IPv4 Route**

    ![](/images/Deploying_cEdge40/26_ipv4routenew.PNG)

6. Enter the **Prefix** as *0.0.0.0/0* and click on **Add Next Hop**. We're adding the default route for VPN 0 (draw parallels with the manual configuration that was done on the vEdges)

    ![](/images/Deploying_cEdge40/27_defroute_addhop.PNG)

7. Click on **Add Next Hop** again and choose **Device Specific** from the Address drop down. Enter *vpn0_next_hop_ip_address_0*. Click on Add.

    ![](/images/Deploying_cEdge40/28_addnexthopagain.PNG)

    ![](/images/Deploying_cEdge40/29_devspec.PNG)

8. Make sure you have **1 Next Hop** showing up in the IPv4 Route window and click on **Add** again. Once on the main Template page, click on **Create** to create your Feature Template

    ![](/images/Deploying_cEdge40/30_clickadd.PNG)

9. Choose to **Add Template**, searching and selecting CSR1000v like before. This time, choose to add a **Cisco VPN Interface Ethernet** template

    ![](/images/Deploying_cEdge40/32_addftemp_csr_vpnint.PNG)

10. Populate the details as shown in the table below. Screenshots may be used as reference. Cick on **Create** at the end to create your Feature Template.

    | Section                    | Field                        | Global or Device Specific (drop down) | Value                                                         |
    |----------------------------|------------------------------|---------------------------------------|---------------------------------------------------------------|
    |                            | Template Name                | NA                                    | *cedge-vpn0-int-dual*                                         |
    |                            | Description                  | NA                                    | cEdge VPN 0 Interface Template for Devices with a dual uplink |
    | Basic Configuration        | Shutdown                     | Global                                | No                                                            |
    | Basic Configuration        | Interface Name               | Global                                | GigabitEthernet2                                              |
    | Basic Configuration - IPv4 | IPv4 Address / prefix-length | Device Specific                       | *inet_ipv4_address*                                           |
    | Tunnel                     | Tunnel Interface             | Global                                | On                                                            |
    | Tunnel                     | Color                        | Device Specific                       | *inet_if_tunnel_color_value*                                  |
    | Tunnel - Allow Service     | All                          | Global                                | On                                                            |

    ![](/images/Deploying_cEdge40/33_cedgeiface.PNG)

    ![](/images/Deploying_cEdge40/34_ip_tunn.PNG)

    ![](/images/Deploying_cEdge40/34_tuncolall.PNG)

11. You should now see the feature template created. We now need to create the feature templates for VPN 512 and the VPN 512 Interface. The power of templates becomes apparent at this point since we can copy a template that was created previously and tweak it as per the requirement. Click on the three dots at the end of the *cEdge_VPN0_dual_uplink* template and click on **Copy**

    ![](/images/Deploying_cEdge40/35_copy_vpn0_temp.PNG)

12. You will be prompted to name the copied template. Give it a name of *cEdge_VPN512_dual_uplink* and update the description to *cEdge VPN 512 Template for Dual Uplinks* (sometimes, the description doesn't get updated and needs to be done again when editing the template). Click on **Copy**.

    ![](/images/Deploying_cEdge40/36_vpn512_tempcopy.PNG)

13. Click on the three dots next to the newly created template and choose to **Edit**. Notice that the description did not get updated in the screenshot below, so we will edit it while tweaking the template

    ![](/images/Deploying_cEdge40/37_vpn512_edit_descdoesntchangesometimes.PNG)

14. Populate the details as follows. To populate the IPv4 Route, click on the edit (pencil icon) next to the existing IPv4 Route and then click on **1 Next Hop**. Edit and click on **Save Changes**

    | Section             | Field                        | Global or Device Specific (drop down) | Value                                   |
    |---------------------|------------------------------|---------------------------------------|-----------------------------------------|
    |                     | Template Name                | NA                                    | *cedge_vpn512_dual_uplink*              |
    |                     | Description                  | NA                                    | cEdge VPN 512 Template for Dual Uplinks |
    | Basic Configuration | VPN                          | Global                                | 512                                     |
    | IPv4 Route          | Update IPv4 Route - Next Hop | Device Specific                       | *vpn512_next_hop_ip_address_0*          |

    ![](/images/Deploying_cEdge40/38_changevpn512.PNG)

    ![](/images/Deploying_cEdge40/39_editipv4route.PNG)

    ![](/images/Deploying_cEdge40/40_devspecvpn_512.PNG)

15. Make a copy of the VPN 0 Interface template so as to use it for VPN 512. Click on the 3 dots next to the template *cedge-vpn0-int-dual* and click on **Copy**. Update the name and description to *cedge-vpn512-int-dual* and *cEdge VPN 512 Interface Template for devices with a dual uplink* and click on **Copy**

    ![](/images/Deploying_cEdge40/41_copyintfor512.PNG)

16. Click on the three dots next to the newly copied template and choose to **Edit** it. Populate the details as given in the table below and click on **Save Changes**

    | Section                    | Field                        | Global or Device Specific (drop down) | Value                                                           |
    |----------------------------|------------------------------|---------------------------------------|-----------------------------------------------------------------|
    |                            | Template Name                | NA                                    | *cedge-vpn512-int-dual*                                         |
    |                            | Description                  | NA                                    | cEdge VPN 512 Interface Template for devices with a dual uplink |
    | Basic Configuration        | Shutdown                     | Global                                | No                                                              |
    | Basic Configuration        | Interface Name               | Global                                | GigabitEthernet1                                                |
    | Basic Configuration - IPv4 | IPv4 Address / prefix-length | Device Specific                       | *vpn512_mgmt_ipv4_address*                                      |
    | Tunnel                     | Tunnel Interface             | Global                                | Off                                                             |

    ![](/images/Deploying_cEdge40/42_edittempname_int_desc_ip.PNG)

    ![](/images/Deploying_cEdge40/43_tunnoff.PNG)

We are done with creating feature templates (for now) and while it was a lot of work, these templates can be reused and/or repurposed as required.

<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
~~- Creating the cEdge40 VM~~
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Initial Configuration - non SD-WAN mode~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Setting up Feature Templates~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating and Attaching Device Templates
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

### Creating and Attaching Device Templates

The feature templates created in the previous sections are referenced in Device Templates. Devices are then attached to Device Templates which pushes configuration to them, in line with the settings in the Feature templates. The general workflow for templates is given below
![](/images/Deploying_cEdge40/99_DevTemp_Flow.PNG)

1. From the **Configuration -> Templates** window, make sure you're on the **Device** tab and click on **Create Template**. Choose to create a template From Feature Template

    ![](/images/Deploying_cEdge40/44_devtemp_fromfeat.PNG)

2. Choose CSR1000v as the Device Model and enter *cedge_dualuplink_devtemp* for the **Template Name** and *cedge Device Template for devices with a dual uplink* as the **Description**

    ![](/images/Deploying_cEdge40/45_devtemplatenew.PNG)

3. In the template, navigate to the **Transport & Management VPN** section. Update the fields as per the table below, selecting templates which we created before and click on **Create** to create the Device Template

    {% include tip.html content="You can create templates on the fly if the template hasn't already been created. This can be done via the **Create Template** hyperlink from the drop down menu" %}

    | Section                      | Field         | Sub Field                    | Value (Drop Down)        |
    |------------------------------|---------------|------------------------------|--------------------------|
    | Transport and Management VPN | Cisco VPN 0   |                              | cEdge_VPN0_dual_uplink   |
    | Transport and Management VPN | Cisco VPN 0   | Cisco VPN Interface Ethernet | cedge-vpn0-int-dual      |
    | Transport and Management VPN | Cisco VPN 512 |                              | cEdge_VPN512_dual_uplink |
    | Transport and Management VPN | Cisco VPN 512 | Cisco VPN Interface Ethernet | cedge-vpn512-int-dual    |

    ![](/images/Deploying_cEdge40/46_vpn0_vpn512_feattempsel.PNG)

4. Once created, the Device Template will need to be attached to a Device for it to take effect. Click on the three dots (right-hand side) and click on **Attach Devices**

    ![](/images/Deploying_cEdge40/47_create_dots_attach.PNG)

5. We will be presented with a list of devices that can be associated with this template. Choose any device, making note of the Name (e.g. the device with a name ending in **73F2** has been selected over here). Click on **Attach**

    ![](/images/Deploying_cEdge40/48_chooseavaildev_remembername.PNG)

6. This should take you to a page which shows the attached device. Click on the three dots (right-hand side) and click on **Edit Device Template**. Also, make note of the cross mark next to the device name, on the left-hand side. This is the point where we need to enter details for the device specific values populated in the Feature Templates.

    ![](/images/Deploying_cEdge40/49_editdevtempl.PNG)

7. Enter details as per the screenshot below (these can be found in the table referenced at the beginning of the page) and click on **Update**

    ![](/images/Deploying_cEdge40/50_enterdevdet.PNG)

8. Click on the entry in the Device List to view the configuration that will be pushed to the device. Notice that the vBond IP and the Organization Name have been populated. These are taken from the vManage **Administration -> Settings** page, where they need to be populated. Click on **Configure** to configure the device.

    ![](/images/Deploying_cEdge40/51_configprev_ou_vb_vman.PNG)

Since this isn't a device that exists (as of now), the configuration push is scheduled for later, when a device is associated with this Device Name (the one ending in 73F2). This is done in the next section


<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
~~- Creating the cEdge40 VM~~
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Initial Configuration - non SD-WAN mode~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Setting up Feature Templates~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- ~~Creating and Attaching Device Templates~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Copying the Bootstrap file and converting to SD-WAN IOS-XE mode
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

### Copying the Bootstrap file and converting to SD-WAN IOS-XE mode

We will be generating a Bootstrap file and placing it in the flash of the device we want to bring up. The device (cEdge40) should come up and establish control connections with vManage, along with establishing BFD sessions with other devices.

{% include note.html content="While we are placing the Bootstrap file in flash for the lab, this can be put on a USB drive and plugged into the cEdge. This is usually done at a staging facility, post which the device is shipped to the customer site. Once they plug it in and power it on, the bootstrap configuration file allows the device to come up and establish control connections" %}

1. Go to **Configuration -> Devices**

    ![](/images/Deploying_cEdge40/52_config_dev_edit.PNG)

2. Identify the **Chassis Number** that was selected before, while attaching a Device to the Template. In this case, it ended in **73F2**. Click on the three dots in the right-hand side and click on **Generate Bootstrap Configuration**. Choose **Cloud-Init** and click on OK.

    ![](/images/Deploying_cEdge40/53_genbootstrap.PNG)

    ![](/images/Deploying_cEdge40/54_cloudinit.PNG)

3. Download the bootstrap file (will get saved to the Downloads folder by default). It should be a file beginning with CSR...

    ![](/images/Deploying_cEdge40/55_download.PNG)

4. Rename this to *ciscosdwan_cloud_init.cfg*. Note that the name should match exactly as is enumerated here, else Bootstrapping will not work. If a file already exits with the same name, choose to overwrite.

    ![](/images/Deploying_cEdge40/56_rename.PNG)

    {% include tip.html content="On bootup, a cEdge looks for a file on it's USB port (if a bootable USB drive is connected) and in bootflash:. The file name must match as above for Cloud type devices (i.e. CSR1K). For physical devices, the file name should be *ciscosdwan.cfg*. If the file is present on the USB drive and in bootflash:, the one in bootflash: takes precedence" %}

5. From the Jumphost Desktop, start TFTPD64. Click on Browse and choose the Downloads folder (or wherever the renamed .cfg file has been stored)

    ![](/images/Deploying_cEdge40/57_starttftpd32_pointtodnld.PNG)

6. Choose the 192.168.0.X IP from the Server Interfaces drop down

    ![](/images/Deploying_cEdge40/58_setiptojump.PNG)

7. Log in to the CLI of cEdge40 (we can log in via Putty now, using the saved session or by SSH'ing to 192.168.0.40) and issue `copy tftp: bootflash:`. Specify a Remote Host IP of your Jumphost (192.168.0.121 in this case). The source and destination file name should be *ciscosdwan_cloud_init.cfg*. The file should get copied over to bootflash: successfully

    ![](/images/Deploying_cEdge40/59_copytoflash.PNG)

    ```
    copy tftp: bootflash:
    ```
8. Log in to the CLI of the vManage (again, via the saved Putty session or by SSH'ing to 192.168.0.6) and issue the following commands to SCP the ROOTCA.pem file over to cEdge40

    ![](/images/Deploying_cEdge40/61_scppemfiletocedge.PNG)
    ```
    vshell
    scp ROOTCA.pem admin@192.168.0.40:ROOTCA.pem
    yes
    admin
    ```
    The last **admin** over there is the password of cEdge40

9. Go back to the CLI of cEdge40 and issue `controller-mode enable` from privilege mode. **Confirm** and this should lead to the device rebooting

    ![](/images/Deploying_cEdge40/62_controlmode.PNG)
    ```
    controller-mode enable

    ```

We have completed this section of the lab and will now need to wait for the cEdge to reboot. On rebooting, it should pick up the configuratio file from bootflash: and connect to the vManage/vSmarts/other vEdges. This will be verified in the next section.

<br>

{% include callout.html content="**Task List**
<br/><br/>
~~- Verifying the current lab setup~~
<br/>
~~- Creating the cEdge40 VM~~
<br/>
- Onboarding cEdge40
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Initial Configuration - non SD-WAN mode~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;~~- Setting up Feature Templates~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- ~~Creating and Attaching Device Templates~~
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- ~~Copying the Bootstrap file and converting to SD-WAN IOS-XE mode~~
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}
