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

<br>

{% include callout.html content="**Task List**
<br/><br/>
- Verifying the existing lab setup
<br/>
- Creating the DC-vEdge1 VM
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Overview
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Deploying the VM on vCenter
    <br/>
- Onboarding DC-vEdge1
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Bootstrapping DC-vEdge1 (Initial Configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Installing certificates and activating the vEdge
    <br/>
- Onboarding Verification
<br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Verifying the existing lab setup

The vManage, vBond and vSmarts have been deployed and should be ready to accept control connections from vEdges and cEdges. We will start by verifying the existing setup.

1. Log in to vManage by clicking on the bookmark or navigating to https://192.168.0.6. Use the following credentials:

    Username | Password
    ------------ | -------------
    admin | admin

    ![](/images/Deploying_DC_vEdge1/01_logintovmanage.PNG)

2. On logging in, you should see 2 vSmarts, 1 vBond and 1 vManage along the top row and 2 control planes should be up (naming convention might vary)

    ![](/images/Deploying_DC_vEdge1/02_2smarts_1bond_vm.PNG)
3. Open and log in to the vManage via the CLI - fire up Putty and double click the saved session for vManage or SSH to 192.168.0.6. Use the same credentials as the GUI.

    ![](/images/Deploying_DC_vEdge1/03_cliver.PNG)
4. Issue `show control connections` and you should see the vManage talking to the vSmarts and the vBond.

    ![](/images/Deploying_DC_vEdge1/03_controlconnver.PNG)
5. Additionally, you can log in to the CLI for the vBond via the saved link (and the same password as the vManage) and issue a `show orchestrator connections`

    ![](/images/Deploying_DC_vEdge1/99_orcconnver.PNG)

We see that the connections are up and this completes the verification activity.
<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [Creating the DC-vEdge1 VM](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Overview](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Deploying the VM on vCenter](#deploying-the-vm-on-vcenter)
    <br/>
- [Onboarding DC-vEdge1](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping DC-vEdge1 (Initial Configuration)](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

## Creating the DC-vEdge1 VM

### Overview

We will be deploying a vEdge in our first site (the Data Center) via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later. X is your POD number and is used only during vCenter related activities.

{% include note.html content="We will be using vEdge Clouds and CSR1000v images which are software images and don't support the standard Day 0 onboarding options. They do have a tweaked version of the Day 0 onboarding which we will be implementing later on in the lab. Hardware devices can do Day 0 onboarding (Zero Touch Provisioning)" %}

VM Name | System IP | Network Adapter | Network | Interface | IP Address | Default Gateway
------- | | ------------- | ------------- | ------------- | ------------- | -------------
DC-vEdge1-podX | 10.255.255.11 | Network Adapter 1 | Management | eth0 | 192.168.0.10/24 | 192.168.0.1
 || Network Adapter 2 | MPLS10 | ge0/1 | 192.0.2.2/30 | 192.0.2.1
 || Network Adapter 3 | SiteDC_VPN10 | ge0/2 | 10.100.10.2/24 | 10.100.10.1
 || Network Adapter 4 | SiteDC-VPN20 | ge0/3 | 10.100.20.2/24 | 10.100.20.1
 || Network Adapter 5 | Internet | ge0/0 | 100.100.100.10/24 | 100.100.100.1

{% include tip.html content="Plan your sites and addressing carefully. Proper planning can prevent a number of issues and will help with a successful, early deployment." %}

{% include tip.html content="There is configuration applicable only to virtual vEdges/cEdges in some of the sections. Physical vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options." %}

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [Creating the DC-vEdge1 VM](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Deploying the VM on vCenter](#deploying-the-vm-on-vcenter)
    <br/>
- [Onboarding DC-vEdge1](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping DC-vEdge1 (Initial Configuration)](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

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
5. Change the Virtual Machine name to **DC-vEdge1-podX** and click on Next (where X is your POD number, image below doesn't have the suffix of podX)

    {% include note.html content="We will only use the podX suffix over here to distinguish between different VMs in our Data Center. The rest of the guide will refer to this VM as **DC-vEdge1**" %}

    ![](/images/Deploying_DC_vEdge1/07_namedcvedge1_next.PNG)
6. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
7. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
8. Choose the Datastore and click on Next

    ![](/images/Deploying_DC_vEdge1/10_storage_next.PNG)
9. Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_DC_vEdge1/11_populatevmnetworks_referenceipschema.PNG)
10. Click on **Finish** to deploy your DC-vEdge1 VM

    ![](/images/Deploying_DC_vEdge1/12_finish.PNG)
11. Once the VM is deployed, right click **DC-vEdge1-podX** and click Edit settings.

    ![](/images/Deploying_DC_vEdge1/13_rightclickdcvedge1_editsettings.png)
12. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 5 for our lab)

    ![](/images/Deploying_DC_vEdge1/14_addnewdev_netadapt.PNG)
13. Click on the drop down next to **New Network** and click on *Browse*

    ![](/images/Deploying_DC_vEdge1/15_dropdown_browse.png)
14. Choose the **Internet** Network and click on OK. Make sure the Network Adapters match with the second image below and click on OK again
    {% include warning.html content="The Network Adapter mapping might vary based on the version of vEdge being deployed. Sometimes, trial and error is the easiest way to figure out which Network Adapter maps to which interface on the vEdge" %}

    ![](/images/Deploying_DC_vEdge1/16_chooseinternet_ok_ok.PNG)

    ![](/images/Deploying_DC_vEdge1/17_NetworkAdaptersdcvedge1.PNG)
15. Click on the DC-vEdge1-podX VM and choose to power it on

    ![](/images/Deploying_DC_vEdge1/18_choosedcvedge_poweron.png)

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [~~Creating the DC-vEdge1 VM~~](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the VM on vCenter~~](#deploying-the-vm-on-vcenter)
    <br/>
- [Onboarding DC-vEdge1](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping DC-vEdge1 (Initial Configuration)](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

## Onboarding DC-vEdge1

### Bootstrapping DC-vEdge1 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM IP     | VM        | Network Adapter   | Network      | Interface | IP                | Gateway       |
|---------|---------------|-----------|-------------------|--------------|-----------|-------------------|---------------|
| 1       | 10.255.255.11 | DC-vEdge1 | Network Adapter 1 | Management   | eth0      | 192.168.0.10/24   | 192.168.0.1   |
|         |               |           | Network Adapter 2 | MPLS10       | ge0/1     | 192.0.2.2/30      | 192.0.2.1     |
|         |               |           | Network Adapter 3 | SiteDC_VPN10 | ge0/2     | 10.100.10.2/24    | 10.100.10.1   |
|         |               |           | Network Adapter 4 | SiteDC-VPN20 | ge0/3     | 10.100.20.2/24    | 10.100.20.1   |
|         |               |           | Network Adapter 5 | Internet     | ge0/0     | 100.100.100.10/24 | 100.100.100.1 |


1. Console in to the DC-vEdge1 VM from vCenter (you should already be logged in from our last activity)

    ![](/images/Deploying_DC_vEdge1/19_choosetoopenconsolefordcvedge1.png)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in)

    | Username | Password     |
    | ------------- | ------------- |
    | admin      | admin       |

    {% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

    ![](/images/Deploying_DC_vEdge1/20_login_adminadminresettoadmin.PNG)

3. Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_DC_vEdge1/21_Enterbootstrap_typeincons.PNG)

    ```
    conf t
    system
     host-name DC-vEdge1
     system-ip 10.255.255.11
     site-id 1
     organization-name "swat-sdwanlab"
     vbond 100.100.100.3
     exit
    !
    vpn 0
     ip route 0.0.0.0/0 100.100.100.1
     interface ge0/0
      ip address 100.100.100.10/24
      no tunnel-interface
      no shutdown
      exit
      !
      exit
    !
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.10/24
      no shutdown
    !
    commit and-quit
```
    {% include tip.html content="We are ensuring that the vEdge has basic IP Addressing and Routing to the Controllers. `no tunnel-interface` has been added under the ge0/0 interface in VPN 0 in order to prevent control connections from being established" %}

4. Open **Putty** and double click the saved session for DC-vEdge1 (or **SSH** to **192.168.0.10**)

    ![](/images/Deploying_DC_vEdge1/22_openputty_dcvedge1doubleclick.PNG)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Login using the same credentials as Step 2.

    ![](/images/Deploying_DC_vEdge1/24_loginusingadminadmin.PNG)

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [~~Creating the DC-vEdge1 VM~~](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the VM on vCenter~~](#deploying-the-vm-on-vcenter)
    <br/>
- [Onboarding DC-vEdge1](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping DC-vEdge1 (Initial Configuration)~~](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

### Installing certificates and activating the vEdge

{% include tip.html content="Make sure the certificates and relevant files (like the .viptela Serial file) are in order before initiating a deployment. Certificate mismatches are one of the most widely seen causes for devices not being able to establish control connections with the vManage/vSmarts" %}

1. Type `vshell` and enter `scp admin@192.168.0.6:ROOTCA.pem .` to copy the ROOTCA.pem certificate to the vEdge. Commands can be copy-pasted now since we have SSH'd in to the vEdge (there is a dot at the end of the scp command). Enter `yes` when prompted and enter the password of vManage (i.e. admin). Exit when done with this step.

    ![](/images/Deploying_DC_vEdge1/25_vshellcopypemtovedgefromvman.PNG)
    ```
    vshell
    scp admin@192.168.0.6:ROOTCA.pem .
    ```
    {% include note.html content="This is NOT how you would normally install certificates over to your devices. In a lab, this manual method works fine but for production environments, the other options are definitely preferred (like Cisco PKI)" %}
2. Go to the vManage GUI (https://192.168.0.6) and log in, if logged out. Navigate to **Configuration -> Devices** (from the left-hand side, click on the cog wheel to access the configuration options)

    ![](/images/Deploying_DC_vEdge1/26_config_devices.png)

3. Choose any vEdge Cloud device (it doesn't matter which one you pick, as long as it is a vEdge Cloud) and click on the three dots at the extreme right-hand side. Choose **Generate Bootstrap Configuration**

    ![](/images/Deploying_DC_vEdge1/27_threedots_genbootstrap.png)

4. Select **Cloud-Init** and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the DC-vEdge1 device. Alternatively, leave this popup open and we can come back to it when required
    {% include important.html content="The UUID and OTP/Token are super important for vEdge Cloud or cEdge CSRs. Physical devices don't have a token associated with them and are uniquely identified by their serial number" %}

    ![](/images/Deploying_DC_vEdge1/29_makenote_uuid_otp.PNG)

6. Go back to the Putty session for DC-vEdge1 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully

    ![](/images/Deploying_DC_vEdge1/30_installrootcertchain.PNG)
    ```
    request root-cert-chain install /home/admin/ROOTCA.pem
    ```
7. Enter `tunnel-interface`, `encapsulation ipsec` and `allow-service all` under `interface ge0/0` to bring up the tunnel Interface. Make sure to `commit and-quit` in order to write the configuration change

    ![](/images/Deploying_DC_vEdge1/31_enable_tunnint.PNG)
    ```
    config t
    vpn 0
    interface ge0/0
     tunnel-interface
     encapsulation ipsec
     allow-service all
     exit
     !
     commit and-quit
     ```
    This ensures that our vEdge is now able to establish control connections with the vManage and vSmarts via the vBond. However, these connections will not be fully formed till we don't activate the vEdge itself

8. Issue the `request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)`command. Replace the *(Enter your UUID)* and *(Enter your OTP)* fields with the UUID and OTP generated in Step 5 (image below is an example, UUID and OTP may not match).

    ![](/images/Deploying_DC_vEdge1/32_activatevedge_uuid_token_diff.PNG)
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for DC-vEdge1


<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [~~Creating the DC-vEdge1 VM~~](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the VM on vCenter~~](#deploying-the-vm-on-vcenter)
    <br/>
- [~~Onboarding DC-vEdge1~~](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping DC-vEdge1 (Initial Configuration)~~](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

## Onboarding Verification

1. Wait for a couple of minutes and run `show control connections` in the DC-vEdge1 CLI. We should see that the vEdge has been able to establish a DTLS tunnel with the vManage and the vSmarts. If you don't see any output, wait for a couple of minutes and run the command again

    ![](/images/Deploying_DC_vEdge1/33_showcontrolconn.PNG)
    ```
    show control connections
    ```
    {% include tip.html content="You can also issue `show control connections-history` in the event of failures to find out why is the connection not working as expected. A few helpful commands are `show certificate installed`, `show certificate root-ca-cert`, `show control local-properties` and `show certificate validity`. Most of these commands give us details about the status of certificates on the device and are helpful in ascertaining the root cause of failure when control connections aren't getting established." %}

2. On the vManage GUI, navigate to **Monitor -> Network Devices** (the computer icon on the left-hand side)

    ![](/images/Deploying_DC_vEdge1/34_monitor_network.png)

3. DC-vEdge1 should show up in the list of devices

    ![](/images/Deploying_DC_vEdge1/35_DC-vEdge1added.PNG)

4. Click on DC-vEdge1 and navigate to **Troubleshooting -> Control Connections(Live view)**. You should see the vEdge successfully connected to 2 vSmarts and 1 vManage

    ![](/images/Deploying_DC_vEdge1/36_MonNet_dcvedge1_tshoot_controlconn.PNG)


This completes the verification activity.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [~~Creating the DC-vEdge1 VM~~](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the VM on vCenter~~](#deploying-the-vm-on-vcenter)
    <br/>
- [~~Onboarding DC-vEdge1~~](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping DC-vEdge1 (Initial Configuration)~~](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>
- [~~Onboarding Verification~~](#onboarding-verification)
<br/>
- [Helpful debugs and logs](#helpful-debugs-and-logs)
<br/>

" type="primary" %}

## Helpful debugs and logs

{% include note.html content="This section is optional and is intended as a learning activity. It is not required to go through this in order to complete the lab tasks successfully" %}

1. On the CLI for DC-vEdge1, issue `debug vdaemon all` followed by `clear control connections`. This will tear down all the control connections and the vEdge will rebuilt the DTLS tunnels. We can capture the logs to see the process associated with the DTLS tunnels being built

    ![](/images/Deploying_DC_vEdge1/37_enabledebug_clearcontr.PNG)
    ```
    debug vdaemon all
    clear control connections
    ```

2. Wait for a couple of minutes and go to `vshell`. Type `cat /var/log/tmplog/vdebug` to view the contents of the log file

    ![](/images/Deploying_DC_vEdge1/38_accessvsh_deboutput.PNG)

3. Given below are a couple of sample outputs

    ![](/images/Deploying_DC_vEdge1/39_sampleoutput.PNG)

    ![](/images/Deploying_DC_vEdge1/40_sampleoutput2.PNG)    

    Use `no debug vdaemon all` to disable the debug

This completes our onboarding activity for DC-vEdge1.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Verifying the existing lab setup~~](#verifying-the-existing-lab-setup)
<br/>
- [~~Creating the DC-vEdge1 VM~~](creating-the-dc-vedge1-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the VM on vCenter~~](#deploying-the-vm-on-vcenter)
    <br/>
- [~~Onboarding DC-vEdge1~~](#onboarding-dc-vedge1)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping DC-vEdge1 (Initial Configuration)~~](#bootstrapping-dc-vedge1-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>
- [~~Onboarding Verification~~](#onboarding-verification)
<br/>
- [~~Helpful debugs and logs~~](#helpful-debugs-and-logs)
<br/>

" type="primary" %}
