---
layout: page
title: Basic Gravity
parent: Scripting Tutorials
permalink: /script-tutorials/basic-gravity/
nav_order: 5
---

## Gravity Scripts

{: .warning }
Gravity-related code tends to use hacky methods of accessing the physics simulation, occasionally causing buggy behavior and crashes. Be warned when using some of the following script examples.

{: .note }
The y direction in the track physics simulator is inverted, so positive y points down and vice-versa.

The following section relates to basic gravity scripting techniques. For more advanced examples, see [advanced gravity scripts]({{ site.baseurl }}{% link pages/scripting_tutorials/advanced_gravity.md %}).

### Setting Gravity

The overall gravity of a track can be set using the `$ENGINE_PARAMS` window property. This gravity value applies to all riders in the track. This property should be set *before the track runs*. The following code sample sets the gravity to zero.

```js
window.$ENGINE_PARAMS.gravity = {x: 0, y: 0};
```

To get around having to run the gravity code before starting the track, the following code is used to reset the physics and camera caches. This code is especially useful for scripts relying on changing gravity, and is included in all other script samples.

```js
window.store.getState().camera.playbackFollower._frames.length = 0;
window.store.getState().simulator.engine.engine._computed._frames.length = 1;
```

### Changing Gravity

Sometimes, it's helpful to have different gravity for different sections of track. The following code is an example of defining gravity values for ranges of indices.

```js
// Cache reset code
window.store.getState().camera.playbackFollower._frames.length = 0;
window.store.getState().simulator.engine.engine._computed._frames.length = 1;

// Gravity definition function
Object.defineProperty(window.$ENGINE_PARAMS, "gravity", { get() {
  // Get simulator index while track is playing
  const index = store.getState().simulator.engine.engine._computed._frames.length;

  // Checking index range from 00:00 to 03:00
  if (index <= 3 * 40) {
    return {x: 0, y: 0}; // Zero gravity
  }

  // Checking index range from 03:00 to 04:00
  if (3 * 40 <= index && index <= 4 * 40) {
    return {x: 0, y: -0.175}; // Negative gravity
  }

  // Returns default gravity for all indices not included above
  return {x: 0, y: 0.175};
}});
```