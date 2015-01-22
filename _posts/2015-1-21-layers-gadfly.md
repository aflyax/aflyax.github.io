---
layout: post
title: Adding layers in Gadfly (Julia)
permalink: layers-gadfly
comments: true
tags: [julia, gadfly, plotting]
---
It's not really very clear from the [documentation](http://gadflyjl.org/#layers) of Gadfly package how to add layers to an existing plot outside of the first `plot` statement, or how to display a plot once a layer has been added. After asking around, I finally figured out how to do it:

<!-- more -->

``` julia
# tested in IJulia notebook

using Gadfly
# the package is slow the first time you load it AND the first time you plot

x = [-π:0.1:π]
y = sin(x)
w = cos(x)

pi = round(π, 2)
ticks = [-pi:pi/2:pi]

p = plot(x=x, y=y, Geom.point, Geom.line, Theme(default_color=color("seagreen")),
            Guide.xticks(ticks=ticks))
q = layer(x=x, y=w, Geom.line, Theme(default_color=color("indianred")))
append!(p.layers, q);

# draw(SVGJS("sin-cos.svg", 7.5inch, 3.25inch), p)
display(p)    # works as a side effect! (unlike `plot(...)` or just `p`)
2+2
```

The commented-out `draw` statement will save the image in an SVG format (feel free to interact with the Gadfly plot below).

<object type="image/svg+xml" data="\images\sin-cos.svg"></object>

To embed it above in this post, I entered:

``` html
<object type="image/svg+xml" data="\images\sin-cos.svg"></object>
```
