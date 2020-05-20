---
title: Deploying and Onboarding DC-vEdge2
tags: [getting_started]
keywords: DC-vEdge2, onboarding, deploying, vEdge2
last_updated: May 15, 2020
summary: "Step by step process for deploying DC-vEdge2"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_DCvEdge2.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Creating the DC-vEdge2 VM
<br/>
- Onboarding DC-vEdge2
<br/>
- Onboarding Verification
    <br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Creating the DC-vEdge2 VM

### Overview

{% include note.html content="There will be a number of repetitive tasks from the Deploying DC-vEdge1 section." %}

{% include note.html content="The important steps which will guide you through this activity will be earmarked, indicating a delta from the previous section." %}

> This is what an earmarked step will look like


We will be deploying another vEdge in our first site (the Data Center) via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.


VM Name | System IP | Network Adapter | Network | Interface | IP Address | Default Gateway
------- | ----- | ------------- | ------------- | ------------- | ------------- | -------------
DC-vEdge2 | 10.255.255.12 | Network Adapter 1 | Management | eth0 | 192.168.0.11/24 | 192.168.0.1
 || Network Adapter 2 | MPLS11 | ge0/1 | 192.0.2.6/30 | 192.0.2.5
 || Network Adapter 3 | SiteDC_VPN10 | ge0/2 | 10.100.10.3/24 | 10.100.10.1
 || Network Adapter 4 | SiteDC-VPN20 | ge0/3 | 10.100.20.3/24 | 10.100.20.1
 || Network Adapter 5 | Internet | ge0/0 | 100.100.100.11/24 | 100.100.100.1

### Deploying the DC-vEdge2 VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. > Change the Virtual Machine name to DC-vEdge2 and click on Next.

    ![](/images/Deploying_DC_vEdge2/01_CreateVM.PNG)

5. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. >Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_DC_vEdge2/02_NetworkAdapters.PNG)
9. Click on **Finish** to deploy your DC-vEdge2 VM

    ![](/images/Deploying_DC_vEdge2/03_Summary.PNG)

10. Once the VM is deployed, right click **DC-vEdge2** and click Edit settings.

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 5 for our lab)

12. Click on the drop down next to **New Network** and click on *Browse*

    ![](/images/Deploying_DC_vEdge2/04_addnetworkadapt.PNG)

13. Choose the **Internet** Network and click on OK. Make sure the Network Adapters match with the second image below and click on OK again

    ![](/images/Deploying_DC_vEdge2/05_Internet.PNG)

    ![](/images/Deploying_DC_vEdge2/99_nwve2.PNG)
14. Click on DC-vEdge2 and choose to power it on

<br>

{% include callout.html content="**Task List**
<br/><br/>
- ~~Creating the DC-vEdge2 VM~~
<br/>
- Onboarding DC-vEdge2
<br/>
- Onboarding Verification
    <br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Onboarding DC-vEdge2

### Bootstrapping DC-vEdge2 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM IP     | VM        | Network Adapter   | Network      | Interface | IP                | Gateway       |
|---------|---------------|-----------|-------------------|--------------|-----------|-------------------|---------------|
| 1       | 10.255.255.12 | DC-vEdge2 | Network Adapter 1 | Management   | eth0      | 192.168.0.11/24   | 192.168.0.1   |
|         |               |           | Network Adapter 2 | MPLS11       | ge0/1     | 192.0.2.6/30      | 192.0.2.5     |
|         |               |           | Network Adapter 3 | SiteDC_VPN10 | ge0/2     | 10.100.10.3/24    | 10.100.10.1   |
|         |               |           | Network Adapter 4 | SiteDC-VPN20 | ge0/3     | 10.100.20.3/24    | 10.100.20.1   |
|         |               |           | Network Adapter 5 | Internet     | ge0/0     | 100.100.100.11/24 | 100.100.100.1 |

1. Console in to the DC-vEdge2 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in)

    | Username | Password     |
    | ------------- | ------------- |
    | admin     | admin       |

    {% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

3. >Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_DC_vEdge2/06_bootstap.PNG)
    ```
    conf t
    system
     host-name DC-vEdge2
     system-ip 10.255.255.12
     site-id 1
     organization-name "swat-sdwanlab"
     vbond 100.100.100.3
     exit
    !
    vpn 0
     ip route 0.0.0.0/0 100.100.100.1
     interface ge0/0
      ip address 100.100.100.11/24
      no tunnel-interface
      no shutdown
      exit
      !
      exit
    !
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.11/24
      no shutdown
    !
    commit and-quit
    ```  
4. Open **Putty** and double-click the saved session for DC-vEdge2 (or **SSH** to **192.168.0.11**)

    ![](/images/Deploying_DC_vEdge2/07_dcvedge2putty.PNG)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Log in using the same credentials as Step 2.

<br>

### Installing certificates and activating the vEdge

