---
title: Software Defined Application Visibility and Control
tags:
  - publishing
keywords: "build scripts, generating outputs, building, publishing"
last_updated: "June 3, 2020"
summary: "Installing and Configuring SD-AVC in a Cisco SD-WAN environment for DPI and First Packet Identification"
sidebar: mydoc_sidebar
permalink: mydoc_sdavc.html
folder: mydoc
---

## Uploading the AVC Image to vManage

The mydoc project has 5 build scripts and a script that runs them all. These scripts will require a bit of detail to configure. Every team member who is publishing on the project should set up their folder structure in the way described here.

## Enabling AVC on vManage and Verification

Your command-line terminal opens up to your user name (for example, `Users/tjohnson`). I like to put all of my projects from repositories into a subfolder under my username called "projects." This makes it easy to get to the projects from the command line. You can vary from the project organization I describe here, but following the pattern I outline will make configuration easier.

To set up your projects:

1. Set up your Jekyll theme in a folder called "docs." All of the source files for every project the team is working on should live in this directory. Most likely you already either downloaded or cloned the jekyll-documentation-theme. Just rename the folder to "docs" and move it into the projects folder as shown here.
2. In the same root directory where the docs folder is, create another directory parallel to docs called doc_outputs. 

   Thus, your folder structure should be something like this:

   ```
   projects
   - docs
   - doc_outputs
   ```

   The docs folder contains the source of all your files, while the doc_outputs contains the site outputs.

## Checking Policy configuration for AVC

For the mydocs project, you'll see a series of build scripts for each project. There are 5 build scripts, described in the following sections. Note that you really only need to run the last one, e.g., mydoc_all.sh, because it runs all of the build scripts. But you have to make sure each script is correctly configured so that they all build successfully.

{% include tip.html content="In the descriptions of the build scripts, \"mydoc\" is used as the sample project. Substitute in whatever your real project name is." %}

## Verification

Here's what this script looks like:

```
echo 'Killing all Jekyll instances'
kill -9 $(ps aux | grep '[j]ekyll' | awk '{print $2}')
clear


echo "Building PDF-friendly HTML site for Mydoc Writers ..."
jekyll serve --detach --config configs/mydoc/config_writers.yml,configs/mydoc/config_writers_pdf.yml
echo "done"

echo "Building PDF-friendly HTML site for Mydoc Designers ..."
jekyll serve --detach --config configs/mydoc/config_designers.yml,configs/mydoc/config_designers_pdf.yml
echo "done"

echo "All done serving up the PDF-friendly sites. Now let's generate the PDF files from these sites."
echo "Now run . mydoc_2_multibuild_pdf.sh"
```

After killing all existing Jekyll instances that may be running, this script serves up a PDF friendly version of the docs (in HTML format) at the destination specified in the configuration file.

Each of your configuration files needs to have a destination like this: `../doc_outputs/mydoc/adtruth-java`. That is, the project should build in the doc_outputs folder, in a subfolder that matches the project name.

The purpose of this script is to make a version of the HTML output that is friendly to the Prince XML PDF generator. This version of the output strips out the sidebar, topnav, and other components to just render a bare-bones HTML representation of the content.

Customize the script with your own PDF configuration file names.

## Sample Policy Configuration (Information Only)
