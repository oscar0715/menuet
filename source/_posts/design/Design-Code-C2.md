---
title: 《Design Code》 Charpter 2 Learn the tools
tags:
  - Design
categories:
  - Art
  - Design
date: 2016-09-25 13:18:25
---
所有通往圣殿的路一定道阻且长

《Design Code》
Charpter 2 Learn the tools
<!-- more -->

***
# Learn Sketch 3
## Shortcut
{% link sketch shortcut https://designcode.io/sketch-keyboard shortcut %}

## Working With Icons
{% link PixelLove  https://www.pixellove.com/ PixelLove  %}
{% link  Streamline   http://www.streamlineicons.com/  Streamline   %}

后面就是学习工具了………………省略…………

# Master Sketch
1. Scissors: quick delete
2. Border Options
3. Background Blur
4. Working with Vectors.  press v first (mirror / disconnected / straight)
5. insert star / polygon point
6. Duplicate and Transform
    - Make grid
        makes it easy to duplicate anything
    - Perspective Transform
        First, make sure to Convert to Outlines every text layer. 
        ungroup everything
        Finally, select all the layers together and do Transform (Cmd Shift T)
    - Scale Tool (cmd+k)
        For instance, a 1 px border scaled at 200% will be 2 px.
    - 
7.  Alignment, Distances and Guides
    - Smart Guides
    - Distances: hold alt key
    - Align and Distribute Objects
    - Ruler: ctrl+R / hold shift to jump by 10px
    - Layout
    - Grids Ctrl+ G
8.  Color Picker
    - you can switch from RGB to HSB, a more intuitive way to manipulate colors. 
    - Quick Eyedropper： Ctrl+ C
    - Frequently Used Colors
    - Color Palettes
    - Gradients

Designing From Scratch
一个25分钟的视频，我跟了一个半小时

# Working with Vectors
Vectors are composed of points, which can be curved using the Bezier Curve.

## Bézier Curve
{% link Bézier Curve  https://medium.com/sketch-app/mastering-the-bezier-curve-in-sketch-4da8fdf0dbbb#.eagemhej0 Bézier Curve %}
***
r
Vector shapes: collection of points
Bézier curves: create curved lines around those vector points

### How Curves Work
 “cubic-Bézier” is the most commonly used in graphics software — and the only method used in Sketch.

1. Bézier curves are created by *handles* that extend from any *vector point*.
2. At the end of each handle is the handle control point. 

Using Bézier Handles:
1. Mirrored
    both handles are at the same angle and same distance
2. Asymmetric
    at the same angle, not same distance
3. Disconnected
    change each handle independently
4. Straight
    a vector point has no handles

Sketch hot key
- Double-clicking a vector point will alternate it between Straight and Mirrored modes.
- select the next vector point along the path by hitting “tab”
- “Shift” + “tab” will select the previous point along the path.
- Holding down the “Command” (⌘) key while moving a Bézier handle (Disconnected mode)

>Like any skill, creating perfectly-curved vector shapes requires repetition & practice.

A great way to practice these skills is by tracing letterforms. Take a photo or drawing of a beautiful letter (something curved, like a “G”, “Q”, “R”, or anything from a script alphabet), bring that image into Sketch, and use the vector tool to trace the outline of the letterform. Try to position your vector points at the outermost places on the shape, use vertical and horizontal handles, and fine-tune them by manually entering coordinates.



## Other skills
1. Union
2. Subtract
3. Intersect 
4. Difference
5. flatten
6. scissors

# Exporting Assets

## Points Versus Pixels
1. When the iPhone was first introduced, the two units were the same: `1pt equals 1px`.
2. Then when retina screens came along, `1pt became 2px`.
3. Pixels as the real values depending on the pixel density (iPhone 4,5,6 = @2x, iPhone 6 Plus = @3x).
{% img  https://designcode.io/cloud/sketch/Resolutions.jpg 500  Points Versus Pixels  %}

## What To Export?
1. As a rule of thumb, any vector that is more complex than a square should be rendered as an asset: `icons`, `buttons` and `logos`. 
2. Your goal should be to export as little as possible to allow maximum flexibility
3. Text, backgrounds, Navigation Bar, Tab Bar and even dialogs/cards `shouldn’t` be exported since they can be easily coded. 
4. `Drop` shadows, blurred effects can be rebuilt in code as well
5. Pictures and Avatars are part of the content, which just like text, are loaded from a set of data.

## Slices
There are 2 ways to export assets: 
1. Make Exportable and Slice Tool. 
2. Slices are represented by a knife icon.