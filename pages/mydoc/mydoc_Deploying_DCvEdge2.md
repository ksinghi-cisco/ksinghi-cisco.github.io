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

Task List

- [ ] Creating the DC-vEdge2 VM
- [ ] Onboarding DC-vEdge2
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Creating the DC-vEdge2 VM

### Overview

**Note 1: There will be a number of repetitive tasks from the Deploying DC-vEdge1 section. In the interest of brevity, only the delta from the previous section will be enumerated over here with the steps being listed out. Images will be similar to the ones seen in the previous section so many will be carried over or omitted entirely.**

**Note 2: The important sections which will guide you through this activity will be `highlighted like this`**


We will be deploying another vEdge in our first site (the Data Center) via vCenter. Make note of the following information for this section. The IP Addressing will not be used for some of the Network Adapters until later.


VM Name | System IP | Network Adapter | Network | Interface | IP Address | Default Gateway
------- | ----- | ------------- | ------------- | ------------- | ------------- | -------------
DC-vEdge2 | 10.255.255.12 | Network Adapter 1 | Management | eth0 | 192.168.0.11/24 | 192.168.0.1
 || Network Adapter 2 | MPLS10 | ge0/1 | 192.0.2.6/30 | 192.0.2.5
 || Network Adapter 3 | SiteDC-VPN10 | ge0/2 | 10.100.10.3/24 | 10.100.10.1
 || Network Adapter 4 | SiteDC-VPN20 | ge0/3 | 10.100.20.3/24 | 10.100.20.1
 || Network Adapter 5 | Internet | ge0/0 | 100.100.100.11/24 | 100.100.100.1

> *Plan your sites and addressing carefully. Proper planning can prevent a number of issues and will help with a successful, early deployment.*

> *This configuration is applicable only for virtual vEdges/cEdges. Physical vEdges are a lot easier to deploy, not only from a connectivity standpoint but also with respect to certificate exchange options.*

### Deploying the VM on vCenter
<br>

1. Click on the bookmark for vCenter or navigate to the following URL: https://10.2.1.50/ui. Log in with the credentials provided for your POD.

2. Right click on the host and choose to **Deploy OVF Template**

    ![](/images/Deploying_DC_vEdge1/05_rightclickhost_deployovf.png)
3. Choose the **Local file** option and click on **Choose files**. Navigate to the SD-WAN images folder and select the file beginning with *viptela-edge-*. Click on Next.

4. `Change the Virtual Machine name to **DC-vEdge2** and click on Next.`

5. Select the host assigned to you (image shown as an example only) and click on Next

    ![](/images/Deploying_DC_vEdge1/08_leavethehostasis.PNG)
6. Review the details shown and click on Next

    ![](/images/Deploying_DC_vEdge1/09_reviewdetails_next.PNG)
7. Choose the Datastore and click on Next

8. Populate the VM Networks as per the table given at the start of this section (or reference the image below)

    ![](/images/Deploying_DC_vEdge1/11_populatevmnetworks_referenceipschema.PNG)
9. Click on **Finish** to deploy your DC-vEdge2 VM

10. Once the VM is deployed, right click **DC-vEdge2** and click Edit settings.

11. Choose to **Add a new device** (top right corner) and select Network Adapter to add one (since our deployed VM has only 4 Network Adapters but we will need 5 for our lab)

12. Click on the drop down next to **New Network** and click on *Browse*

    ![](/images/Deploying_DC_vEdge1/15_dropdown_browse.png)
13. Choose the **Internet** Network and click on OK. Make sure the Network Adapters match with the second image below and click on OK again

    ![](/images/Deploying_DC_vEdge1/16_chooseinternet_ok_ok.PNG)

    ![](/images/Deploying_DC_vEdge1/17_NetworkAdaptersdcvedge1.PNG)
14. Click on DC-vEdge2 and choose to power it on

<br>


Task List


- [x] Creating the DC-vEdge2 VM
- [ ] Onboarding DC-vEdge2
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Onboarding DC-vEdge2

### Bootstrapping DC-vEdge2 (Initial Configuration)

Use the following information in this section (some of the information will be used later)

| SITE ID | SYSTEM IP     | VM        | Network Adapter   | Network      | Interface | IP                | Gateway       |
|---------|---------------|-----------|-------------------|--------------|-----------|-------------------|---------------|
| 1       | 10.255.255.12 | DC-vEdge2 | Network Adapter 1 | Management   | eth0      | 192.168.0.11/24   | 192.168.0.1   |
|         |               |           | Network Adapter 2 | MPLS10       | ge0/1     | 192.0.2.6/30      | 192.0.2.5     |
|         |               |           | Network Adapter 3 | SiteDC-VPN10 | ge0/2     | 10.100.10.3/24    | 10.100.10.1   |
|         |               |           | Network Adapter 4 | SiteDC-VPN20 | ge0/3     | 10.100.20.3/24    | 10.100.20.1   |
|         |               |           | Network Adapter 5 | Internet     | ge0/0     | 100.100.100.11/24 | 100.100.100.1 |

