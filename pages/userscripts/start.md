---
layout: page
title: Getting Started
parent: Userscripts
permalink: /userscripts/start/
nav_order: 1
---

{: .note }
This tutorial is only for downloading userscripts from GitHub repositories. Userscripts hosted on [Greasy Fork](https://greasyfork.org/en) are straightforward enough to download that they don't warrant their own tutorial.

Linerider.com uses userscripts as the primary method of supporting userscripts. The following tutorial will walk through how to set up a userscript manager and install a userscript.

### Downloading Userscripts

1) Install a userscript manager supported by your browser. There are multiple options available, but this guide recommends [Tampermonkey](https://www.tampermonkey.net/). This will allow userscripts to run in linerider.com.

2) Choose any number of userscripts to install. Userscripts can be found at the GitHub repositories listed [here]({{ site.baseurl }}{% link pages/userscripts/repos.md %}). To install userscripts, navigate to their respective github page, click "Raw," then follow the Tampermonkey prompt to install.

<img alt="Install page on GitHub" src="{{site.baseurl}}/assets/mod-install-repo.png" width="650" style="border: 2px solid gray">\
*Step 2.1: Installing from GitHub*

<img alt="Install page on Tampermonkey" src="{{site.baseurl}}/assets/mod-install-page.png" width="650" style="border: 2px solid gray">\
*Step 2.2: Installing in Tampermonkey*

3) Enable userscripts by going to `Settings -> Advanced -> Mods Enabled` and switching them on.

<img alt="Enable mods location in linerider.com" src="{{site.baseurl}}/assets/mod-enable-setting.png" width="650" style="border: 2px solid gray">

4) Developer mode might be required for tampermonkey to work properly. See their [faqs](https://www.tampermonkey.net/faq.php?locale=en#Q209) for steps to enable it per browser.
