---
layout: page
title: Rider Skin Customization
parent: Scripting Tutorials
permalink: /script-tutorials/rider-skins/
nav_order: 3
---

{: .warning }
Although it is not a prerequisite, this tutorial uses terminology and concepts from the stylesheet language CSS. To learn more about its syntax and applications, a series of tutorials are available <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/Tutorials/" target="_blank">here</a>.

The rider texture is an svg that can be restyled with CSS styling. This tutorial focuses on fill and outline colors, but all styling options for svg segments are available. The `setCustomRiders` function that's used to apply styles to the rider also comes with some helpful properties. Information about using the function can be accessed via the `setCustomRiders.help` property. All of the texture components of the rider can be previewed with `setCustomRiders.parts`, and the initial texturing of the rider can be seen with `setCustomRiders.initial`.

Based on the output of `setCustomRiders.parts`, the rider consists mainly of two different types, which are `rect` and `path`. The two exceptions to this are the eye, which uses a `polygon`, and the top of the hat, which uses a `circle`, but these behave similarly to the `rect` in that they are all basic shapes.

Shapes have properties for fill color and outline color, which are `fill` and `stroke`, respectively. These properties will be used in more detail in the following examples.

### Fill Colors

This sets each riders' scarf to gray.

```js
setCustomRiders([
  ".scarfOdd { fill: gray; } .scarfEven { fill: gray; }"
])
```

### Outline Colors

This changes the outline color of the sled to green.

```js
setCustomRiders([
  ".sled { stroke: green; }"
])
```

### Multiple Riders


