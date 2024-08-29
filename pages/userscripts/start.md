---
layout: page
title: Getting Started
parent: Mods
permalink: /userscripts/start/
nav_order: 1
---

Linerider.com uses userscripts as the primary method of supporting mods. The following tutorial will walk through how to set up a userscript manager and install userscript mods.

### Downloading Userscripts

1) Install a userscript manager supported by your browser. There are multiple options available, but this guide recommends [Tampermonkey](https://www.tampermonkey.net/). This will allow mods to run during a linerider.com browser instance.

2) Install a mod loader. This will be responsible for registering each mod you install and providing UI to interact with them. Here are the two options for mod loaders:
- [Emergent Studios'](https://github.com/EmergentStudios/linerider-userscript-mods/blob/master/custom-tools-api.user.js)
- [Malizma's](https://github.com/Malizma333/linerider-userscript-mods/blob/master/line-rider-improved-api.user.js)

<img alt="Install page on GitHub" src="{{site.baseurl}}/assets/mod-install-repo.png" width="650" style="border: 2px solid gray">\
*Installing from GitHub*

<img alt="Install page on Tampermonkey" src="{{site.baseurl}}/assets/mod-install-page.png" width="650" style="border: 2px solid gray">\
*Installing in Tampermonkey*

3) Choose any number of mods to install. Mods can be found at the GitHub repositories listed [here]({{ site.baseurl }}{% link pages/userscripts/repos.md %}).
