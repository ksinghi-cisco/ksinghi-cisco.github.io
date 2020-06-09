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

## Overview

You can create a glossary for your content. First create your glossary items in a data file such as glossary.yml.

Then create a page and use definition list formatting, like this:

fractious
: {{site.data.glossary.fractious}}

gratuitous
: {{site.data.glossary.gratuitous}}

haughty
: {{site.data.glossary.haughty}}

gratuitous
: {{site.data.glossary.gratuitous}}

impertinent
: {{site.data.glossary.intrepid}}

Here's the code:

```
{% raw %}fractious
: {{site.data.glossary.fractious}}

gratuitous
: {{site.data.glossary.gratuitous}}

haughty
: {{site.data.glossary.haughty}}

gratuitous
: {{site.data.glossary.gratuitous}}

impertinent
: {{site.data.glossary.intrepid}}{% endraw %}
```

The glossary works well as a link in the top navigation bar.

## Horizontally styled definiton lists

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

Although I like the side-by-side view for shorter definitions, I found it problematic with longer definitions.

{% include warning.html content="STOP!!!! Time to take a snapshot. Refer to the Testing Procedure, Step 7 of the SOP" %}
