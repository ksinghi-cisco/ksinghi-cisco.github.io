---
title: Deploying and Onboarding DC-vEdge1
tags: [getting_started]
keywords: DC-vEdge1, onboarding, deploying, vEdge1
last_updated: May 15, 2020
summary: "Step by step process for deploying DC-vEdge1"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_DCvEdge1.html
folder: mydoc
---

Task List

- [ ] Verifying the existing lab setup
- [ ] Creating the DC-vEdge1 VM
- [ ] Onboarding DC-vEdge1
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs


## Verifying the existing lab setup

The vManage, vBond and vSmarts have been deployed and should be ready to accept control connections from vEdges and cEdges. We will start by verifying the existing setup.

1. Log in to vManage by clicking on the bookmark or navigating to https://192.168.0.6. Use the following credentials:

    Username | Password
    ------------ | -------------
    admin | admin

    ![](/images/Deploying_DC_vEdge1/01_logintovmanage.PNG)

2. On logging in, you should see 2 vSmarts, 1 vBond and 1 vManage along the top row and 2 control planes should be up

    ![](/images/Deploying_DC_vEdge1/02_2smarts_1bond_vm.PNG)
3. Open and log in to the vManage via the CLI - fire up Putty and double click the saved session for vManage or SSH to 192.168.0.6. Use the same credentials as the GUI.

    ![](/images/Deploying_DC_vEdge1/03_cliver.PNG)
4. Issue `show control connections` and you should see the vManage talking to the vSmarts and the vBond.

    ![](/images/Deploying_DC_vEdge1/03_controlconnver.PNG)
5. Additionally, you can log in to the CLI for the vBond via the saved link (and the same password as the vManage) and issue a `show orchestrator connections`

    ![](/images/Deploying_DC_vEdge1/99_orcconnver.PNG)

We see that the connections are up and this completes the verification activity.
<br>

Task List

- [x] Verifying the existing lab setup
- [ ] Creating the DC-vEdge1 VM
- [ ] Onboarding DC-vEdge1
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Creating the DC-vEdge1 VM

### Overview

We will be deploying a vEdge in our first site (the Data Center) via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

VM Name | Network Adapter | Network | Interface | IP Address | Default Gateway
------- | ------------- | ------------- | ------------- | ------------- | -------------
DC-vEdge1 | Network Adapter 1 | Management | eth0 | 192.168.0.10/24 | 192.168.0.1
 | Network Adapter 2 | MPLS10 | ge0/1 | 192.0.2.2/30 | 192.0.2.1
 | Network Adapter 3 | SiteDC-VPN10 | ge0/2 | 10.10.10.2/24 | 10.10.10.1
 | Network Adapter 4 | SiteDC-VPN20 | ge0/3 | 10.10.20.2/24 | 10.10.20.1
 | Network Adapter 5 | Internet | ge0/0 | 100.100.100.10/24 | 100.100.100.1

> *Plan your sites and addressing carefully. Proper planning can prevent a number of issues and will help with a successful, early deployment.*

> *This configuration is applicable only for virtual vEdges/cEdges. Physical vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options.*

### Deploying the VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

    ![](/images/Deploying_DC_vEdge1/03_logintovcenter_usecredsprovided.PNG)
2. Notice that no vEdges/cEdges have been deployed on the host you've accessed. This is expected.

    ![](/images/Deploying_DC_vEdge1/04_nositesdeployed_onlyctrl.PNG)
3. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
4. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

    ![](/images/Deploying_DC_vEdge1/06_chooselocalfile_vedgeimagefromfolder.PNG)
5. Change the Virtual Machine name to **DC-vEdge1** and click on Next.

    ![](/images/Deploying_DC_vEdge1/07_namedcvedge1_next.PNG)
6. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
7. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
8. Choose the Datastore and click on Next

    ![](/images/Deploying_DC_vEdge1/10_storage_next.PNG)
9. Populate the VM Networks as per the table given at the start of this section (or reference the image below)

    ![](/images/Deploying_DC_vEdge1/11_populatevmnetworks_referenceipschema.PNG)
10. Click on **Finish** to deploy your DC-vEdge1 VM

    ![](/images/Deploying_DC_vEdge1/12_finish.PNG)
11. Once the VM is deployed, right click **DC-vEdge1** and click Edit settings.

    ![](/images/Deploying_DC_vEdge1/13_rightclickdcvedge1_editsettings.png)
12. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 5 for our lab)

    ![](/images/Deploying_DC_vEdge1/14_addnewdev_netadapt.PNG)
13. Click on the drop down next to **New Network** and click on *Browse*

    ![](/images/Deploying_DC_vEdge1/15_dropdown_browse.png)
14. Choose the **Internet** Network and click on OK. Make sure the Network Adapters match with the second image below and click on OK again

    ![](/images/Deploying_DC_vEdge1/16_chooseinternet_ok_ok.PNG)

    ![](/images/Deploying_DC_vEdge1/17_NetworkAdaptersdcvedge1.PNG)
15. Click on DC-vEdge1 and choose to power it on

    ![](/images/Deploying_DC_vEdge1/18_choosedcvedge_poweron.png)

<br>


Task List

- [x] Verifying the existing lab setup
- [x] Creating the DC-vEdge1 VM
- [ ] Onboarding DC-vEdge1
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Onboarding DC-vEdge1

You can use include templates for notes, tips, and warnings. These include templates make it easier to insert notes. If you make an error, you're immediately made aware since the site won't build. See [Alerts][mydoc_alerts] for more details.

<br>

Task List

- [x] Verifying the existing lab setup
- [x] Creating the DC-vEdge1 VM
- [x] Onboarding DC-vEdge1
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Onboarding Verification

Similar to alerts, images also have include templates. You can insert both regular images and inline images, such as images that are a button or icon. See [Images][mydoc_images] for more details.

<br>

Task List

- [x] Verifying the existing lab setup
- [x] Creating the DC-vEdge1 VM
- [x] Onboarding DC-vEdge1
- [x] Onboarding Verification
- [ ] Helpful debugs and logs

## Helpful debugs and logs

Instead of using YAML references to handle links, I've switched to a Markdown reference style approach. A links.html file iterates through the sidebar files and formats the content in the Markdown reference. You then just use Markdown syntax for the links. See [Links][mydoc_hyperlinks] for more details.

<br>
Task List
<br>
<br>

- [x] Verifying the existing lab setup
- [x] Creating the DC-vEdge1 VM
- [x] Onboarding DC-vEdge1
- [x] Onboarding Verification
- [x] Helpful debugs and logs
