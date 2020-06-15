---
title: Configuring Cloud OnRamp for SaaS
keywords: archives, tagging
last_updated: June 3, 2020
tags: [navigation]
summary: "Implementing Cloud OnRamp for SaaS in Cisco SD-WAN"
sidebar: mydoc_sidebar
permalink: mydoc_conrampsaas.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Prerequisite configuration for Cloud OnRamp
<br/>
- Configuring Cloud OnRamp for SaaS
<br/>
- Verification and Testing
<br/>
" type="primary" %}

## Overview

With the changing network landscape, the way in which applications are consumed has also undergone a massive overhaul. Applications being hosted in the cloud (Public/Private) are a common occurrence, rather than the exception.

Cloud OnRamp for SaaS monitors widely used Cloud Applications and arrives at a vQoE score (Viptela Quality of Experience). Loss and latency are used to calculate the vQoE score and based on this, the solution routes traffic to the Cloud Application via the optimal path. The vQoE value is calculated periodically to ensure persistent optimal application performance.

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Prerequisite configuration for Cloud OnRamp](#prerequisite-configuration-for-cloud-onramp)
<br/>
- [Configuring Cloud OnRamp for SaaS](#configuring-cloud-onramp-for-saas)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Prerequisite configuration for Cloud OnRamp

1. On the vManage GUI, navigate to **Configuration -> Templates -> Feature Tab**. Locate the *vEdge30_INET* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/Cloud_OnRamp/06_edit.PNG)

2. Scroll down to the NAT section and set NAT to a global value of *On*. Click on **Update**

    ![](/images/Cloud_OnRamp/07_nat.PNG)

3. Click on **Next** and **Configure Device**. There are no changes to be made here since we are simply enabling NAT on the interface.

4. On the vManage GUI, go to **Configuration -> Templates -> Feature Tab**. Locate the *DC-vEdge_INET* template and click on the three dots next to it. Choose to **Edit** the template

    ![](/images/Cloud_OnRamp/08_dcnat.PNG)

5. 





<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Prerequisite configuration for Cloud OnRamp~~](#prerequisite-configuration-for-cloud-onramp)
<br/>
- [Configuring Cloud OnRamp for SaaS](#configuring-cloud-onramp-for-saas)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Configuring Cloud OnRamp for SaaS

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Prerequisite configuration for Cloud OnRamp~~](#prerequisite-configuration-for-cloud-onramp)
<br/>
- [~~Configuring Cloud OnRamp for SaaS~~](#configuring-cloud-onramp-for-saas)
<br/>
- [Verification and Testing](#verification-and-testing)
<br/>
" type="primary" %}

## Verification and Testing

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Prerequisite configuration for Cloud OnRamp~~](#prerequisite-configuration-for-cloud-onramp)
<br/>
- [~~Configuring Cloud OnRamp for SaaS~~](#configuring-cloud-onramp-for-saas)
<br/>
- [~~Verification and Testing~~](#verification-and-testing)
<br/>
" type="primary" %}
