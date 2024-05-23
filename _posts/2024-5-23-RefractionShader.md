---
layout: single
title:  "Refraction Shader"
categories: "shader"
tag: [Unity, Shader]
toc: true
sidebar:
    nav: "counts"
---

<img src="/images/Performat.png" width="300" height="1000">


### Enabling Opaque Texture in Unity's Performant Settings
When working with Unity, optimizing performance without sacrificing visual quality is a crucial balance to strike. One key setting that can help in this regard is enabling the opaque texture in Unity's performant settings. In this blog post, I'll walk you through why this setting is important and how to use it effectively, along with some shader graph examples.

Enabling this setting can have a performance impact, but in many cases, the benefits in terms of visual effects outweigh the cost, especially if you're targeting high-end devices or platforms.


<img src="/images/distortion.png" width="300" height="1000">

In this graph, we sample the opaque texture and apply a distortion effect based on a noise texture. This creates a dynamic visual effect that uses the underlying scene texture for refraction.