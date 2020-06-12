---
title: Configuring Low Latency Queuing and QoS
tags: [formatting, special_layouts]
keywords: definitions, glossaries, terms, style guide
last_updated: June 2, 2020
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

| Field              | Global or Device Specific (drop down) | Value         |
|--------------------|---------------------------------------|---------------|
| Ingress ACL - IPv4 | Global                                | On            |
|      ^^              | Global                                | Impair-PL-AAR |
|     ^^               | Global                                | On            |
|                    | Global                                | Impair-PL-AAR |

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

```html
{% raw %}<dl class="dl-horizontal">

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

</dl>{% endraw %}
```

If you squish your screen small enough, at a certain breakpoint this style reverts to the regular `dl` class.

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
