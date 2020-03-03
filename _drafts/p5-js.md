---
layout: post
title: "p5 js"
date: 2020-01-14
categories: javascript p5
---

# P5 Programming

## Method Overrides

- setup is called once to initialize variables etc
- draw is called to draw the screen

```js
function setup() {
  // put setup code here
}
function draw() {
  // put drawing code here
}
```
## draw Method

The draw method is called each time a frame is display. By default the frame rate is 60/s

## Global variables

The variables declared outside the draw and setup are global. These are created/initialize first and then the setup, draw local variables.