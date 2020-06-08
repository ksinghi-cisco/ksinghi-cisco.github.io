---
title: TLOC Extensions at Site 20
tags: [single_sourcing]
keywords: includes, conref, dita, transclusion, transclude, inclusion, reference
last_updated: July 3, 2016
summary: "Configuring TLOC Extensions for transport redundancy."
sidebar: mydoc_sidebar
permalink: mydoc_tloc_ext.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Feature Templates for TLOC Extensions
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN Interface Template for the TLOC-EXT interface
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the VPN Interface Template for the Tunnel interface
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating the BGP Template for the MPLS link
    <br/>
- Updating the VPN and Device Templates
<br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview
You can embed content from one file inside another using includes. Put the file containing content you want to reuse (e.g., mypage.html) inside the \_includes/custom folder and then use a tag like this:

{% raw %}
```
{% include custom/mypage.html %}
```
{% endraw %}

With content in your \_includes folder, you don't add any frontmatter to these pages because they will be included on other pages already containing frontmatter.

Also, when you include a file, all of the file's contents get included. You can't specify that you only want a specific part of the file included. However, you can use parameters with includes.

{% unless site.output == "pdf" %}
See the following Jekyll cast for more info about using parameters with includes:

<iframe width="640" height="480" src="https://www.youtube.com/embed/kzpGqdEMbIs" frameborder="0" allowfullscreen></iframe>
{% endunless %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the TLOC-EXT interface](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the Tunnel interface](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Feature Templates for TLOC Extensions

### Creating the VPN Interface Template for the TLOC-EXT interface

You can then access the values in those custom variables using the `page` namespace, like this:

{% raw %}
```
thing1: {{page.thing1}}
thing2: {{page.thing2}}
```
{% endraw %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the VPN Interface Template for the Tunnel interface](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the VPN Interface Template for the Tunnel interface

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Feature Templates for TLOC Extensions](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating the BGP Template for the MPLS link](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Creating the BGP Template for the MPLS link

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [Updating the VPN and Device Templates](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Updating the VPN and Device Templates

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [~~Updating the VPN and Device Templates~~](#updating-the-vpn-and-device-templates)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Activity Verification

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Feature Templates for TLOC Extensions~~](#feature-templates-for-tloc-extensions)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the TLOC-EXT interface~~](#creating-the-vpn-interface-template-for-the-tloc-ext-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the VPN Interface Template for the Tunnel interface~~](#creating-the-vpn-interface-template-for-the-tunnel-interface)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating the BGP Template for the MPLS link~~](#creating-the-bgp-template-for-the-mpls-link)
    <br/>
- [~~Updating the VPN and Device Templates~~](#updating-the-vpn-and-device-templates)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
