---
layout: page
title: Advanced Gravity
parent: Scripting Tutorials
permalink: /script-tutorials/advanced-gravity/
nav_order: 6
---

## Advanced Gravity Scripts

{: .warning }
Gravity-related code tends to use hacky methods of accessing the physics simulation, occasionally causing buggy behavior and crashes. Be warned when using some of the following script examples.

{: .note }
The y direction in the track physics simulator is inverted, so positive y points down and vice-versa.

The following section relates to some more advanced gravity scripting techniques, which have more complex code examples. These examples can also be used for specific sections of track, similar to the changing gravity example found in the [basic]({{ site.baseurl }}{% link pages/scripting_tutorials/basic_gravity.md %}) section.

### Circular/Relative Gravity

Circular gravity is an effect that causes the rider to fall relative to where the sled is facing. For example, if the sled is upright, gravity points in the normal downwards direction. But if the rider is sideways such that the bottom of the sled is pointing left, then gravity will also point left. The following code achieves this effect.

```js
(function() {
    window.store.getState().camera.playbackFollower._frames.length = 0;
    window.store.getState().simulator.engine.engine._computed._frames.length = 1;
    const currentIndex = store.getState().player.index;
    store.dispatch({type: "SET_PLAYER_INDEX", payload: 0});
    requestAnimationFrame(() => store.dispatch({type: "SET_PLAYER_INDEX", payload: currentIndex}));
})();

Object.defineProperty(window.$ENGINE_PARAMS, "gravity", {  get() {
  // Retrieving data about the current rider
  const index = store.getState().simulator.engine.engine._computed._frames.length;
  const frameData = store.getState().simulator.engine.engine.getFrame(index-1);
  const riderData = frameData.snapshot.entities[0].entities[0];
  const tailContactPoint = riderData.points[1];
  const noseContactPoint = riderData.points[2];

  // Calculating angle perpendicular to the sled
  const angle = Math.atan2(tail.pos.y - nose.pos.y, tail.pos.x - nose.pos.x) - Math.PI/2;

  // Returns gravity vector pointing perpendicular away from sled
  return {
    x: 0.175 * Math.cos(angle),
    y: 0.175 * Math.sin(angle)
  };
}});
```

### Teleportation

We can utilize gravity to achieve movement that is more instantaneous and precise than we could otherwise achieve with lines. Gravity can be used to teleport the rider to a precise location with some simple physics calculations. The following code is an example of this.

```js
(function() {
    window.store.getState().camera.playbackFollower._frames.length = 0;
    window.store.getState().simulator.engine.engine._computed._frames.length = 1;
    const currentIndex = store.getState().player.index;
    store.dispatch({type: "SET_PLAYER_INDEX", payload: 0});
    requestAnimationFrame(() => store.dispatch({type: "SET_PLAYER_INDEX", payload: currentIndex}));
})();

Object.defineProperty(window.$ENGINE_PARAMS, "gravity", { get() {
  // Define position to teleport to and index of teleportation
  const targetIndex = 40;
  const targetPos = {x: 100, y: -100};

  // Get reference point for teleporting rider
  const index = store.getState().simulator.engine.engine._computed._frames.length;
  const frameData = store.getState().simulator.engine.engine.getFrame(index-1);
  const riderData = frameData.snapshot.entities[0].entities[0];
  const pegContactPoint = riderData.points[0];

  if (index == targetIndex + 1) {
    return {x: -pegContactPoint.vel.x, y: -pegContactPoint.vel.y};
  }

  if (index == targetIndex) {
    const accelX = -targetPos.x + pegContactPoint.pos.x + pegContactPoint.vel.x;
    const accelY = -targetPos.y + pegContactPoint.pos.y + pegContactPoint.vel.y;
    return {x: -accelX, y: -accelY};
  }
  
  // Returns default gravity for non-teleportation frame
  return {x: 0, y: 0.175};
}});
```

### Multirider Gravity

Because of the way gravity checks work, each rider is iteratively processed in seventeen sub-frames, one at a time. This means we can check for which rider is being processed and return a different gravity value accordingly. The following is an example of this.

```js
(function() {
    window.store.getState().camera.playbackFollower._frames.length = 0;
    window.store.getState().simulator.engine.engine._computed._frames.length = 1;
    const currentIndex = store.getState().player.index;
    store.dispatch({type: "SET_PLAYER_INDEX", payload: 0});
    requestAnimationFrame(() => store.dispatch({type: "SET_PLAYER_INDEX", payload: currentIndex}));
})();

// Global variable to keep track of which rider we're on
const GRAVITY_CONSTANTS = {iterationCounter: 0};

Object.defineProperty(window.$ENGINE_PARAMS, "gravity", { get() {
  // Calculating the current rider
  GRAVITY_CONSTANTS.iterationCounter += 1;
  const numRiders = store.getState().simulator.engine.engine.state.riders.length;
  const currentRider = 1 + Math.floor(GRAVITY_CONSTANTS.iterationCounter / 17) % numRiders;

  // Return zero gravity for second rider
  if (currentRider == 2) {
    return {x: 0, y: 0};
  }

  // Returns default gravity for other riders
  return {x: 0, y: 0.175};
}});
```

### Platform Controller

We can integrate gravity code with keyboard listeners to produce interactive movement while the track is playing. Below is an example of a platformer script that accelerates the rider left or right based on the **A** and **D** keys, respectively.

```js
// Cache reset code
(function() {
    window.store.getState().camera.playbackFollower._frames.length = 0;
    window.store.getState().simulator.engine.engine._computed._frames.length = 1;
    const currentIndex = store.getState().player.index;
    store.dispatch({type: "SET_PLAYER_INDEX", payload: 0});
    requestAnimationFrame(() => store.dispatch({type: "SET_PLAYER_INDEX", payload: currentIndex}));
})();

// Global variable to keep track of which key is being pressed
const GRAVITY_CONSTANTS = {aKeyPressed: false, dKeyPressed: false};

// Keyboard listeners for detecting when keys are pressed and let go
window.addEventListener('keydown', (e) => {
  if(e.key == 'a') GRAVITY_CONSTANTS.aKeyPressed = true;
  if(e.key == 'd') GRAVITY_CONSTANTS.dKeyPressed = true;
}, false);

window.addEventListener('keyup', (e) => {
  if(e.key == 'a') GRAVITY_CONSTANTS.aKeyPressed = false;
  if(e.key == 'd') GRAVITY_CONSTANTS.dKeyPressed = false;
}, false);

// Gravity definition function
Object.defineProperty(window.$ENGINE_PARAMS, "gravity", { get() {
  // Retrieving data about the current rider
  const index = store.getState().simulator.engine.engine._computed._frames.length;
  const frameData = store.getState().simulator.engine.engine.getFrame(index-1);
  const riderData = frameData.snapshot.entities[0].entities[0];
  const tailContactPoint = riderData.points[1];
  const noseContactPoint = riderData.points[2];

  // Calculating angle gravity should be applied
  const angle = Math.atan2(tailContactPoint.pos.y - noseContactPoint.pos.y, tailContactPoint.pos.x - noseContactPoint.pos.x);
  const gravity = {x: 0, y: 0.175};

  // Alter gravity based on key presses
  if (GRAVITY_CONSTANTS.aKeyPressed) {
    gravity.x += 0.1 * Math.cos(angle);
    gravity.y += 0.1 * Math.sin(angle);
  }

  if (GRAVITY_CONSTANTS.dKeyPressed) {
    gravity.x -= 0.1 * Math.cos(angle);
    gravity.y -= 0.1 * Math.sin(angle);
  }

  return gravity;
}});
```
