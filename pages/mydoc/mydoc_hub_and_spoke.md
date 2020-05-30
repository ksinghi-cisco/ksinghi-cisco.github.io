---
title: Configuring a Hub and Spoke topology
tags:
  - navigation
keywords: Control policies, hub and spoke, single vpn
last_updated: "May 26, 2020"
summary: "Moving the SD-WAN topology from the default of full mesh to a Hub and Spoke for a particular VPN while leaving the other VPNs in full mesh."
sidebar: mydoc_sidebar
permalink: mydoc_hub_and_spoke.html
folder: mydoc
---

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Creating a DC VPN 20 Feature Template
<br/>
- Creating the Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring Network Constructs
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Adding a Custom Control Policy
    <br/>
- Activity Verification
<br/>

" type="primary" %}

## Overview

If you're using the doc as code approach, you might also consider using the same techniques for reviewing the doc as people use in reviewing code. This approach will involve using Github to edit the files.

There's an Edit me button on each page on this theme. This button allows collaborators to edit the content on Github.

Here's the code for that button on the page.html layout for GitHub:


```
{% raw %}{% if site.github_editme_path %}

<a target="_blank" rel="noopener" href="https://github.com/{{site.github_editme_path}}/{{page.folder}}{{page.url | append: ".md"}}{% endif %}" class="btn btn-default githubEditButton" role="button"><i class="fa fa-github fa-lg"></i> Edit me</a>

{% endif %}{% endraw %}
```

and here for GitLab:


```
{% raw %}{% if site.gitlab_editme_path %}

<a target="_blank" rel="noopener" href="https://github.com/{{site.gitlab_editme_path}}/{{page.folder}}{{page.url | append: ".md"}}{% endif %}" class="btn btn-default githubEditButton" role="button"><i class="fa fa-gitlab fa-lg"></i> Edit me</a>

{% endif %}{% endraw %}
```

In your configuration file, edit the value for `github_editme_path` (or for Gitlab: `gitlab_editme_path`). For example, you might create a branch called "reviews" on your Github repo. Then you would add something like this in your configuration file for the 'github_editme_path': tomjoht/documentation-theme-jekyll/edit/reviews. Here "tomjoht" is my github account name. The repo name is "documentation-theme-jekyll". The "reviews" name is the branch.

To suppress this button, comment out the `github_editme_path` in the \_config.yml file.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Creating a DC VPN 20 Feature Template](#creating-a-dc-vpn-20-feature-template)
<br/>
- [Creating the Policy](#creating-the-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring Network Constructs](#configuring-network-constructs)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Adding a Custom Control Policy](#adding-a-custom-control-policy)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Creating a DC VPN 20 Feature Template

If you want people to collaborate on your project so that their edits get committed to a branch on your project, you need to add them as collaborators. For your Github repo, click **Settings** and add the collaborators on the Collaborators tab using their Github usernames.

If you don't want to allow anyone to commit to your Github branch, don't add the reviewers as collaborators. When someone makes an edit, Github will fork the theme. The person's edit then will appear as a pull request to your repo. You can then choose to merge the change indicated in the pull or not.

{% include note.html content="When you process pull requests, you have to accept everything or nothing. You can't pick and choose which changes you'll merge. Therefore you'll probably want to edit the branch you're planning to merge or ask the contributor to make some changes to the fork before processing the pull request." %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating a DC VPN 20 Feature Template~~](#creating-a-dc-vpn-20-feature-template)
<br/>
- [Creating the Policy](#creating-the-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring Network Constructs](#configuring-network-constructs)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Adding a Custom Control Policy](#adding-a-custom-control-policy)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

## Creating the Policy

Users will make edits in your "reviews" branch (or whatever you want to call it). You can then commit those edits as you make updates.

When you're finished making all updates in the branch, you can merge the branch into the master.

Note that if you're making updates online, those updates will be out of sync with any local edits.

{% include warning.html content="Don't make edits both online using Github's browser-based interface AND offline on your local machine using your local tools. When you try to push from your local, you'll likely get a merge conflict error. Instead, make sure you do a pull and update on your local after making any edits online." %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating a DC VPN 20 Feature Template~~](#creating-a-dc-vpn-20-feature-template)
<br/>
- [Creating the Policy](#creating-the-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring Network Constructs](#configuring-network-constructs)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Adding a Custom Control Policy](#adding-a-custom-control-policy)
    <br/>
- [Activity Verification](#activity-verification)
<br/>

" type="primary" %}

### Configuring Network Constructs

 Prose.io is an overlay on Github that would allow people to make comments in an easier interface. If you simply go to [prose.io](http://prose.io), it asks to authorize your Github account, and so it will read files directly from Github but in the Prose.io interface.

 <br/>

 {% include callout.html content="**Task List**
 <br/><br/>

 - [~~Overview~~](#overview)
 <br/>
 - [~~Creating a DC VPN 20 Feature Template~~](#creating-a-dc-vpn-20-feature-template)
 <br/>
 - [Creating the Policy](#creating-the-policy)
 <br/>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Network Constructs~~](#configuring-network-constructs)
     <br/>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Adding a Custom Control Policy](#adding-a-custom-control-policy)
     <br/>
 - [Activity Verification](#activity-verification)
 <br/>

 " type="primary" %}

### Adding a Custom Control Policy

fdsfsfdksfljasjdklfjskljsalkjflasjfdkn
fdjkslfjkldsjfkls

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Creating a DC VPN 20 Feature Template~~](#creating-a-dc-vpn-20-feature-template)
<br/>
- [~~Creating the Policy~~](#creating-the-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Network Constructs~~](#configuring-network-constructs)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Adding a Custom Control Policy~~](#adding-a-custom-control-policy)
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
- [~~Creating a DC VPN 20 Feature Template~~](#creating-a-dc-vpn-20-feature-template)
<br/>
- [~~Creating the Policy~~](#creating-the-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Network Constructs~~](#configuring-network-constructs)
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Adding a Custom Control Policy~~](#adding-a-custom-control-policy)
    <br/>
- [~~Activity Verification~~](#activity-verification)
<br/>

" type="primary" %}
