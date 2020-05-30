---
title: Dynamic Service Side routing at the DC
tags: [formatting]
keywords: font icons, buttons, images, vectors, font awesome, glyphicons
last_updated: May 26, 2020
summary: "Implementing Dynamic Service Side Routing at the DC - OSPF"
sidebar: mydoc_sidebar
permalink: mydoc_ospf_dc.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>
- Overview
<br/>
- Updating the vEdge Service VPN 10 with an OSPF Template
<br/>
- Activity Verification
<br/>
" type="primary" %}

## Overview
Sites in the Cisco SD-WAN will generally have an L3 device on the LAN other than the vEdges/cEdges. These devices might be servicing LAN users and advertising their routes via an IGP of choice. We need to make sure that these routes are advertised across the SD-WAN Fabric. While static routing can be used to achieve this, it is time consuming and extremely prone to errors. Thus, running a Dynamic Routing Protocol between the WAN Edge devices and the L3 devices is usually preferred.

We will run OSPF on VPN 10 in the DC with an L3 Device (called the Central Gateway). The Central Gateway has been configured with the corresponding OSPF configuration.

Given below is the section of the topology that we will be working on for this activity

![](/images/Service_vEdges_Templates/01_tempf.PNG)

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [Updating the vEdge Service VPN 10 with an OSPF Template](#updating-the-vedge-service-vpn-10-with-an-ospf-template)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Updating the vEdge Service VPN 10 with an OSPF Template

When you link to an external site, like [Jekyll](http://jekyllrb.com), an icon appears after the link. If you want to remove this icon, comment out this style in css/customstyles.css.

```css
/* this part adds an icon after external links, using FontAwesome*/
a[href^="http://"]:after, a[href^="https://"]:after {
    content: "\f08e";
    font-family: FontAwesome;
    font-weight: normal;
    font-style: normal;
    display: inline-block;
    text-decoration: none;
    padding-left: 3px;
}
```

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Updating the vEdge Service VPN 10 with an OSPF Template~~](#updating-the-vedge-service-vpn-10-with-an-ospf-template)
<br/>
- [Activity Verification](#activity-verification)
<br/>
" type="primary" %}

## Activity Verification

Go to the [Font Awesome library](http://fortawesome.github.io/Font-Awesome/icons/) to see the available icons.

The Font Awesome icons allow you to adjust their size by simply adding `fa-2x`, `fa-3x` and so forth as a class to the icon to adjust their size to two times or three times the original size. As vector icons, they scale crisply at any size.

Here's an example of how to scale up a camera icon:

```html
<i class="fa fa-camera-retro"></i> normal size (1x)
<i class="fa fa-camera-retro fa-lg"></i> fa-lg
<i class="fa fa-camera-retro fa-2x"></i> fa-2x
<i class="fa fa-camera-retro fa-3x"></i> fa-3x
<i class="fa fa-camera-retro fa-4x"></i> fa-4x
<i class="fa fa-camera-retro fa-5x"></i> fa-5x
```

Here's what they render to:

<i class="fa fa-camera-retro"></i> 1x
<i class="fa fa-camera-retro fa-lg"></i> fa-lg
<i class="fa fa-camera-retro fa-2x"></i> fa-2x
<i class="fa fa-camera-retro fa-3x"></i> fa-3x
<i class="fa fa-camera-retro fa-4x"></i> fa-4x
<i class="fa fa-camera-retro fa-5x"></i> fa-5x

With Font Awesome, you always use the `i` tag with the appropriate class. You also implement `fa` as a base class first. You can use font awesome icons inside other elements. Here I'm using a Font Awesome class inside a Bootstrap alert:

```html
<div class="alert alert-danger" role="alert"><i class="fa fa-exclamation-circle"></i> <b>Warning: </b>This is a special warning message.
```

Here's the result:

<div class="alert alert-danger" role="alert"><i class="fa fa-exclamation-circle fa-lg"></i> This is a special warning message.</div>

The notes, tips, warnings, etc., are pre-coded with Font Awesome and stored in the alerts.yml file. That file includes the following:

{% raw %}
```yaml
tip: '<div class="alert alert-success" role="alert"><i class="fa fa-check-square-o"></i> <b>Tip: </b>'
note: '<div class="alert alert-info" role="alert"><i class="fa fa-info-circle"></i> <b>Note: </b>'
important: '<div class="alert alert-warning" role="alert"><i class="fa fa-warning"></i> <b>Important: </b>'
warning: '<div class="alert alert-danger" role="alert"><i class="fa fa-exclamation-circle"></i> <b>Warning: </b>'
end: '</div>'

callout_danger: '<div class="bs-callout bs-callout-danger">'
callout_default: '<div class="bs-callout bs-callout-default">'
callout_primary: '<div class="bs-callout bs-callout-primary">'
callout_success: '<div class="bs-callout bs-callout-success">'
callout_info: '<div class="bs-callout bs-callout-info">'
callout_warning: '<div class="bs-callout bs-callout-warning">'

hr_faded: '<hr class="faded"/>'
hr_shaded: '<hr class="shaded"/>'
```
{% endraw %}

This means you can insert a tip, note, warning, or important alert simply by using these tags.


```liquid
{% raw %}{% include note.html content="Add your note here." %}{% endraw %}
```


```liquid
{% raw %}{% include tip.html content="Add your tip here." %}{% endraw %}
```


```liquid
{% raw %}{% include important.html content="Add your important info here." %}{% endraw %}
```


{% raw %}
```liquid
{% include warning.html content="Add your warning here." %}
```
{% endraw %}

Here's the result:

{% include note.html content="Add your note here." %}

{% include tip.html content="Here's my tip." %}

{% include important.html content="This information is very important." %}

{% include warning.html content="If you overlook this, you may die." %}

The color scheme is the default colors from Bootstrap. You can modify the icons or colors as neede

<br/>

{% include callout.html content="**Task List**
<br/><br/>
- [~~Overview~~](#overview)
<br/>
- [~~Updating the vEdge Service VPN 10 with an OSPF Template~~](#updating-the-vedge-service-vpn-10-with-an-ospf-template)
<br/>
- [~~Activity Verification~~](#activity-verification)
<br/>
" type="primary" %}
