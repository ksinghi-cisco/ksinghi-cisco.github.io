---
title: Applying Templates to the vSmarts
tags: [single_sourcing]
keywords: vSmart, Templates, control policies
last_updated: May 25, 2020
summary: "Applying Templates to the vSmarts in order to bring them in vManage mode"
sidebar: mydoc_sidebar
permalink: mydoc_Templates_to_vSmarts.html
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Configuring VPN 0 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 0 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 0 Interface Template
    <br/>

- Configuring VPN 512 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 512 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 512 Interface Template
    <br/>
- Attaching vSmarts to the Device Template and Verification

<br/>

" type="primary" %}

## Configuring VPN 0 Templates for vSmarts

### Configuring the main VPN 0 template

You can filter content based on values that you have set either in your page's frontmatter, a config file, or in a file in your \_data folder. If you set the attribute in your config file, you need to restart the Jekyll server to see the changes. If you set the value in a file in your \_data folder or page frontmatter, you don't need to restart the server when you make changes.

### Configuring the VPN 0 Interface Template

Here's an example of conditional logic based on a value in the page's frontmatter. Suppose you have the following in your frontmatter:

```
platform: mac
```

On a page in my site (it can be HTML or markdown), you can conditionalize content using the following:

{% raw %}
```liquid
{% if page.platform == "mac" %}
Here's some info about the Mac.
{% elsif page.platform == "windows" %}
Here's some info about Windows ...
{% endif %}
```
{% endraw %}

This uses simple `if-elsif` logic to determine what is shown (note the spelling of `elsif`). The `else` statement handles all other conditions not handled by the `if` statements.

Here's an example of `if-else` logic inside a list:

{% raw %}
```liquid
To bake a casserole:

1. Gather the ingredients.
{% if page.audience == "writer" %}
2. Add in a pound of meat.
{% elsif page.audience == "designer" %}
3. Add in an extra can of beans.
{% endif %}
3. Bake in oven for 45 min.
```
{% endraw %}

You don't need the `elsif` or `else`. You could just use an `if` (but be sure to close it with `endif`).

## Configuring VPN 512 Templates for vSmarts

### Configuring the main VPN 512 template

You can also use `unless` in your logic, like this:

{% raw %}
```liquid
{% unless site.output == "pdf" %}
...do this
{% endunless %}
```
{% endraw %}

When figuring out this logic, read it like this: "Run the code here *unless* this condition is satisfied."."

Don't read it the other way around or you'll get confused. (It's *not* executing the code only if the condition is satisfied.)

### Configuring the VPN 512 Interface Template

Here's an example of using conditional logic based on a value in a data file:

{% raw %}
```liquid
{% if site.data.options.output == "alpha" %}
show this content...
{% elsif site.data.options.output == "beta" %}
show this content...
{% else %}
this shows if neither of the above two if conditions are met.
{% endif %}
```
{% endraw %}

To use this, I would need to have a \_data folder called options where the `output` property is stored.

## Attaching vSmarts to the Device Template and Verification

You can also specify a `data_source` for your data location in your configuration file. Then you aren't limited to simply using `_data` to store your data files.

For example, suppose you have 2 projects: alpha and beta. You might store all the data files for alpha inside data_alpha, and all the data files for beta inside data_beta.

In your alpha configuration file, specify the data source like this:

```
data_source: data_alpha
```

Then create a folder called \_data_alpha.

Fotr your beta configuration file, specify the data source like this:

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Configuring VPN 0 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 0 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 0 Interface Template
    <br/>

- Configuring VPN 512 Templates for vSmarts
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the main VPN 512 template
    <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring the VPN 512 Interface Template
    <br/>
- Attaching vSmarts to the Device Template and Verification

<br/>

" type="primary" %}
