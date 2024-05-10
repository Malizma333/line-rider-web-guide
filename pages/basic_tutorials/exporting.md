---
layout: page
title: Recording
parent: Basic Tutorials
permalink: /basic-tutorials/exporting/
---

## Recording Page

{: .warning }
Currently, recording tracks with audio is not supported. You must add the audio yourself in an editing software or online video editor, or use another method of recording such as OBS.

This is the recording page, which lets you record a track from within the browser. It can be accessed by pressing the export button on the sidebar.

<img alt="The export track window before rendering" src="{{site.baseurl}}/assets/export-page.png" width="450" style="border: 2px solid gray">

The `Start From` setting can be changed to affect where the recording starts from. This is helpful for splicing track recordings. The end of the recording is the max index of the timeline, which can only be changed with scripts or file editing. You can also stop the recording manually while it's exporting. The `Resolution` setting defines the viewport dimensions of the camera, separate from the dimensions in the settings page. The `High Quality` toggle affects how much compression the final recording has. Higher quality exports have less compression.

This is the export page after the render button is pressed and the render finishes.

<img alt="The export track window after rendering" src="{{site.baseurl}}/assets/export-complete.png" width="450" style="border: 2px solid gray">

The `Save` button downloads the recording as an mp4 file, and the `Discard` button discards the recording by returning to the initial export page.
