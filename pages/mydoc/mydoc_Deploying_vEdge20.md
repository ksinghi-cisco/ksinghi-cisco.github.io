---
title: Deploying a Single uplink (INET) vEdge
tags: [getting_started, troubleshooting]
keywords: vEdge 20, Site 20, Single uplink vedge
summary: "Deploying vEdge20 in Site 20. This vEdge has a single uplink to the Internet"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_vEdge20.html
folder: mydoc
---

<br>

{% include callout.html content="**Task List**
<br/><br/>
- Creating the vEdge20 VM
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Overview
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Deploying the vEdge20 VM on vCenter
    <br/>
- Onboarding vEdge20
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Bootstrapping vEdge20 (Initial Configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Installing certificates and activating the vEdge
    <br/>
- Onboarding Verification
<br/>

" type="primary" %}

## Creating the vEdge20 VM

### Overview

{% include note.html content="There will be a number of repetitive tasks from the Deploying DC-vEdge1/DC-vEdge2 section." %}

{% include note.html content="The important steps which will guide you through this activity will be earmarked, indicating a delta from the previous section." %}

> This is what an earmarked step will look like

We will be deploying a vEdge at Site 20 via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | IP                | Gateway       |
|---------|---------------|---------|-------------------|----------------|-----------|-------------------|---------------|
| 20      | 10.255.255.21 | vEdge20-podX | Network Adapter 1 | Management     | eth0      | 192.168.0.20/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 192.168.25.20/24  |               |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 10.20.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 10.20.20.2/24     |               |
|         |               |         | Network Adapter 5 | Internet       | ge0/0     | 100.100.100.20/24 | 100.100.100.1 |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 192.168.26.20/24  |               |

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [Creating the vEdge20 VM](creating-the-vedge20-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Deploying the vEdge20 VM on vCenter](#deploying-the-vedge20-vm-on-vcenter)
    <br/>
- [Onboarding vEdge20](#onboarding-vedge20)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge20 (Initial Configuration)](#bootstrapping-vedge20-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>

" type="primary" %}

### Deploying the vEdge20 VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. > Change the Virtual Machine name to **vEdge20-podX** and click on Next (where X is your POD number, image below does not have the suffix of podX)

    {% include note.html content="We will only use the podX suffix over here to distinguish between different VMs in our Data Center. The rest of the guide will refer to this VM as **vEdge20**" %}

    ![](/images/Deploying_vEdge20/01_NamevEdge20.PNG)

5. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. >Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_vEdge20/02_nwadapters.PNG)
9. Click on **Finish** to deploy your vEdge20-podX VM

    ![](/images/Deploying_vEdge20/03_summary.PNG)

10. Once the VM is deployed, right click **vEdge20-podX** and click Edit settings.

    ![](/images/Deploying_vEdge20/04_edtisett.PNG)

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 6 for our lab). Add another network device this way, for a total of 6 network adapters.

12. Click on the drop down next to the first **New Network** and click on *Browse*

    ![](/images/Deploying_vEdge20/05_browsenetint.PNG)

13. Choose the **Internet** Network and click on OK.

    ![](/images/Deploying_vEdge20/06_chooseInt.PNG)

14. Click on the drop down for the second **New Network** entry, added as a result of adding two network adapters and click on *Browse*. Select the **TLOCEXT2_vEdge** Network

    ![](/images/Deploying_vEdge20/07_browseagain_tlocext2.PNG)

15.  Make sure the Network Adapters match with the image below and click on OK
    ![](/images/Deploying_vEdge20/99_nwad2.PNG)

16. Click on vEdge20-podX and choose to power it on

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge20 VM~~](creating-the-vedge20-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge20 VM on vCenter~~](#deploying-the-vedge20-vm-on-vcenter)
    <br/>
- [Onboarding vEdge20](#onboarding-vedge20)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge20 (Initial Configuration)](#bootstrapping-vedge20-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>

" type="primary" %}

## Onboarding vEdge20

### Bootstrapping vEdge20 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | IP                | Gateway       |
|---------|---------------|---------|-------------------|----------------|-----------|-------------------|---------------|
| 20      | 10.255.255.21 | vEdge20 | Network Adapter 1 | Management     | eth0      | 192.168.0.20/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 192.168.25.20/24  |               |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 10.20.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 10.20.20.2/24     |               |
|         |               |         | Network Adapter 5 | Internet       | ge0/0     | 100.100.100.20/24 | 100.100.100.1 |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 192.168.26.20/24  |               |

1. Console in to the vEdge20 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in).

    | Username | Password     |
    | ------------- | ------------ |
    | admin     | admin       |

    {% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

3. >Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_vEdge20/08_bootstrap.PNG)
    ```
    conf t
    system
     host-name vEdge20
     system-ip 10.255.255.21
     site-id 20
     organization-name "swat-sdwanlab"
     vbond 100.100.100.3
     exit
    !
    vpn 0
     ip route 0.0.0.0/0 100.100.100.1
     interface ge0/0
      ip address 100.100.100.20/24
      no tunnel-interface
      no shutdown
      exit
     !
     exit
    !
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.20/24
      no shutdown
    !
    commit and-quit
    ```  
4. Open **Putty** and double-click the saved session for vEdge20 (or **SSH** to **192.168.0.20**)

    ![](/images/Deploying_vEdge20/09_Putty.PNG)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Log in using the same credentials as Step 2.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge20 VM~~](creating-the-vedge20-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge20 VM on vCenter~~](#deploying-the-vedge20-vm-on-vcenter)
    <br/>
- [Onboarding vEdge20](#onboarding-vedge20)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge20 (Initial Configuration)~~](#bootstrapping-vedge20-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>

" type="primary" %}

### Installing certificates and activating the vEdge

1. Type `vshell` and  enter `scp admin@192.168.0.6:ROOTCA.pem .` to copy the ROOTCA.pem certificate to the vEdge. Commands can be copy-pasted now since we have SSH'd in to the vEdge (there is a dot at the end of the scp command). Enter `yes` when prompted and enter the password of vManage (i.e. admin). Exit when done with this step.
    ```
    vshell
    scp admin@192.168.0.6:ROOTCA.pem .
    ```
2. Go to the vManage GUI (https://192.168.0.6) and log in, if logged out. Navigate to **Configuration -> Devices** (from the left-hand side, click on the cog wheel to access the configuration options)

    ![](/images/Deploying_DC_vEdge1/26_config_devices.png)

3. >Choose any vEdge Cloud device (it doesn't matter which one you pick, as long as it is a vEdge Cloud) and click on the three dots at the extreme right-hand side. Choose to Generate Bootstrap Configuration

    ![](/images/Deploying_vEdge20/10_genbootstrap.PNG)

4. >Select Cloud-Init and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. >Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the vEdge20 device. Alternatively, leave this popup open and we can come back to it when required

    ![](/images/Deploying_vEdge20/11_uuidotp.PNG)

6. Go back to the Putty session for vEdge20 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully

    ![](/images/Deploying_vEdge20/12_rootcert.PNG)
    ```
    request root-cert-chain install /home/admin/ROOTCA.pem
    ```
7. Enter  `tunnel-interface`, `encapsulation ipsec` and `allow-service all` under `interface ge0/0` to bring up the tunnel Interface. Make sure to `commit and-quit` in order to write the configuration change
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

    ![](/images/Deploying_vEdge20/13_enableTunn.PNG)

8. Issue the `request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)`command. Replace the *(Enter your UUID)* and *(Enter your OTP)* fields with the UUID and OTP generated in Step 5 (image below is an example, UUID and OTP may not match).

    ![](/images/Deploying_vEdge20/14_actvedge.PNG)
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for vEdge20


<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge20 VM~~](creating-the-vedge20-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge20 VM on vCenter~~](#deploying-the-vedge20-vm-on-vcenter)
    <br/>
- [~~Onboarding vEdge20~~](#onboarding-vedge20)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge20 (Initial Configuration)~~](#bootstrapping-vedge20-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>
- [Onboarding Verification](#onboarding-verification)
<br/>

" type="primary" %}

## Onboarding Verification

1. Wait for a couple of minutes and run `show control connections` in the vEdge20 CLI. We should see that the vEdge has been able to establish a DTLS tunnel with the vManage and the vSmarts. If you don't see any output, wait for a couple of minutes and run the command again

    ![](/images/Deploying_vEdge20/15_shcontconn.PNG)

    ```
    show control connections
    ```
    {% include tip.html content="You can also issue `show control connections-history` in the event of failures to find out why is the connection not working as expected. A few helpful commands are `show certificate installed`, `show certificate root-ca-cert`, `show control local-properties` and `show certificate validity`. Most of these commands give us details about the status of certificates on the device and are helpful in ascertaining the root cause of failure when control connections aren't getting established." %}

2. On the vManage GUI, navigate to **Monitor -> Network Devices** (the computer icon on the left-hand side)

    ![](/images/Deploying_DC_vEdge1/34_monitor_network.png)

3. vEdge20 should show up in the list of devices

    ![](/images/Deploying_vEdge20/16_vedge20inlist.PNG)

4. Click on vEdge20 and navigate to **Troubleshooting -> Control Connections(Live view)**. You should see the vEdge successfully connected to 2 vSmarts and 1 vManage

    ![](/images/Deploying_vEdge20/17_vedge20controlgui.PNG)

5. On the main dashboard, notice that we now have three WAN Edges onboarded on DNAC. The two sites also have WAN connectivity since BFD sessions have been established.

    ![](/images/Deploying_vEdge20/98_dash.PNG)

6. This can be verified from the **Monitor -> Network** page as well, where we will see active BFD sessions on all devices. Via CLI, this can be checked using `show bfd sessions`

    ![](/images/Deploying_vEdge20/97_mon.PNG)

    ![](/images/Deploying_vEdge20/96_bfd.PNG)

{% include tip.html content="The vEdges at DC show only one BFD session whereas vEdge20 shows two. THis is because the DC-vEdges detect that they are part of the same site (via the `site-id` command), hence won't have BFD entries for each other" %}

This completes the verification activity.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge20 VM~~](creating-the-vedge20-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge20 VM on vCenter~~](#deploying-the-vedge20-vm-on-vcenter)
    <br/>
- [~~Onboarding vEdge20~~](#onboarding-vedge20)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge20 (Initial Configuration)~~](#bootstrapping-vedge20-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>
- [~~Onboarding Verification~~](#onboarding-verification)
<br/>

" type="primary" %}
