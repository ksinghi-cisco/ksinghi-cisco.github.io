---
title: Configuring Low Latency Queuing and QoS
tags: [formatting, special_layouts]
keywords: definitions, glossaries, terms, style guided
last_updated: June 2, 2020
datatable: true
summary: "SD-WAN allows configuration of various QoS strategies to better support your business. Configure QoS with LLQ for VoIP traffic"
sidebar: mydoc_sidebar
permalink: mydoc_qos.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Create a Localized Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Add a Class List and a QoS Map
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configure the IPv4 ACL Policy
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Complete and apply the localized policy
    <br/>
- Apply the ACL and QoS Map
<br/>
- Activity Verification
<br/>

" type="primary" %}
</br>

<!DOCTYPE html>
<html>
    <head>
    </head>
        <table cellspacing="0" border="0" class="display">
	       <colgroup span="9" width="64"></colgroup>
	          <tr>
		            <td style="border-top: 2px solid #000000; border-left: 2px solid #000000; border-right: 2px solid #000000" colspan=9 height="19" align="center" valign=bottom bgcolor="#4472C4"><b><font face="Century Gothic" color="#FFFFFF">SWAT SD-WAN LAB SET UP</font></b></td>
		      </tr>
	          <tr>
		            <td style="border-top: 1px solid #000000; border-left: 2px solid #000000; border-right: 1px solid #000000" height="20" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">SITE ID</font></b></td>
		            <td style="border-top: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">SYSTEM ID</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">VM</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">Network Adapter</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">Network</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">Interface</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">VLAN</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">IP</font></b></td>
		            <td style="border-top: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=bottom bgcolor="#FFFFFF"><b><font face="Century Gothic" color="#000000">Gateway</font></b></td>
	          </tr>
        </table>
</html>

## Create a Localized Policy

You can create a glossary for your content. First create your glossary items in a data file such as glossary.yml.

### Add a Class List and a QoS Map

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Create a Localized Policy](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configure the IPv4 ACL Policy](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Complete and apply the localized policy](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Configure the IPv4 ACL Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [Create a Localized Policy](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Complete and apply the localized policy](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Complete and apply the localized policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [Apply the ACL and QoS Map](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Apply the ACL and QoS Map

You can also change the definition list (`dl`) class to `dl-horizontal`. This is a Bootstrap specific class. If you do, the styling looks like this:

<dl class="dl-horizontal">

<dt id="fractious">fractious</dt>
<dd>{{site.data.glossary.fractious}}</dd>

<dt id="gratuitous">gratuitous</dt>
<dd>{{site.data.glossary.gratuitous}}</dd>

<dt id="haughty">haughty</dt>
<dd>{{site.data.glossary.haughty}}</dd>

<dt id="benchmark_id">gratuitous</dt>
<dd>{{site.data.glossary.gratuitous}}</dd>

<dt id="impertinent">impertinent</dt>
<dd>{{site.data.glossary.impertinent}}</dd>

<dt id="intrepid">intrepid</dt>
<dd>{{site.data.glossary.intrepid}}</dd>

</dl>

For this type of list, you must use HTML. The list would then look like this:

Idf you squish your screen small enough, at a certain breakpoint this style reverts to the regular `dl` class.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [~~Apply the ACL and QoS Map~~](#apply-the-acl-and-qos-map)
<br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Activity Verification

{% include warning.html content="STOP!!!! Time to take a snapshot. Refer to the Testing Procedure, Step 7 of the SOP" %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Create a Localized Policy~~](#create-a-localized-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Add a Class List and a QoS Map~~](#add-a-class-list-and-a-qos-map)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configure the IPv4 ACL Policy~~](#configure-the-ipv4-acl-policy)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Complete and apply the localized policy~~](#complete-and-apply-the-localized-policy)
    <br/>
- [~~Apply the ACL and QoS Map~~](#apply-the-acl-and-qos-map)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
