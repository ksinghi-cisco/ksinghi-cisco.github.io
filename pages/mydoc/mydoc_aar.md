---
title: Configuring Application Aware Routing
tags: [special_layouts]
keywords: knowledge base, support portal, grid, doc portal
last_updated: June 3, 2020
summary: "Manipulate the path taken by traffic based on network parameters  like latency, loss and jitter."
sidebar: mydoc_sidebar
permalink: mydoc_aar.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Creating and Activating the AAR Policy
<br/>
- Viewing modified traffic flows and current network statistics
<br/>
- Configuring a Policer to simulate network impairment
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Creating a Policer List
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the IPv4 ACL Policy
    <br/>
- Applying the Policer on the MPLS link
<br/>
- Viewing changed statistics and resultant traffic flows
<br/>

" type="primary" %}

## Overview

Here's the sample knowledge-base style layout:

<div class="row">
         <div class="col-lg-12">
             <h2 class="page-header">Knowledge Base Categories</h2>
         </div>
         <div class="col-md-3 col-sm-6">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary"></i>
                           <i class="fa fa-tree fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body">
                     <h4>Getting started</h4>
                     <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
                     <a href="tag_getting_started.html" class="btn btn-primary">Learn More</a>
                 </div>
             </div>
         </div>
         <div class="col-md-3 col-sm-6">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary"></i>
                           <i class="fa fa-car fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body">
                     <h4>Navigation</h4>
                     <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
                     <a href="tag_navigation.html" class="btn btn-primary">Learn More</a>
                 </div>
             </div>
         </div>
         <div class="col-md-3 col-sm-6">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary"></i>
                           <i class="fa fa-support fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body">
                     <h4>Single sourcing</h4>
                     <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
                     <a href="tag_single_sourcing.html" class="btn btn-primary">Learn More</a>
                 </div>
             </div>
         </div>
         <div class="col-md-3 col-sm-6">
             <div class="panel panel-default text-center">
                 <div class="panel-heading">
                     <span class="fa-stack fa-5x">
                           <i class="fa fa-circle fa-stack-2x text-primary"></i>
                           <i class="fa fa-database fa-stack-1x fa-inverse"></i>
                     </span>
                 </div>
                 <div class="panel-body">
                     <h4>Formatting</h4>
                     <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
                     <a href="tag_formatting.html" class="btn btn-primary">Learn More</a>
                 </div>
             </div>
         </div>
</div>

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Creating and Activating the AAR Policy](#creating-and-activating-the-aar-policy)
<br/>
- [Viewing modified traffic flows and current network statistics](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Creating and Activating the AAR Policy

If you don't want to link to a tag archive index, but instead want to list all pages that have a certain tag, you could use this code:

```html
{% raw %}Getting started pages:
<ul>
{% assign sorted_pages = site.pages | sort: 'title' %}
{% for page in sorted_pages %}
{% for tag in page.tags %}
{% if tag == "getting_started" %}
<li><a href="{{ page.url | remove: "/" }}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>{% endraw %}
```

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [Viewing modified traffic flows and current network statistics](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Viewing modified traffic flows and current network statistics

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Creating a Policer List](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Configuring a Policer to simulate network impairment

### Creating a Policer List

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [Configuring a Policer to simulate network impairment](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring the IPv4 ACL Policy](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

### Configuring the IPv4 ACL Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [Applying the Policer on the MPLS link](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Applying the Policer on the MPLS link

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [~~Applying the Policer on the MPLS link~~](#applying-the-policer-on-the-mpls-link)
<br/>
- [Viewing changed statistics and resultant traffic flows](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}

## Viewing changed statistics and resultant traffic flows

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating and Activating the AAR Policy~~](#creating-and-activating-the-aar-policy)
<br/>
- [~~Viewing modified traffic flows and current network statistics~~](#viewing-modified-traffic-flows-and-current-network-statistics)
<br/>
- [~~Configuring a Policer to simulate network impairment~~](#configuring-a-policer-to-simulate-network-impairment)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Creating a Policer List~~](#creating-a-policer-list)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring the IPv4 ACL Policy~~](#configuring-the-ipv4-acl-policy)
    <br/>
- [~~Applying the Policer on the MPLS link~~](#applying-the-policer-on-the-mpls-link)
<br/>
- [~~Viewing changed statistics and resultant traffic flows~~](#viewing-changed-statistics-and-resultant-traffic-flows)
<br/>

" type="primary" %}
