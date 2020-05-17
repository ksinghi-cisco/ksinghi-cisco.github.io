---
title: Deploying a Single uplink (INET) vEdge
tags: [getting_started, troubleshooting]
keywords:
summary: "Deploying vEdge20 in Site 20. This vEdge has a single uplink to the Internet"
sidebar: mydoc_sidebar
permalink: mydoc_Deploying_vEdge20.html
folder: mydoc
---

Task List

- [ ] Creating the vEdge20 VM
- [ ] Onboarding vEdge20
- [ ] Onboarding Verification

## Creating the vEdge20 VM

### Overview

{% include note.html content="There will be a number of repetitive tasks from the Deploying DC-vEdge1/DC-vEdge2 section." %}

{% include note.html content="The important steps which will guide you through this activity will be earmarked, indicating a delta from the previous section." %}

> This is what an earmarked step will look like

We will be deploying a vEdge at Site 20 via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | VLAN | IP                | Gateway       |
|---------|---------------|---------|-------------------|----------------|-----------|------|-------------------|---------------|
| 20      | 10.255.255.21 | vEdge20 | Network Adapter 1 | Management     | eth0      | 0    | 192.168.0.20/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 25   | 192.168.25.20/24  |               |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 21   | 10.20.10.1/24     |               |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 22   | 10.20.20.1/24     |               |
|         |               |         | Network Adapter 5 | Internet       | ge0/0     | 2    | 100.100.100.20/24 | 100.100.100.1 |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 26   | 192.168.26.20/24  |               |

### Deploying the vEdge20 VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. > Change the Virtual Machine name to vEdge20 and click on Next.

    ![](/images/Deploying_vEdge20/01_NamevEdge20.PNG)

5. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. >Populate the VM Networks as per the image given below

    {% include important.html content="Please make sure that these look exactly as shown below" %}

    ![](/images/Deploying_vEdge20/02_nwadapters.PNG)
9. Click on **Finish** to deploy your vEdge20 VM

    ![](/images/Deploying_vEdge20/03_summary.PNG)

10. Once the VM is deployed, right click **vEdge20** and click Edit settings.

    ![](/images/Deploying_vEdge20/04_edtisett.PNG)

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 6 for our lab). Add another network device this way, for a total of 6 network adapters.

12. Click on the drop down next to the first **New Network** and click on *Browse*

    ![](/images/Deploying_vEdge20/05_browsenetint.PNG)

13. Choose the **Internet** Network and click on OK.

    ![](/images/Deploying_vEdge20/06_chooseInt.PNG)

14. Click on the drop down for the second **New Network** entry, added as a result of adding two network adapters and click on *Browse*. Select the **TLOCEXT2_vEdge** Network

    ![](/images/Deploying_vEdge20/07_browseagain_tlocext2.PNG)

15.  Make sure the Network Adapters match with the image below and click on OK

    ![](/images/Deploying_vEdge20/99_nwad.PNG)

16. Click on vEdge20 and choose to power it on

<br>


Task List


- [x] Creating the vEdge20 VM
- [ ] Onboarding vEdge20
- [ ] Onboarding Verification

## Onboarding vEdge20

### Bootstrapping vEdge20 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM ID     | VM      | Network Adapter   | Network        | Interface | VLAN | IP                | Gateway       |
|---------|---------------|---------|-------------------|----------------|-----------|------|-------------------|---------------|
| 20      | 10.255.255.21 | vEdge20 | Network Adapter 1 | Management     | eth0      | 0    | 192.168.0.20/24   | 192.168.0.1   |
|         |               |         | Network Adapter 2 | TLOCEXT_vEdge  | ge0/1     | 25   | 192.168.25.20/24  |               |
|         |               |         | Network Adapter 3 | Site20-VPN10   | ge0/2     | 21   | 10.20.10.1/24     |               |
|         |               |         | Network Adapter 4 | Site20-VPN20   | ge0/3     | 22   | 10.20.20.1/24     |               |
|         |               |         | Network Adapter 5 | Internet       | ge0/0     | 2    | 100.100.100.20/24 | 100.100.100.1 |
|         |               |         | Network Adapter 6 | TLOCEXT2_vEdge | ge0/4     | 26   | 192.168.26.20/24  |               |

1. Console in to the vEdge20 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in).

    | Username | Password     |
    | ------------- | ------------- |
    | admin     | admin       |

{% include note.html content="From version 19.2, the password will need to be reset on initial login. For this lab, we will reset the password to `admin`." %}

3. >Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly

    ![](/images/Deploying_DC_vEdge2/06_bootstap.PNG)
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
4. Open **Putty** and double-click the saved session for DC-vEdge2 (or **SSH** to **192.168.0.20**)

    ![](/images/Deploying_DC_vEdge2/07_dcvedge2putty.PNG)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Log in using the same credentials as Step 2.

<br>

### Installing certificates and activating the vEdge

1. Type `vshell` and  enter `scp admin@192.168.0.6:ROOTCA.pem .` to copy the ROOTCA.pem certificate to the vEdge. Commands can be copy-pasted now since we have SSH'd in to the vEdge (there is a dot at the end of the scp command). Enter `yes` when prompted and enter the password of vManage (i.e. admin)
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

5. >Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the vEdge20 device. Alternatively, leave this popup open and we can come back to it when required

    ![](/images/Deploying_DC_vEdge2/09_uuid_otp.PNG)

6. Go back to the Putty session for vEdge20 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully

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
This completes the Onboarding section for vEdge20


<br>

Task List


- [x] Creating the vEdge20 VM
- [x] Onboarding vEdge20
- [ ] Onboarding Verification

## Onboarding Verification

1. Wait for a couple of minutes and run `show control connections` in the vEdge20 CLI. We should see that the vEdge has been able to establish a DTLS tunnel with the vManage and the vSmarts. If you don't see any output, wait for a couple of minutes and run the command again

    ![](/images/Deploying_DC_vEdge2/16_shcontrolconn.PNG)

    ```
    show control connections
    ```
{% include tip.html content="You can also issue `show control connections-history` in the event of failures to find out why is the connection not working as expected. A few helpful commands are `show certificate installed` and `show certificate validity`" %}

2. On the vManage GUI, navigate to **Monitor -> Network Devices** (the computer icon on the left-hand side)

    ![](/images/Deploying_DC_vEdge1/34_monitor_network.png)

3. vEdge20 should show up in the list of devices

    ![](/images/Deploying_DC_vEdge2/13_monitornetworks.PNG)

4. Click on vEdge20 and navigate to **Troubleshooting -> Control Connections(Live view)**. You should see the vEdge successfully connected to 2 vSmarts and 1 vManage

    ![](/images/Deploying_DC_vEdge2/14_2smarts_vmanage.PNG)

5. On the main dashboard, notice that we now have two WAN Edges onboarded on DNAC. The site doesn't have WAN connectivity yet since BFD sessions are not being established as of now. This will change once we get more sites onboard

    ![](/images/Deploying_DC_vEdge2/15_maindashboard.PNG)

This completes the verification activity.

<br>

Task List


- [x] Creating the vEdge20 VM
- [x] Onboarding vEdge20
- [x] Onboarding Verification