---
layout: page
title: Camera
parent: Basic Tutorials
permalink: /basic-tutorials/camera/
---

This is the camera field, where camera zoom and dimensions can be set. This can be found in the settings tab of the sidebar.

<img alt="The camera settings area" src="{{site.baseurl}}/assets/camera-settings.png" width="200" style="border: 2px solid gray">

The zoom value can used to set the static zoom of the camera during playback. For the viewport field, any viewport width and height is available, but the recommended viewport size to work with is 720p, which is easier to see on smaller monitors and is supported by most video sharing sites. When filling in the camera dimensions, new options appear.

<img alt="The camera settings area with the viewport fields filled in" src="{{site.baseurl}}/assets/camera-settings-loaded.png" width="200" style="border: 2px solid gray">

The `Show Viewport` toggle shows the viewport and camera collision box during playback. The viewport can be affected by zoom levels, camera panning, and rider focus that can be altered by settings and scripts, and the camera collision box can currently only be edited with a script. The `Show Visible Areas` toggle dims parts of the track that are invisible to the camera, which is helpful for removing lines not seen during playback.

These are some of the camera settings found in the riders section of the sidebar settings.

<img alt="The rider settings area with multiple riders" src="{{site.baseurl}}/assets/rider-settings-multi.png" width="200" style="border: 2px solid gray">

The `Playback Camera Focus` checkboxes determine which riders the camera should focus on. If multiple checkboxes are selected, the camera will focus between those riders. This can get overridden by scripts. The `Keep Rider in View While Scrubbing` toggle can enable and disable whether the camera should keep focus on the rider when using the timeline separate from playing the track. The `Editor Camera Focus` option is only available if the keep rider in view toggle is enabled, and determines which rider the editor camera should follow during timeline navigation.