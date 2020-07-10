---
title: Deploying a dual uplink vEdge
permalink: mydoc_Deploying_vEdge30.html
keywords: vedge30, Site 30, dual uplink vEdge
summary: "Deploying vEdge30 in Site 30. This vEdge has dual uplinks (INET and MPLS)"
sidebar: mydoc_sidebar
folder: mydoc
---



<br>

{% include callout.html content="**Task List**
<br/><br/>
- Creating the vEdge30 VM
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Overview
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Deploying the vEdge30 VM on vCenter
    <br/>
- Onboarding vEdge30
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Bootstrapping vEdge30 (Initial Configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Installing certificates and activating the vEdge
    <br/>

" type="primary" %}

## Creating the vEdge30 VM on vCenter

### Overview

<p class="lead" data-step="1" data-intro="1st Step">

<a
	class="btn btn-large btn-primary"
	href="javascript:introJs().setOption('showProgress', true).start();"
	>Get a demo tour</a>



{% include warning.html content="Since we have gone through deploying vEdges multiple times by now, this section will just have the steps listed out. Images for every step has not been populated due to similarity with the previous sections. " %}
</p>

{% include note.html content="The important steps which will guide you through this activity will be earmarked, indicating a delta from the previous sections." %}

> This is what an earmarked step will look like

We will be deploying a vEdge at Site 30 via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface | IP                | Gateway       |
|---------|---------------|---------|-------------------|--------------|-----------|-------------------|---------------|
| 30      | 10.255.255.31 | vEdge30-podX | Network Adapter 1 | Management   | eth0      | 192.168.0.30/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | MPLS30       | ge0/1     | 192.0.2.14/30     | 192.0.2.13    |
|         |               |         | Network Adapter 3 | Site30-VPN10 | ge0/2     | 10.30.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site30-VPN20 | ge0/3     | 10.30.20.2/24     |               |
|         |               |         | Network Adapter 5 | Internet     | ge0/0     | 100.100.100.30/24 | 100.100.100.1 |

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [Creating the vEdge30 VM](creating-the-vedge30-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Deploying the vEdge30 VM on vCenter](#deploying-the-vedge30-vm-on-vcenter)
    <br/>
- [Onboarding vEdge30](#onboarding-vedge30)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge30 (Initial Configuration)](#bootstrapping-vedge30-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}

### Deploying the vEdge30 VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. > Change the Virtual Machine name to **vEdge30-podX** and click on Next (where X is your POD number)

    {% include note.html content="We will only use the podX suffix over here to distinguish between different VMs in our Data Center. The rest of the guide will refer to this VM as **vEdge30**" %}

5. Select the host assigned to you (image shown as an example only) and click on Next

    {% include note.html content="If the screen gets stuck over here at **Validating** then close Chrome and open the vCenter in Internet Explorer, going through the same steps. Deployment should go through. This is a known issue with Google Chrome." %}

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. >Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}
    ![](/images/Deploying_vEdge30/01_netad.PNG)
9. Click on **Finish** to deploy your vEdge30-podX VM

10. Once the VM is deployed, right click on **vEdge30-podX** and click Edit settings.

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 5 for our lab).

12. Click on the drop down next to the **New Network** and click on *Browse*

13. > Choose the **Internet** Network and click on OK.

14.  > Make sure the Network Adapters match with the image below and click on *OK*
    ![](/images/Deploying_vEdge30/02_addnwad.PNG)

15. Click on vEdge30-podX and choose to power it on

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge30 VM~~](creating-the-vedge30-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge30 VM on vCenter~~](#deploying-the-vedge30-vm-on-vcenter)
    <br/>
- [Onboarding vEdge30](#onboarding-vedge30)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Bootstrapping vEdge30 (Initial Configuration)](#bootstrapping-vedge30-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Installing certificates and activating the vEdge](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}

## Onboarding vEdge30

### Bootstrapping vEdge30 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network      | Interface | IP                | Gateway       |
|---------|---------------|---------|-------------------|--------------|-----------|-------------------|---------------|
| 30      | 10.255.255.31 | vEdge30 | Network Adapter 1 | Management   | eth0      | 192.168.0.30/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | MPLS30       | ge0/1     | 192.0.2.14/30     | 192.0.2.13    |
|         |               |         | Network Adapter 3 | Site30-VPN10 | ge0/2     | 10.30.10.2/24     |               |
|         |               |         | Network Adapter 4 | Site30-VPN20 | ge0/3     | 10.30.20.2/24     |               |
|         |               |         | Network Adapter 5 | Internet     | ge0/0     | 100.100.100.30/24 | 100.100.100.1 |

1. Console in to the vEdge30 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in).

    | Username | Password     |
    | ------------- | ------------ |
    | admin     | admin       |

    {% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

3. >Enter the configuration enumerated below. Unfortunately, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_vEdge30/03_bootstrap.PNG)
    ```
    conf t
    system
     host-name vEdge30
     system-ip 10.255.255.31
     organization-name "swat-sdwanlab"
     site-id 30
     vbond 100.100.100.3
     exit
    !
    vpn 0
     ip route 0.0.0.0/0 100.100.100.1
     interface ge0/0
      ip address 100.100.100.30/24
      no tunnel-interface
      no shutdown
      exit
     !
     exit
    !
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.30/24
      no shutdown
    !
    commit and-quit
    ```  
4. Open **Putty** and double-click the saved session for vEdge30 (or **SSH** to **192.168.0.30**)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Log in using the same credentials as Step 2.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge30 VM~~](creating-the-vedge30-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge30 VM on vCenter~~](#deploying-the-vedge30-vm-on-vcenter)
    <br/>
- [Onboarding vEdge30](#onboarding-vedge30)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge30 (Initial Configuration)~~](#bootstrapping-vedge30-initial-configuration)
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
2. Go to the vManage GUI (https://192.168.0.6) and log in, if logged out. Navigate to **Configuration => Devices** (from the left-hand side, click on the cog wheel to access the configuration options)

    ![](/images/Deploying_DC_vEdge1/26_config_devices.png)

3. >Choose any vEdge Cloud device (it doesn't matter which one you pick, as long as it is a vEdge Cloud) and click on the three dots at the extreme right-hand side. Choose to Generate Bootstrap Configuration

    ![](/images/Deploying_vEdge30/04_genbootgui.PNG)

4. >Select Cloud-Init and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. >Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the vEdge30 device. Alternatively, leave this popup open and we can come back to it when required

    ![](/images/Deploying_vEdge30/05_bootstrapconfig.PNG)

6. Go back to the Putty session for vEdge30 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully
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

8. Issue the `request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)`command. Replace the *(Enter your UUID)* and *(Enter your OTP)* fields with the UUID and OTP generated in Step 5 (image below is an example, UUID and OTP may not match).
    ![](/images/Deploying_vEdge30/06_certtunn.PNG)
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for vEdge30

<br>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Creating the vEdge30 VM~~](creating-the-vedge30-vm)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Overview~~](#overview)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Deploying the vEdge30 VM on vCenter~~](#deploying-the-vedge30-vm-on-vcenter)
    <br/>
- [~~Onboarding vEdge30~~](#onboarding-vedge30)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Bootstrapping vEdge30 (Initial Configuration)~~](#bootstrapping-vedge30-initial-configuration)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Installing certificates and activating the vEdge~~](#installing-certificates-and-activating-the-vedge)
    <br/>

" type="primary" %}

<script type="text/javascript" src="intro.js"></script>
