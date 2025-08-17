---
title: Section 1 - Youtube example
nav_order: 2
---

# Section 1: Youtube example

Github Pages Jekly Template and Markdown does not support embeding youtube, but you can add this inside the _includes folder. Here are some examples;

Autoplay single video muted:
{% include youtube.html id="dQw4w9WgXcQ" autoplay="1" mute="1" %}


Fixed size autoplay:

THese don't seem to work, playlist is WIP.

{% include youtube.html id="dQw4w9WgXcQ" width="800px" height="450px" autoplay="1" %}

Playlist starting at 30 seconds:
{% include youtube.html playlist="PLiaHhY2iBX9hdHaRr6b7XevZtgZRa1PoU" start="30" %}

{% include youtube.html 
   playlist="PLiaHhY2iBX9hdHaRr6b7XevZtgZRa1PoU"
   start="30"
   autoplay="1"
   mute="1"
   width="800px"
   height="450px" %}

{% include youtube.html playlist="LvsgCdWss4I&list=PLg6pANed2DOem5mckBSfX0nSZeTwjNsCH" start="30" %}

https://www.youtube.com/watch?v=LvsgCdWss4I&list=PLg6pANed2DOem5mckBSfX0nSZeTwjNsCH

