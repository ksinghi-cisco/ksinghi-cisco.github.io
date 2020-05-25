---
title: Service Side VPN configuration - vEdges
tags: [formatting]
keywords: popovers, tooltips, user interface text, glossaries, definitions
last_updated: May 26, 2020
summary: "Configure the Service Side VPNs for the vEdges at DC, Site 20 and Site 30"
sidebar: mydoc_sidebar
permalink: mydoc_ssv_vedges.html
folder: mydoc
---

## Creating tooltips
Because this theme is built on Bootstrap, you can simply use a specific attribute on an element to insert a tooltip.

Suppose you have a glossary.yml file inside your \_data folder. You could pull in that glossary definition like this:

{% raw %}
```html
<a href="#" data-toggle="tooltip" data-original-title="{{site.data.glossary.jekyll_platform}}">Jekyll</a> is my favorite tool for building websites.
```
{% endraw %}

This renders to the following:

<a href="#" data-toggle="tooltip" data-original-title="{{site.data.glossary.jekyll_platform}}">Jekyll</a> is my favorite tool for building websites.

{% include links.html %}
