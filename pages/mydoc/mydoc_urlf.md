---
title: Configuring URL Filtering
keywords: atom, text editor,
last_updated: June 3, 2020
summary: "Configuring URL Filtering for DIA Guest Users"
sidebar: mydoc_sidebar
permalink: mydoc_urlf.html
folder: mydoc
---

<br/>
{% include callout.html content="**Task List**
<br/><br/>
- Updating the Security Policy
<br/>
- Verification
<br/>

" type="primary" %}

## Updating the Security Policy

URL Filtering allows networks to block traffic to certain sites by utilizing URL-based policies. It is implemented using the Snort Engine.

1. On the vManage GUI, navigate to **Configuration => Security**. Locate the *Guest-FW-IPS-DIA* policy and click on the three dots next to it. Choose to **Edit** the policy. We will add URL Filtering capabilities to the same policy which we used for IPS deployment

    ![](/images/SD-WAN_Security_URLF/01_sec.PNG)

2. Click on the **URL Filtering** tab and then click on **Add URL Filtering Policy**. Choose **Create New**

    ![](/images/SD-WAN_Security_URLF/02_urlf.PNG)

3. Click on **Target VPNs** and enter a Target VPN of *30*. Click on **Save Changes**

    ![](/images/SD-WAN_Security_URLF/02_zaddtarg30.PNG)

4. Enter *URLF-NoShopping* for the **Policy Name**. Set the **Web Categories** to Block and add *auctions* and *shopping* to the categories. Set the **Web Reputation** to High Risk

    ![](/images/SD-WAN_Security_URLF/03_urlf1.PNG)

5. Specify *This is not allowed!* in the **Content Body** and make sure all the **Alerts** are selected. Click on **Save URL Filtering Policy**

    ![](/images/SD-WAN_Security_URLF/04_urlf2.PNG)

6. Make sure the *URLF-NoShopping* URL Filtering policy shows up and click on **Save Policy Changes**

    ![](/images/SD-WAN_Security_URLF/05_save.PNG)

7. Click on **Next** and choose to **Configure Devices**. You can check the side-by-side configuration if needed, making note of the `web-filter` and `block page-profile` configuration being pushed by vManage. This is our URL-F configuration

    ![](/images/SD-WAN_Security_URLF/06_sbs.PNG)

<br/>
{% include callout.html content="**Task List**
<br/><br/>
- [~~Updating the Security Policy~~](#updating-the-security-policy)
<br/>
- [Verification](#verification)
<br/>

" type="primary" %}

## Verification

Wait for a few minutes before going through the verification steps enumerated below.

1. Log in to the Site40 PC by accessing vCenter (use the bookmark or access 10.2.1.50/ui). Log in using the credentials provided and click on the sdwan-slc/ghi-site40pc-podX. Click on the console icon to open a Web Console. Open an **Incognito window** in Chrome or a **Private Browsing** tab in Mozilla Firefox. Try to access http://www.amazon.com. The page should get blocked, giving the message we had customized

    ![](/images/SD-WAN_Security_URLF/07_pvtbrowse.PNG)

2. Log in to the CLI for **cEdge40** via Putty and issue `show utd engine standard logging events`. This will show us amazon.com being blocked with a category of **shopping** attached to it

    ![](/images/SD-WAN_Security_URLF/shoppingupdate.PNG)

URL Filtering is working as expected in our lab environment.

<br/>
{% include callout.html content="**Task List**
<br/><br/>
- [~~Updating the Security Policy~~](#updating-the-security-policy)
<br/>
- [~~Verification~~](#verification)
<br/>

" type="primary" %}
