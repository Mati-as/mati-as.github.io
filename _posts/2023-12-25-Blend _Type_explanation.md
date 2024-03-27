---
layout: single
title:  "blend type Explanation"
categories: shader
tag: Unity
toc: true
sidebar:
    nav: "counts"
---


## the detailed description of blend types:

- Traditional Transparency ("Blend SrcAlpha OneMinusSrcAlpha")

The source color is weighted by its alpha value (SrcAlpha), and the destination color is weighted by the inverse of the source's alpha value (oneMinusSrcAlpha).
This is traditional transparency blending.
It can be used for general UI elements, character transparency effects, windows, or glass effects.

- Premultiplied Transparency ("Blend one OneMinusSrcAlpha")

The source color is applied as is (one), and the destination color is weighted by the inverse of the source's alpha value.
Useful for blending images while preserving their transparency.
Can be used for layering images with various transparencies.

- Additive (Blend One One)

Used in particle systems for light effects, laser beams, flames, magical effects, etc.
Used when light or energy needs to be overlapped and shown brighter.
For example, it's useful for creating explosion effects or the radiance of magic.

- Soft Additive ("Blend OneMinusDstColor One")

Used when you need to create a soft accumulation of light.
Can be applied to smoke, fog, ghost effects, etc.

-Multiplicative (Blend DstColor Zero)

Used for shadows, rendering objects in dark environments, night effects, etc.
Used when an object needs to appear darker than its surroundings.
For example, it can be used to represent objects that appear darker at night.

- 2x Multiplicative (Blend DstColor SrcColor)

Used for more intense shadow effects or dark environment effects.
Used when a darker effect is needed than regular multiplication blending.
For example, it's used to depict thick smoke or dark objects.