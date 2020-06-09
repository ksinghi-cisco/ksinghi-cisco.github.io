---
title: Configuring a Zone Based Firewall for Guest DIA users
permalink: mydoc_zbf_dia.html
tags: [publishing, single_sourcing, content_types]
keywords: zbf, zone based firewall, dia
last_updated: June 2, 2020
summary: "Implementing a Zone Base Firewall at Site 40 for Guest Direct Internet Access users"
sidebar: mydoc_sidebar
folder: mydoc
---

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- Overview
<br/>
- Setting up Lists
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring Zones
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Configuring an Application List
    <br/>
- Creating a Security Policy
<br/>
- Applying the Policy and Verification
<br/>

" type="primary" %}

## Overview
This process for creating a PDF relies on Prince XML to transform the HTML content into PDF. Prince costs about $500 per license. That might seem like a lot, but if you're creating a PDF, you're probably working for a company that sells a product, so you likely have access to some resources. There's also a free license that prints a small "P" watermark on your title page, so if you're fine with that, great.

The basic approach is to generate a list of all web pages that need to be added to the PDF, and then add leverage Prince to package them up into a PDF. Once you set it up, building a pdf is just a matter of running a couple of commands. Also, creating a PDF this way gives you a lot more control and customization capabilities than with other methods for creating PDFs. If you know CSS, you can entirely customize the output.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Setting up Lists](#setting-up-lists)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring Zones](#configuring-zones)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring an Application List](#configuring-an-application-list)
    <br/>
- [Creating a Security Policy](#creating-a-security-policy)
<br/>
- [Applying the Policy and Verification](#applying-the-policy-and-verification)
<br/>

" type="primary" %}

## Setting up Lists

You can see an example of the finished product here:

<a target="\_blank" class="noCrossRef" href="{{ "pdf/mydoc.pdf"}}"><button type="button" class="btn btn-default" aria-label="Left Align"><span class="glyphicon glyphicon-download-alt" aria-hidden="true"></span> PDF Download</button></a>

To generate the PDF, browse to the theme's directory in your terminal and run this script:

```bash
. pdf-mydoc.sh
```

This builds a PDF for the documentation in the theme. Look in the **pdf** folder for the output, and see the "last generated date" to confirm that you generated the PDF.

To build a PDF for the other sample projects, run these commands:

```bash
. pdf-product1.sh
```

or

```bash
. pdf-product2.sh
```

You can see the details of the script in these files in the theme's root directory. For example, open pdf-mydoc.sh. It contains the following:

```bash
# Note that .sh scripts work only on Mac. If you're on Windows, install Git Bash and use that as your client.

echo 'Kill all Jekyll instances'
kill -9 $(ps aux | grep '[j]ekyll' | awk '{print $2}')
clear

echo "Building PDF-friendly HTML site for Mydoc ...";
bundle exec jekyll serve --detach --config _config.yml,pdfconfigs/config_mydoc_pdf.yml;
echo "done";

echo "Building the PDF ...";
prince --javascript --input-list=_site/pdfconfigs/prince-list.txt -o pdf/mydoc.pdf;

echo "Done. Look in the pdf directory to see if it printed successfully."
```

After stopping all Jekyll instances, we build Jekyll using a special configuration file that specifies a unique stylesheet. The build contains a file (prince-list.txt) that contains a list of all pages to be included in the PDF. We feed this list into a Prince command to build the PDF.

The following sections explain more about the setup.


### Configuring Zones

Download and install [Prince](http://www.princexml.com/doc/installing/).

You can install a fully functional trial version. The only difference is that the title page will have a small Prince PDF watermark.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [Setting up Lists](#setting-up-lists)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Zones~~](#configuring-zones)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [Configuring an Application List](#configuring-an-application-list)
    <br/>
- [Creating a Security Policy](#creating-a-security-policy)
<br/>
- [Applying the Policy and Verification](#applying-the-policy-and-verification)
<br/>

" type="primary" %}

### Configuring an Application List

The PDF configuration file will build on the settings in the regular configuration file but will some additional fields. Here's the configuration file for the mydoc product within this theme. This configuration file is located in the pdfconfigs folder.

```yaml
destination: _site/
url: "http://127.0.0.1:4010"
baseurl: "/mydoc-pdf"
port: 4010
output: pdf
product: mydoc
print_title: Jekyll theme for documentation — mydoc product
print_subtitle: version 5.0
output: pdf
defaults:
  -
    scope:
      path: ""
      type: "pages"
    values:
      layout: "page_print"
      comments: true
      search: true

pdf_sidebar: mydoc_sidebar
```

{% include note.html content="Although you're creating a PDF, you must still build an HTML web target before running Prince. Prince will pull from the HTML files and from the file-list for the TOC." %}

Note that the default page layout specified by this configuration file is `page_print`. This layout strips out all the sections that shouldn't appear in the print PDF, such as the sidebar and top navigation bar.

Also note that there's a `output: pdf` property in case you want to make some of your content unique to PDF output. For example, you could add conditional logic that checks whether `site.output` is `pdf` or `web`. If it's `pdf`, then include information only for the PDF, and so on. If you're using nav tabs, you'll definitely want to create an alternative experience in the PDF.

In the configuration file, customize the values for the `print_title` and `print_subtitle` that you want. These will appear on the title page of the PDF.

We will access this configure file in the PDF generation script.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Setting up Lists~~](#setting-up-lists)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Zones~~](#configuring-zones)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring an Application List~~](#configuring-an-application-list)
    <br/>
- [Creating a Security Policy](#creating-a-security-policy)
<br/>
- [Applying the Policy and Verification](#applying-the-policy-and-verification)
<br/>

" type="primary" %}

## Creating a Security Policy

There are two template pages in the root directory that are critical to the PDF:

* titlepage.html
* tocpage.html

These pages should appear in your sidebar YML file (in this product, mydoc_sidebar.yml):

```yaml
- title:
  output: pdf
  type: frontmatter
  folderitems:
  - title:
    url: /titlepage.html
    output: pdf
    type: frontmatter
  - title:
    url: /tocpage.html
    output: pdf
    type: frontmatter
```

Leave these pages here in your sidebar. (The `output: pdf` property means they won't appear in your online TOC because the conditional logic of the sidebar.html checks whether `web` is equal to `pdf` or not before including the item in the web version of the content.)

The code in the tocpage.html is mostly identical to that of the sidebar.html page. This is essential for Prince to create the page numbers correctly with cross references.

There's another file (in the root directory of the theme) that is critical to the PDF generation process: prince-list.txt. This file simply iterates through the items in your sidebar and creates a list of links. Prince will consume the list of links from prince-list.txt and create a running PDF that contains all of the pages listed, with appropriate cross references and styling for them all.

{% include note.html content="If you have any files that you do not want to appear in the PDF, add <code>output: web</code> (rather than <code>output: pdf</code>) in the list of attributes in your sidebar. The prince-list.txt file that loops through the mydoc_sidebar.yml file to grab the URLs of each page that should appear in the PDF will skip over any items that do not list <code>output: pdf</code> in the item attributes. For example, you might not want your tag archives to appear in the PDF, but you probably will want to list them in the online help navigation." %}

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Setting up Lists~~](#setting-up-lists)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Zones~~](#configuring-zones)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring an Application List~~](#configuring-an-application-list)
    <br/>
- [~~Creating a Security Policy~~](#creating-a-security-policy)
<br/>
- [Applying the Policy and Verification](#applying-the-policy-and-verification)
<br/>

" type="primary" %}

## Applying the Policy and Verification

Open up the css/printstyles.css file and customize what you want for the headers and footers. At the very least, customize the email address (`youremail@domain.com`) that appears in the bottom left.

Exactly how the print style works here is pretty nifty. You don't need to understand the rest of the content in this section unless you want to customize your PDFs to look different from what I've configured. But I'm adding this information here in case you want to understand how to customize the look and feel of the PDF output.

<br/>

{% include callout.html content="**Task List**
<br/><br/>

- [~~Overview~~](#overview)
<br/>
- [~~Setting up Lists~~](#setting-up-lists)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring Zones~~](#configuring-zones)
<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- [~~Configuring an Application List~~](#configuring-an-application-list)
    <br/>
- [~~Creating a Security Policy~~](#creating-a-security-policy)
<br/>
- [~~Applying the Policy and Verification~~](#applying-the-policy-and-verification)
<br/>

" type="primary" %}
