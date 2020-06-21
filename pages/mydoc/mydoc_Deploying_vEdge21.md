---
title: Deploying a single uplink (MPLS) vEdge
tags: [getting_started, troubleshooting]
keywords: vEdge21, Site 20, Single uplink vEdge
summary: "Deploying vEdge21 in Site 20. This vEdge has a single uplink (MPLS)"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_vEdge21.html
folder: mydoc
---

{% include warning.html content="This section might already be done, depending on your selected Lab Scenario. Most lab work will start [from here](mydoc_Deploying_vEdge30.html). The configuration given below can be used to review what was done to bring the Site up. If this VM is already deployed (check via vCenter), you **Do Not** need to perform these lab activities." %}

<br>

{% include callout.html content="**Task List**
<br/><br/>
- Creating the vEdge21 VM
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Overview
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Deploying the vEdge21 VM on vCenter
    <br/>
- Onboarding vEdge21
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Bootstrapping vEdge21 (Initial Configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Installing certificates and activating the vEdge
    <br/>

" type="primary" %}

## Creating the vEdge21 VM

### Overview

{% include warning.html content="Since we have gone through deploying vEdges multiple times by now, this section will just have the steps listed out. Images for every step has not been populated due to similarity with the previous sections. " %}

{% include note.html content="The important steps which will guide you through this activity will be earmarked, indicating a delta from the previous sections." %}

> This is what an earmarked step will look like

We will be deploying another vEdge at Site 20 via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | IP               | Gateway     |
|---------|---------------|---------|-------------------|----------------|-----------|------------------|-------------|
| 20      | 10.255.255.22 | vEdge21-podX | Network Adapter 1 | Management     | eth0      | 192.168.0.21/24  | 192.168.0.1 |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 192.168.25.21/24 |             |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 10.20.10.3/24    |             |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 10.20.20.3/24    |             |
|         |               |         | Network Adapter 5 | MPLS20         | ge0/0     | 192.0.2.10/30    | 192.0.2.9   |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 192.168.26.21/24 |             |

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [Creating the vEdge21 VM](creating-the-vedge21-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Deploying the vEdge21 VM on vCenter](#deploying-the-vedge21-vm-on-vcenter)
    <br/>
- [Onboarding vEdge21](#onboarding-vedge21)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge21 (Initial Configuration)](#bootstrapping-vedge21-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}

### Deploying the vEdge21 VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. > Change the Virtual Machine name to **vEdge21-podX** and click on Next (where X is your POD number)

    {% include note.html content="We will only use the podX suffix over here to distinguish between different VMs in our Data Center. The rest of the guide will refer to this VM as **vEdge21**" %}

5. Select the host assigned to you (image shown as an example only) and click on Next

    {% include note.html content="If the screen gets stuck over here at **Validating** then close Chrome and open the vCenter in Internet Explorer, going through the same steps. Deployment should go through. This is a known issue with Google Chrome." %}

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. >Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_vEdge21/01_nwad.PNG)
9. Click on **Finish** to deploy your vEdge21-podX VM

    ![](/images/Deploying_vEdge21/02_summ.PNG)

10. Once the VM is deployed, right click **vEdge21-podX** and click Edit settings.

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 6 for our lab). Add another network device this way, for a total of 6 network adapters.

12. Click on the drop down next to the first **New Network** and click on *Browse*

13. > Choose the **MPLS20** Network and click on OK.

14. > Click on the drop down for the second **New Network** entry, added as a result of adding two network adapters and click on *Browse*. Select the **TLOCEXT2_vEdge** Network

15.  Make sure the Network Adapters match with the image below and click on OK
    ![](/images/Deploying_vEdge21/03_addnetad.PNG)

16. Click on vEdge21-podX and choose to power it on

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge21 VM~~](creating-the-vedge21-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge21 VM on vCenter~~](#deploying-the-vedge21-vm-on-vcenter)
    <br/>
- [Onboarding vEdge21](#onboarding-vedge21)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge21 (Initial Configuration)](#bootstrapping-vedge21-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}

## Onboarding vEdge21

### Bootstrapping vEdge21 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | IP               | Gateway     |
|---------|---------------|---------|-------------------|----------------|-----------|------------------|-------------|
| 20      | 10.255.255.22 | vEdge21 | Network Adapter 1 | Management     | eth0      | 192.168.0.21/24  | 192.168.0.1 |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 192.168.25.21/24 |             |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 10.20.10.3/24    |             |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 10.20.20.3/24    |             |
|         |               |         | Network Adapter 5 | MPLS20         | ge0/0     | 192.0.2.10/30    | 192.0.2.9   |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 192.168.26.21/24 |             |

1. Console in to the vEdge21 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in).

    | Username | Password     |
    | ------------- | ------------ |
    | admin     | admin       |

    {% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

3. >Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_vEdge21/04_bootstrap.PNG)
    ```
    conf t
    system
     host-name vEdge21
     system-ip 10.255.255.22
     organization-name "swat-sdwanlab"
     site-id 20
     vbond 100.100.100.3
     exit
    !
    vpn 0
     ip route 0.0.0.0/0 192.0.2.9
     interface ge0/0
      ip address 192.0.2.10/30
      no tunnel-interface
      no shutdown
      exit
     !
     exit
    !
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.21/24
      no shutdown
    !
    commit and-quit
    ```  
4. Open **Putty** and double-click the saved session for vEdge21 (or **SSH** to **192.168.0.21**)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Log in using the same credentials as Step 2.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge21 VM~~](creating-the-vedge21-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge21 VM on vCenter~~](#deploying-the-vedge21-vm-on-vcenter)
    <br/>
- [Onboarding vEdge21](#onboarding-vedge21)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge21 (Initial Configuration)~~](#bootstrapping-vedge21-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
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

4. >Select Cloud-Init and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. >Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the vEdge21 device. Alternatively, leave this popup open and we can come back to it when required

    ![](/images/Deploying_vEdge21/05_genbootstrap.PNG)

6. Go back to the Putty session for vEdge21 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully
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

8. Issue the `request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)`command. Replace the *(Enter your UUID)* and *(Enter your OTP)* fields with the UUID and OTP generated in Step 5.
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for vEdge21

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge21 VM~~](creating-the-vedge21-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge21 VM on vCenter~~](#deploying-the-vedge21-vm-on-vcenter)
    <br/>
- [~~Onboarding vEdge21~~](#onboarding-vedge21)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge21 (Initial Configuration)~~](#bootstrapping-vedge21-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}
