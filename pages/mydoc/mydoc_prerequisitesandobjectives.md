---
title: Before you begin
sidebar: mydoc_sidebar
permalink: mydoc_prerequisitesandobjectives.html
folder: mydoc
---

## Prerequisites

### What will you need?
<br>


* A workstation with Windows or MacOS installed

* Cisco AnyConnect. This can be downloaded from [here](https://ghi-vpn.swat4partners.com){:target="_blank"} after logging in with the credentials provided

* A stable internet connection that has standard Cisco AnyConnect ports allowed

{% include note.html content="It is recommended to open this Lab Guide on one screen and perform lab activities on another" %}


### What should you know?

* Fundamental knowledge of Routing & Switching with a few details of Data Center operations

* Familiarity with Cisco SD-WAN as a solution and its architecture/protocols. A few helpful links can be found in the top navigation bar under **SD-WAN Documentation**

* Knowledge of Cisco WAAS and NGIPS concepts is an added advantage


## Objectives


### What will you learn?

This lab has multiple use cases that are covered as part of the tasks. We are working on expanding this list as and when new features are tested/released.

* Deploying vEdges and cEdges in a virtual environment

* Onboarding devices on vManage

   * Manual Onboarding of vEdges and cEdges
   * Day 0 bootstrapping of cEdges


* Working with Configuration Templates

  * Bringing up cEdges and vEdges with Single uplinks
  * Bringing up cEdges and vEdges with Dual uplinks


* Implementing Service VPNs and Dynamic Service Side routing using OSPF and EIGRP

  * Establishing OSPF adjacencies at DC with route redistribution
  * Establishing EIGRP adjacencies at Site 40 with route redistribution
  * Configuring VRRP at Site 50

* Implementing TLOC Extensions with eBGP Peering

* Working with Control Policies

  * Enforcing a Hub and Spoke Topology
  * Implementing a Regional Hub


* Implementing Data Policies

  * Custom traffic Engineering
  * Direct Internet Access




* Application Aware Routing

  * Influencing Traffic Path selection
  * Introducing Packet Loss via Policers


* Cisco SD-WAN Security

  * IPS Deployment at DIA Sites
  * URL Filtering at DIA Site
  * Cisco SD-AVC


* Cloud On-Ramp for SaaS

  * Injecting delay via a traffic shaper




* WAAS Integration with SD-WAN using AppNav-XE

Happy Labbing!