1. Type `vshell` and  enter `scp admin@192.168.0.6:ROOTCA.pem .` to copy the ROOTCA.pem certificate to the vEdge. Commands can be copy-pasted now since we have SSH'd in to the vEdge (there is a dot at the end of the scp command). Enter `yes` when prompted and enter the password of vManage (i.e. admin). Exit when done with this step.
    ```
    vshell
    scp admin@192.168.0.6:ROOTCA.pem .
    ```
2. Go to the vManage GUI (https://192.168.0.6) and log in, if logged out. Navigate to **Configuration -> Devices** (from the left-hand side, click on the cog wheel to access the configuration options)

    ![](/images/Deploying_DC_vEdge1/26_config_devices.png)

3. >Choose any vEdge Cloud device (it doesn't matter which one you pick, as long as it is a vEdge Cloud) and click on the three dots at the extreme right-hand side. Choose to Generate Bootstrap Configuration

    ![](/images/Deploying_DC_vEdge2/08_genboot.PNG)

4. >Select Cloud-Init and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. >Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the DC-vEdge2 device. Alternatively, leave this popup open and we can come back to it when required

    ![](/images/Deploying_DC_vEdge2/09_uuid_otp.PNG)

6. Go back to the Putty session for DC-vEdge2 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully

    ![](/images/Deploying_DC_vEdge2/10_installrootcert.PNG)
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

    ![](/images/Deploying_DC_vEdge2/11_enabletunnel_activating.PNG)
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for DC-vEdge2


<br>

{% include callout.html content="**Task List**
<br/><br/>
- ~~Creating the DC-vEdge2 VM~~
<br/>
- ~~Onboarding DC-vEdge2~~
<br/>
- Onboarding Verification
    <br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Onboarding Verification

1. Wait for a couple of minutes and run `show control connections` in the DC-vEdge2 CLI. We should see that the vEdge has been able to establish a DTLS tunnel with the vManage and the vSmarts. If you don't see any output, wait for a couple of minutes and run the command again

    ![](/images/Deploying_DC_vEdge2/16_shcontrolconn.PNG)

    ```
    show control connections
    ```
    {% include tip.html content="You can also issue `show control connections-history` in the event of failures to find out why is the connection not working as expected. A few helpful commands are `show certificate installed`, `show certificate root-ca-cert`, `show control local-properties` and `show certificate validity`. Most of these commands give us details about the status of certificates on the device and are helpful in ascertaining the root cause of failure when control connections aren't getting established." %}

2. On the vManage GUI, navigate to **Monitor -> Network Devices** (the computer icon on the left-hand side)

    ![](/images/Deploying_DC_vEdge1/34_monitor_network.png)

3. DC-vEdge2 should show up in the list of devices

    ![](/images/Deploying_DC_vEdge2/13_monitornetworks.PNG)

4. Click on DC-vEdge2 and navigate to **Troubleshooting -> Control Connections(Live view)**. You should see the vEdge successfully connected to 2 vSmarts and 1 vManage

    ![](/images/Deploying_DC_vEdge2/14_2smarts_vmanage.PNG)

5. On the main dashboard, notice that we now have two WAN Edges onboarded on DNAC. The site doesn't have WAN connectivity yet since BFD sessions are not being established as of now. This will change once we get more sites onboard

    ![](/images/Deploying_DC_vEdge2/15_maindashboard.PNG)

This completes the verification activity.

<br>

{% include callout.html content="**Task List**
<br/><br/>
- ~~Creating the DC-vEdge2 VM~~
<br/>
- ~~Onboarding DC-vEdge2~~
<br/>
- ~~Onboarding Verification~~
    <br/>
- Helpful debugs and logs
<br/>

" type="primary" %}

## Helpful debugs and logs


{% include note.html content="This section is optional and is intended as a learning activity. It is not required to go through this in order to complete the lab tasks successfully" %}

1. On the CLI for DC-vEdge2, issue `debug vdaemon all` followed by `clear control connections`. This will tear down all the control connections and the vEdge will rebuilt the DTLS tunnels. We can capture the logs to see the process associated with the DTLS tunnels being built
    ```
    debug vdaemon all
    clear control connections
    ```

2. Wait for  a couple of minutes and go to `vshell`. Type `cat /var/log/tmplog/vdebug` to view the contents of the log file

3. Given below are a couple of sample outputs

    ![](/images/Deploying_DC_vEdge1/39_sampleoutput.PNG)

    ![](/images/Deploying_DC_vEdge1/40_sampleoutput2.PNG)    


This completes our onboarding activity for DC-vEdge2.

<br>
{% include callout.html content="**Task List**
<br/><br/>
- ~~Creating the DC-vEdge2 VM~~
<br/>
- ~~Onboarding DC-vEdge2~~
<br/>
- ~~Onboarding Verification~~
    <br/>
- ~~Helpful debugs and logs~~
<br/>

" type="primary" %}
