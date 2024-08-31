---
layout: page
title: Layer Automation
parent: Scripting Tutorials
permalink: /script-tutorials/layer-automation/
nav_order: 2
---
## Layer Automation

This section explains the basics of layer automation, which is the technique of hiding and showing layers at specific times.

### Basics

This is the basic template for a layer automation function. You can set which layers are visible using the id of the layer and the current index in the timeline. However, it is somewhat inconvenient to access just these properties, so the next few examples will add some helpful boilerplate functions.

```js
window.getLayerVisibleAtTime = (layerId, frame) => {
  return true;
};
```

This first function converts timestamps to index values, so it doesn't have to be calculated by hand for each new conditional.

```js
const timestamp = (min, sec, frame) => minute * 2400 + second * 40 + frame;
```

This second function is for extracting the layer index from the layer id. This is needed because the id passed to the layer function doesn't necessarily correspond to its position in the list of layers. This function is very nice to have when wanting to quickly reference a layer without looking up its id. Although this boilerplate is quite long, a minified version is included in the full template.

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
  ({ simulator: { engine: { engine: { state: {layers} } } } }) => layers,
  (layers) => {
    window.idToIndex = [];

    for (let [i, layer] of [...layers].entries()) {
      window.idToIndex[layer.id] = i
    }
  }
);
```

Here is the full template with boilerplate code included. Now we can move on examples for using this function.

```js
window.subscribe=(e=window.store,t=e=>e,i=()=>{})=>{let s;function n(){let n=t(e.getState());n!==s&&i(s=n)}let r=e.subscribe(n);return n(),r},window.subscribe(window.store,({simulator:{engine:{engine:{state:{layers:e}}}}})=>e,e=>{for(let[t,i]of(window.idToIndex=[],[...e].entries()))window.idToIndex[i.id]=t});

window.getLayerVisibleAtTime = (layerId, frame) => {
  const layerIndex = window.idToIndex[layerId];
  const t = (m, s, f) => m * 2400 + s * 40 + f;

  // Show all layers by default
  return true;
};
```

### Toggling Single Layers

Here is a basic example of using layer automation code. It defaults to hiding the base layer at all times, and hides the second layer after a short time period. Note that in this extended version, we are only defining and returning the cases where layers should be hidden, and in all other cases, layers will be shown by default. This is because of the `return true;` statement at the end.

```js
window.subscribe=(e=window.store,t=e=>e,i=()=>{})=>{let s;function n(){let n=t(e.getState());n!==s&&i(s=n)}let r=e.subscribe(n);return n(),r},window.subscribe(window.store,({simulator:{engine:{engine:{state:{layers:e}}}}})=>e,e=>{for(let[t,i]of(window.idToIndex=[],[...e].entries()))window.idToIndex[i.id]=t});

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
window.subscribe=(e=window.store,t=e=>e,i=()=>{})=>{let s;function n(){let n=t(e.getState());n!==s&&i(s=n)}let r=e.subscribe(n);return n(),r},window.subscribe(window.store,({simulator:{engine:{engine:{state:{layers:e}}}}})=>e,e=>{for(let[t,i]of(window.idToIndex=[],[...e].entries()))window.idToIndex[i.id]=t});

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
window.subscribe=(e=window.store,t=e=>e,i=()=>{})=>{let s;function n(){let n=t(e.getState());n!==s&&i(s=n)}let r=e.subscribe(n);return n(),r},window.subscribe(window.store,({simulator:{engine:{engine:{state:{layers:e}}}}})=>e,e=>{for(let[t,i]of(window.idToIndex=[],[...e].entries()))window.idToIndex[i.id]=t});

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
