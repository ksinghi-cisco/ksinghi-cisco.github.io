---
title: Implementing Custom Traffic Engineering
tags: [publishing]
keywords: custom traffic engineering, path selection
last_updated: May 29, 2020
summary: "Influencing Path selection and facilitating custom traffic engineering in Cisco SD-WAN"
sidebar: mydoc_sidebar
permalink: mydoc_custom_te.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Deploying a Policy
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Setting up Groups of Interest and Traffic Rules
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Applying and Activating the Policy
    <br/>
- Verification
<br/>

" type="primary" %}

## Overview

The normal way to build the Jekyll site is through the build command:

```
jekyll build
```

To build the site and view it in a live server so that Jekyll rebuilds that site each time you make a change, use the `serve` command:

```
jekyll serve
```

By default, the \_config.yml in the root directory will be used, Jekyll will scan the current directory for files, and the folder `_site` will be used as the output. You can customize these build commands like this:

```
jekyll serve --config configs/myspecialconfig.yml --destination ../doc_outputs
```

Here the `configs/myspecialconfig.yml` file is used instead of `_config.yml`. The destination directory is `../doc_outputs`, which would be one level up from your current directory.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Setting up Groups of Interest and Traffic Rules](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Deploying a Policy

If you have a long build argument and don't want to enter it every time in Jekyll, noting all your configuration details, you can create a shell script and then just run the script. Simply put the build argument into a text file and save it with the .sh extension (for Mac) or .bat extension (for Windows). Then run it like this:

```
. myscript.sh
```

My preference is to add the scripts to profiles in iTerm. See [iTerm Profiles][mydoc_iterm_profiles] for more details.

### Setting up Groups of Interest and Traffic Rules

When you're done with the preview server, press **Ctrl+C** to exit out of it. If you exit iTerm or Terminal without shutting down the server, the next time you build your site, or if you build multiple sites with the same port, you may get a server-already-in-use message.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Deploying a Policy](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Applying and Activating the Policy](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

### Applying and Activating the Policy

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Verification

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Deploying a Policy~~](#deploying-a-policy)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Setting up Groups of Interest and Traffic Rules~~](#setting-up-groups-of-interest-and-traffic-rules)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Applying and Activating the Policy~~](#applying-and-activating-the-policy)
    <br/>
- [~~Verification~~](#verification)
<br/>

" type="primary" %}
