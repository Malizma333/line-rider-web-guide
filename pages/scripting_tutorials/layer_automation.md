---
layout: page
title: Layer Automation
parent: Scripting Tutorials
permalink: /script-tutorials/layer-automation/
nav_order: 2
---
## Layer Automation

This section explains the basics of layer automation, which is the technique of using code to hide and show layers at specific times.

### Basics

This is the basic template for a layer automation function. You can set which layers are visible using the ID of the layer and the current index in the timeline. However, it is somewhat inconvenient to access these properties alone. The next few examples will add helpful boilerplate functions that expose other properties.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  return true;
};
```

This first function converts timestamps to index values, so it doesn't have to be calculated by hand for each new conditional.

```js
const timestamp = (min, sec, frame) => minute * 2400 + second * 40 + frame;
```

This second function is for extracting the layer index from the layer ID by creating an array mapping ids to indices. This is needed because the id passed to the layer function doesn't necessarily correspond to its position in the list of layers. Although this function is quite long, it only needs to be run once to get the array working. You can read more about how this works in the subsection for [state subscriptions]({{ site.baseurl }}{% link pages/scripting_tutorials/misc_examples.md %}/#subscriptions).

```js
window.subscribe = (store = window.store, select = (state) => state, notify = () => {}) => {
  let state;

  function subscription() {
    const update = select(store.getState());
    if (update !== state) {
      state = update;
      notify(state);
    }
  }

  const unsubscribe = store.subscribe(subscription);
  subscription();
  return unsubscribe;
}

window.subscribe(
  window.store,
  (state) => state.simulator.engine.engine.state.layers,
  (layers) => {
    window.idToIndex = [];

    for (let [i, layer] of [...layers].entries()) {
      window.idToIndex[layer.id] = i
    }
  }
);
```

Here is the template with boilerplate code included. Now we can move on to examples for using this function.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  const layerIndex = window.idToIndex[layerId];
  const t = (m, s, f) => m * 2400 + s * 40 + f;

  // Show all layers by default
  return true;
};
```

### Toggling Single Layers

Here is a basic example of using layer automation code. It hides the base layer at all times, hides the second layer after a couple of seconds, and shows all other layers. Note that in this extended version, we are only defining the cases where layers should be hidden, and in all other cases, layers will be shown by default. This is because of the `return true;` statement at the end.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  const layerIndex = window.idToIndex[layerId];
  const t = (m, s, f) => m * 2400 + s * 40 + f;

  // Hide base layer
  if (layerIndex == 0) {
    return false;
  }

  // Hide second layer after two seconds
  if (layerIndex == 2) {
    if (frame > t(0, 2, 0)) {
      return false;
    }
  }

  // Show all other layers by default
  return true;
};
```

### Toggling Layer Groups

We can also toggle groups of layers at a time, in the case that we want to turn off a single drawing comprised of multiple layers.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  const layerIndex = window.idToIndex[layerId];
  const t = (m, s, f) => m * 2400 + s * 40 + f;

  // Turning off layers 3 - 6 after three seconds
  if (3 <= layerIndex && layerIndex <= 6) {
    if (frame > t(0, 3, 0)) {
      return false;
    }
  }

  // Show all other layers by default
  return true;
};
```

### Cycling Layers

To create cycles of layers hiding and showing for animation purposes, we can use the modulo (%) operator to calculate when each layer should hide or show. Below is an example of this.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  const layerIndex = window.idToIndex[layerId];
  const t = (m, s, f) => m * 2400 + s * 40 + f;

  // Creating an animation for the first ten layers (after the base layer) at half speed
  const animStartIndex = 1;
  const animEndIndex = 10;
  const animSpeed = 0.5;
  if (animStartIndex <= layerIndex && layerIndex <= animEndIndex) {
    if (Math.round(frame * animSpeed) % animEndIndex != layerIndex - animStartIndex) {
      return false;
    }
  }

  // Show all other layers by default
  return true;
};
```