1. Console in to the DC-vEdge2 VM from vCenter (you should already be logged in from our last activity)

2. Wait for the VM to prompt you for the username and password and enter the credentials given below. If you get a message stating that they are incorrect, wait for 30 seconds and try again (since the processes need to initialize before you can log in)

    | Username | Password     |
    | ------------- | ------------- |
    | admin     | admin       |

3. `Enter the configuration enumerated below. Unfortunatley, this will need to be typed out since the console isn't copy-paste friendly`

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
    vpn 512
     ip route 0.0.0.0/0 192.168.0.1
     interface eth0
      ip address 192.168.0.11/24
      no shutdown
    !
    commit and-quit
```
    > We are ensuring that the vEdge has basic IP Addressing and Routing to the Controllers. `no tunnel-interface` has been added under the ge0/0 interface in VPN 0 in order to prevent control connections from being established

4. Open **Putty** and double click the saved session for DC-vEdge2 (or **SSH** to **192.168.0.11**)

5. Choose Yes to accept the certificate, if prompted

    ![](/images/Deploying_DC_vEdge1/23_cert_yes.PNG)

6. Login using the same credentials as Step 1.

<br>

### Installing certificates and activating the vEdge

1. Type `vshell` and  enter `scp admin@192.168.0.6:ROOTCA.pem .` to copy the ROOTCA.pem certificate to the vEdge. Commands can be copy-pasted now since we have SSH'd in to the vEdge (there is a dot at the end of the scp command)
    ```
    vshell
    scp admin@192.168.0.6:ROOTCA.pem .
    exit
    ```
2. Go to the vManage GUI (https://192.168.0.6) and log in, if logged out. Navigate to **Configuration -> Devices** (from the left-hand side, click on the cog wheel to access the configuration options)

    ![](/images/Deploying_DC_vEdge1/26_config_devices.png)

3. `Choose any vEdge Cloud device (it doesn't matter which one you pick, as long as it is a vEdge Cloud) and click on the three dots at the extreme right-hand side. Choose **Generate Bootstrap Configuration**`

4. Select **Cloud-Init** and click on OK

    ![](/images/Deploying_DC_vEdge1/28_cloudinit_ok.PNG)

5. `Make note of the **UUID** and the **OTP** values. These will be required to activate the vEdge. It's best to copy the string and place it in notepad, since we will need to use it in our SSH session to the DC-vEdge1 device. Alternatively, leave this popup open and we can come back to it when required`

6. Go back to the Putty session for DC-vEdge2 and enter `request root-cert-chain install /home/admin/ROOTCA.pem`to install the root cert chain. It should install successfully
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

    ![](/images/Deploying_DC_vEdge1/32_activatevedge_uuid_token_diff.PNG)
    ```
    request vedge-cloud activate chassis-number (Enter your UUID) token (Enter the OTP)
    ```
This completes the Onboarding section for DC-vEdge2


<br>

Task List


- [x] Creating the DC-vEdge2 VM
- [x] Onboarding DC-vEdge2
- [ ] Onboarding Verification
- [ ] Helpful debugs and logs

## Onboarding Verification

1. Wait for a couple of minutes and run `show control connections` in the DC-vEdge2 CLI. We should see that the vEdge has been able to establish a DTLS tunnel with the vManage and the vSmarts. If you don't see any output, wait for a couple of minutes and run the command again
    ```
    show control connections
    ```
> You can also issue `show control connections-history` in the event of failures to find out why is the connection not working as expected. A few helpful commands are `show certificate installed` and `show certificate validity`.

2. On the vManage GUI, navigate to **Monitor -> Network Devices** (the computer icon on the left-hand side)

    ![](/images/Deploying_DC_vEdge1/34_monitor_network.png)

3. DC-vEdge2 should show up in the list of devices

4. Click on DC-vEdge2 and navigate to **Troubleshooting -> Control Connections(Live view)**. You should see the vEdge successfully connected to 2 vSmarts and 1 vManage


This completes the verification activity.

<br>

Task List


- [x] Creating the DC-vEdge2 VM
- [x] Onboarding DC-vEdge2
- [x] Onboarding Verification
- [ ] Helpful debugs and logs

## Helpful debugs and logs


**This section is optional and can be used for learning. It is not required to go through this in order to complete the lab activities successfully.**

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
Task List
<br>
<br>


- [x] Creating the DC-vEdge2 VM
- [x] Onboarding DC-vEdge2
- [x] Onboarding Verification
- [x] Helpful debugs and logs
