---
layout: page
title: Rider Skin Customization
parent: Scripting Tutorials
permalink: /script-tutorials/rider-skins/
nav_order: 3
---

{: .warning }
Although it is not a prerequisite, this tutorial uses terminology and concepts from the stylesheet language CSS. To learn more about its syntax and applications, a series of tutorials are available [here](https://developer.mozilla.org/en-US/docs/Web/CSS/Tutorials/){:target="_blank"}.

The rider texture is an svg that can be restyled with CSS styling. This tutorial focuses on fill and outline colors, but all styling options for svg segments are available. The `setCustomRiders` function that's used to apply styles to the rider also comes with some helpful properties. Information about using the function can be accessed via the `setCustomRiders.help` property. All of the texture components of the rider can be previewed with `setCustomRiders.parts`, and the initial texturing of the rider can be seen with `setCustomRiders.initial`.

Based on the output of `setCustomRiders.parts`, the rider is made up of different types of shapes, which can all be styled similarly. Some shapes can be accessed by their class using the "." symbol, while others are accessed by their id using the "#" symbol. In terms of shape properties, this guide mainly focuses on `fill` and `stroke` colors, but more properties can be explored at the link noted above.

### Fill Colors

The `fill` property changes the background color of shapes. For example, this code sets the color of the scarf to gray.

```js
setCustomRiders([
  ".scarfOdd { fill: gray; } .scarfEven { fill: gray; }"
])
```

### Outline Colors

The `stroke` property changes the outline color of shapes. For example, this code sets the color of the string to green.

```js
setCustomRiders([
  "#string { stroke: green }"
])
```

### Multiple Riders

For multiple riders, multiple css stylesheets are needed. If there are less stylesheets than riders, the stylesheets get reused in a cycle for extra riders. This means that with a single stylesheet, the same style gets used for all riders. Something else to keep in mind is that the first stylesheet in the array corresponds to the last rider, and subsequent riders in the array are one indexed. This makes the stylesheet array look like the following:

```js
setCustomRiders([
  // Last Rider
  // Rider 1
  // Rider 2
  // ...
])
```

As an example, if the first rider needed a red scarf, the second rider needed a green scarf, and the third rider a blue scarf, the code would be this:

```js
setCustomRiders([
  ".scarfOdd { fill: blue; }",
  ".scarfOdd { fill: red; }",
  ".scarfOdd { fill: green; }",
])
```
