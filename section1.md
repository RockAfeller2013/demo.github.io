---
title: Section 1 - Youtube example
nav_order: 2
---

# Section 1: Youtube example

Github Pages Jekly Template and Markdown does not support embeding youtube, but you can add this inside the _includes folder. Here are some examples;

Autoplay single video muted:
{% include youtube.html id="dQw4w9WgXcQ" autoplay="1" mute="1" %}


Fixed size autoplay:
{% include youtube.html id="dQw4w9WgXcQ" width="800px" height="450px" autoplay="1" %}

These don't seem to work, playlist is WIP.


<iframe 
  width="800" 
  height="450" 
  src="https://www.youtube.com/embed/videoseries?list=PLiaHhY2iBX9hdHaRr6b7XevZtgZRa1PoU&start=30&autoplay=1&mute=1" 
  title="YouTube playlist" 
  frameborder="0" 
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
  allowfullscreen>
</iframe>

# WebGL Test

<canvas id="glcanvas" width="400" height="300"></canvas>

{% raw %}
<script>
  const canvas = document.getElementById("glcanvas");
  const gl = canvas.getContext("webgl");

  if (!gl) {
    alert("WebGL not supported");
  } else {
    gl.clearColor(0.0, 0.0, 0.0, 1.0);
    gl.clear(gl.COLOR_BUFFER_BIT);
  }
</script>
{% endraw %}

   start="30"
   autoplay="1"
   mute="1"
   width="800px"
   height="450px" %}

{% include youtube.html playlist="LvsgCdWss4I&list=PLg6pANed2DOem5mckBSfX0nSZeTwjNsCH" start="30" %}

https://www.youtube.com/watch?v=LvsgCdWss4I&list=PLg6pANed2DOem5mckBSfX0nSZeTwjNsCH

