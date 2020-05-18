---
title: Deploying a Dual Uplink cEdge
tags: [getting_started, formatting, content_types]
keywords: cEdges, cEdge40, Site40, Deploy
last_updated: July 16, 2016
summary: "Deploying Site 40 with a single cEdge which has both transport uplinks"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_cEdge40.html
folder: mydoc
---

Task List

- [ ] Verifying the existing lab setup
- [ ] Creating the cEdge40 VM
- [ ] Onboarding cEdge40
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs


## Verifying the existing lab setup

The vManage, vBond and vSmarts have been deployed along with Sites 1, 20 and 30. We will start by verifying the existing setup.

1. Log in to vManage by clicking on the bookmark or navigating to https://192.168.0.6. Use the following credentials:

    Username | Password
    ------------ | -------------
    admin | admin

    ![](/images/Deploying_DC_vEdge1/01_logintovmanage.PNG)

2. On logging in, you should see 2 vSmarts, 1 vBond and 1 vManage along with 5 WAN Edges. 7 control planes should be up and 3 sites should have WAN connectivity.

    ![](/images/Deploying_DC_vEdge1/02_2smarts_1bond_vm.PNG)
3. Open and log in to the vManage via the CLI - fire up Putty and double click the saved session for vManage or SSH to 192.168.0.6. Use the same credentials as the GUI.

    ![](/images/Deploying_DC_vEdge1/03_cliver.PNG)
4. Issue `show control connections` and you should see the vManage talking to the vSmarts, vBond and vEdges.

    ![](/images/Deploying_DC_vEdge1/03_controlconnver.PNG)

Look at the System IP to see which device has the vManage established a control connection with. There should be 5 connections to vEdges. We see that the connections are up and this completes the verification activity.
<br>

Task List

- [x] Verifying the existing lab setup
- [ ] Creating the DC-vEdge1 VM
- [ ] Onboarding DC-vEdge1
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Creating the vEdge40 VM

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

{% include tip.html content="There is configuration applicable only to virtual vEdges/cEdges in some of the sections. Physical vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options." %}

### Deploying the VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

    ![](/images/Deploying_DC_vEdge1/03_logintovcenter_usecredsprovided.PNG)
2. We should see the vEdges from previous sections of the lab deployed.

    ![](/images/Deploying_DC_vEdge1/04_nositesdeployed_onlyctrl.PNG)
3. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
4. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *csr100v-univer*. Click on Next.

    ![](/images/Deploying_DC_vEdge1/06_chooselocalfile_vedgeimagefromfolder.PNG)
5. Change the Virtual Machine name to **cEdge40** and click on Next.

    ![](/images/Deploying_DC_vEdge1/07_namedcvedge1_next.PNG)
6. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
7. Review the details shown and click on Next.  Select the **Large** option (4 vCPUs and 4 GB RAM) and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
8. Choose the Datastore and click on Next.

    ![](/images/Deploying_DC_vEdge1/10_storage_next.PNG)
9. Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_DC_vEdge1/11_populatevmnetworks_referenceipschema.PNG)
10. Click Next on **Customize Template** and then Click on **Finish** to deploy your DC-vEdge1 VM

    ![](/images/Deploying_DC_vEdge1/12_finish.PNG)
11. Once the VM is deployed, right click **cEdge40** and click Edit settings.

    ![](/images/Deploying_DC_vEdge1/13_rightclickdcvedge1_editsettings.png)
12. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 3 Network Adapters but we will need 6 for our lab). Do this twice more for a grand total of 6 Network Adapters

    ![](/images/Deploying_DC_vEdge1/14_addnewdev_netadapt.PNG)
13. Click on the drop down next to the first **New Network** and click on *Browse*

    ![](/images/Deploying_DC_vEdge1/15_dropdown_browse.png)
14. Choose the **Site40-VPN10** Network and click on OK. Do the same for the next two network adapters, allocating them to **Site40-VPN20** and **Site40-VPN30** respectively. Make sure the Network Adapters match with the second image below and click on OK again
    {% include warning.html content="The Network Adapter mapping might vary based on the version of vEdge being deployed. Sometimes, trial and error is the easiest way to figure out which Network Adapter maps to which interface on the vEdge" %}

    ![](/images/Deploying_DC_vEdge1/16_chooseinternet_ok_ok.PNG)

    ![](/images/Deploying_DC_vEdge1/17_NetworkAdaptersdcvedge1.PNG)
15. Click on cEdge40 and choose to power it on

<br>

Task List

- [x] Verifying the existing lab setup
- [x] Creating the cEdge40 VM
- [ ] Onboarding cEdge40
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

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

{% include tip.html content="Starting from IOS-XW 17.2, the cEdge platforms use a Universal image. One can switch from non SD-WAN mode to SD-WAN mode via a command" %}

1. We will first console in to the cEdge and set up an IP Address with basic routing to ensure that the cEdge can reach vManage and the Jumphost. This is done by issuing `ip route 0.0.0.0 0.0.0.0 192.168.0.1` followed by `interface GigabitEthernet1` and giving an IP Address to the interface through `ip address 192.168.0.40 255.255.255.0`. Make sure you `no shut` the interface.

    ```
    enable
    conf t
    interface GigabitEthernet1
    ip address 192.168.0.40 255.255.255.0
    no shut
    exit
    ip route 0.0.0.0 0.0.0.0 192.168.0.1
    ```

2. Verify connectivity to the vManage and the JumpHost (IP of the Jumphost might vary)

3. On vManage, navigate to **Configuration -> Templates**.
