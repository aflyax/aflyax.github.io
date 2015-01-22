---
layout: post
title: Adding layers in Gadfly (Julia)
permalink: layers-gadfly
comments: true
tags: [julia, gadfly, plotting]
---
It's not really very clear from the [documentation](http://gadflyjl.org/#layers) of Gadfly package how to add layers to an existing plot, or how to display a plot once a layer has been added. After asking around, I finally figure out how to do it:

``` julia
x = [-π:0.1:π]
y = sin(x)
w = cos(x)

p = plot(x=x, y=y, Geom.point, Geom.line, Theme(default_color=color("seagreen")))
q = layer(x=x, y=w, Geom.line, Theme(default_color=color("indianred")))
append!(p.layers, q);
# draw(SVGJS("sin-cos.svg", 7.5inch, 3.25inch), p)
p
```

The commented-out part will save the image in an SVG format.

<object type="image/svg+xml" data="images\sin-cos.svg"></object>

To display it above in this post, I entered:

``` html
<object type="image/svg+xml" data="images\sin-cos.svg"></object>
```