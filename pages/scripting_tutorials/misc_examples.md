---
layout: page
parent: Scripting Tutorials
permalink: /script-tutorials/misc-examples/
nav_order: 7
---

## Misc Examples

These are some examples of scripts that don't fit into any particular category on their own, but are helpful enough to be documented.

### Globals

The `$Zoom` window constant stores the editor zoom tool parameters. Changing its properties will reflect those changes in the editor. This script demonstrates extending the zoom bounds and decreasing the sensitivity of changes in zoom.

```js
$Zoom.MIN = 0.01
$Zoom.MAX = 128
$Zoom.STRENGTH = 1 + 0.003
```

The `setRemountFactors` window function can be used to change remount constants of the rider, namely the `endurance` and `strength` factors. The following example sets the remount factors to the default parameters.

```js
window.setRemountFactors(2, 0.1)
```

The `downloadPhysicsStats` window function downloads a csv file that describes the rider physics data of each frame up to the current one. The parameter is used to specify the target rider, with the default being zero (the first rider). The physics data included consists of mounted state, speed, direction of movement, angular velocity, and the total strain on all of the bones. This example downloads the csv for the first rider.

```js
window.downloadPhysicsStats(0)
```

### Scripts

This script removes any scenery lines that are outside of the camera bounds and duplicated. It ignores any lines on locked layers.

```js
(function () {
  const hash = (l) => {
    return [`${l.x1},${l.y1},${l.x2},${l.y2}`,`${l.x2},${l.y2},${l.x1},${l.y1}`]
  }

  if (store.getState().camera.playbackDimensions == null) {
    throw new Error('Camera dimensions not defined!')
  }

  console.log("Starting...")
  const start = performance.now()

  const layers = store.getState().simulator.engine.engine.state.layers.buffer
  const {width, height} = store.getState().camera.playbackDimensions
  const track = store.getState().simulator.engine

  const lockedLayers = new Set()
  const preserveIds = new Set()
  const seenEndpoints = new Set()

  for(const layer of layers) {
    if(!layer.editable) {
      lockedLayers.add(layer.id)
    }
  }

  for(let index = 0; index < store.getState().player.maxIndex; index++) {
    const zoom = window.getAutoZoom ? window.getAutoZoom(index) : store.getState().camera.playbackZoom
    const camera = store.getState().camera.playbackFollower.getCamera(track, { zoom, width, height }, index)
    const boundingBox = {
      x: camera.x - 0.5 * width / zoom,
      y: camera.y - 0.5 * height / zoom,
      width: width / zoom,
      height: height / zoom,
    }
    for(const line of track.selectLinesInRect(boundingBox)) {
      preserveIds.add(line.id)
    }
  }

  const newTrack = {
    ...store.getState().trackData,
    layers,
    lines: [],
    duration: store.getState().player.maxIndex
  }

  for(const line of track.linesList.buffer) {
    if(line.type !== 2 || lockedLayers.has(line.layer || 0)) {
      newTrack.lines.push(line)
      continue
    }

    const [lineHash, flippedLineHash] = hash(line)

    if (preserveIds.has(line.id) && !seenEndpoints.has(lineHash)) {
      newTrack.lines.push(line)
      seenEndpoints.add(lineHash)
      seenEndpoints.add(flippedLineHash)
    }
  }

  const link = document.createElement('a');
  link.setAttribute('download', newTrack.label + '.track.json');
  link.href = window.URL.createObjectURL(new Blob([JSON.stringify(newTrack)], {type: 'application/json'}));
  document.body.appendChild(link);
  window.requestAnimationFrame(function () {
    link.dispatchEvent(new MouseEvent('click'));
    document.body.removeChild(link);
  });

  console.log(`Took ${Math.round(performance.now() - start) / 1000}s`)
})();
```

This script sets the camera to the Beta 2 camera.

```js
(function () {
  store.dispatch({
    type: 'SET_PLAYBACK_FOLLOWER_SETTINGS',
    payload: {
      maxZoom: 32,
      area: 0.25,
      pull: 0,
      push: 0,
      roundness: 0,
      squareness: 0
    }
  });
})();
```

This script simulates the loading effect from Beta 2 on the track currently being edited.

```js
(function f() {
  const trackLines = JSON.parse(JSON.stringify(
    window.store.getState().simulator.engine.linesList.buffer
  ));
  const lineIds = [...trackLines.keys()].map(i => i+1);
  const deltaTime = 1;

  window.store.dispatch({
    type: "UPDATE_LINES",
    payload: { linesToRemove: lineIds, initialLoad: false },
    meta: { name: "REMOVE_LINES" }
  });

  for(let i = 0; i < trackLines.length; i++) {
    setTimeout(() => {
      window.store.dispatch({
        type: "UPDATE_LINES",
        payload: { linesToAdd:[trackLines[i]], initialLoad: false },
        meta: { name: "ADD_LINE" }
      });
    }, i*deltaTime);
  }
  setTimeout(() => {
    window.store.dispatch({ type: "COMMIT_TRACK_CHANGES" })
    window.store.dispatch({ type: "REVERT_TRACK_CHANGES" })
  }, trackLines.length*deltaTime);
})();
```
