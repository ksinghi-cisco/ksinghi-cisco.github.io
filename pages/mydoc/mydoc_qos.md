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

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">

<html>

 <body>

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
	<tr>
		<td style="border-top: 2px solid #000000; border-bottom: 2px solid #000000; border-left: 2px solid #000000; border-right: 1px solid #000000" rowspan=8 height="155" align="center" valign=middle bgcolor="#DAE3F3" sdval="1000" sdnum="1033;"><font face="Century Gothic" color="#000000">1000</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.255.255.1</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">vManage</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">eth1</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.6/24</font></td>
		<td style="border-top: 2px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.2/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle><font face="Century Gothic" color="#000000">10.255.255.2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle><font face="Century Gothic" color="#000000">vBond</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">eth1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.168.0.7/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">100.100.100.3/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.255.255.3</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">vSmart</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">eth1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.8/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.4/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">10.255.255.4</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=2 align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">vSmart2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">eth1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF" sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">192.168.0.9/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF" sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">100.100.100.5/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#FFFFFF"><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>
	<tr>
		<td style="border-bottom: 2px solid #000000; border-left: 2px solid #000000; border-right: 1px solid #000000" rowspan=10 height="194" align="center" valign=middle bgcolor="#FFFFFF" sdval="1" sdnum="1033;"><font face="Century Gothic" color="#000000">1</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=5 align="center" valign=middle><font face="Century Gothic" color="#000000">10.255.255.11</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=5 align="center" valign=middle><font face="Century Gothic" color="#000000">DC-vEdge1</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.168.0.10/24</font></td>
		<td style="border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">MPLS10</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">ge0/1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="110" sdnum="1033;"><font face="Century Gothic" color="#000000">110</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.0.2.2/30</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">192.0.2.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 3</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">SiteDC_VPN10</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">ge0/2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="10" sdnum="1033;"><font face="Century Gothic" color="#000000">10</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">10.100.10.2/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">10.100.10.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 4</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">SiteDC-VPN20</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">ge0/3</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="12" sdnum="1033;"><font face="Century Gothic" color="#000000">12</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">10.100.20.2/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">10.100.20.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Network Adapter 5</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">ge0/0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">100.100.100.10/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=5 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.255.255.12</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" rowspan=5 align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">DC-vEdge2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Management</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">eth0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="0" sdnum="1033;"><font face="Century Gothic" color="#000000">0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.11/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.168.0.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">MPLS11</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">ge0/1</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="111" sdnum="1033;"><font face="Century Gothic" color="#000000">111</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.0.2.6/30</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">192.0.2.5</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 3</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">SiteDC_VPN10</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">ge0/2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="10" sdnum="1033;"><font face="Century Gothic" color="#000000">10</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.100.10.3/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.100.10.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 4</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">SiteDC-VPN20</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">ge0/3</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="12" sdnum="1033;"><font face="Century Gothic" color="#000000">12</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.100.20.3/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 1px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">10.100.20.1</font></td>
	</tr>
	<tr>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Network Adapter 5</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">Internet</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">ge0/0</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4" sdval="2" sdnum="1033;"><font face="Century Gothic" color="#000000">2</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.11/24</font></td>
		<td style="border-top: 1px solid #000000; border-bottom: 2px solid #000000; border-left: 1px solid #000000; border-right: 2px solid #000000" align="center" valign=middle bgcolor="#4472C4"><font face="Century Gothic" color="#000000">100.100.100.1</font></td>
	</tr>

  </table>
<!-- ************************************************************************** -->
 </body>

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
