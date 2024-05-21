---
layout: page
title: Time Remapping
parent: Scripting Tutorials
permalink: /script-tutorials/time-remap/
nav_order: 4
---

Time remapping is used to slow down or speed up the playback speed while the track is playing. It is recommended to use smooth playback, as set frame rates have unstable behavior.

### Basic

This slows down the playback to 50% speed at 1 second, then speeds up playback back to 100% at 2 seconds. In normal time, this is the same as slow motion for 2 seconds (1 second / 0.5 speed).

```js
timeRemapper = createTimeRemapper([
  [0, 1],
  [[1,0], 1/2],
  [[2,0], 1],
])
```

### Interpolate

You can also interpolate between speeds for slowing down and speeding up. This example is similar to the one above, but uses a jump to remove the interpolation effect at the end.

```js
timeRemapper = createTimeRemapper([
  [0, 1],
  [[1,0], 1/2],
  [[2,0], 1/2],
  [[2,0], 1], // No interpolation from 1-2 seconds
], true)
```
