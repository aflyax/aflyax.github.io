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

pi = round(π, 2)
ticks = [-pi, -pi/2, 0, pi/2, pi]

p = plot(x=x, y=y, Geom.point, Geom.line, Theme(default_color=color("seagreen")), Guide.xticks(ticks=ticks))
q = layer(x=x, y=w, Geom.line, Theme(default_color=color("indianred")))
append!(p.layers, q);

draw(SVGJS("sin-cos.svg", 7.5inch, 3.25inch), p)
p
```

The commented-out part will save the image in an SVG format (feel free to interact with the Gadfly image below).

<object type="image/svg+xml" data="images\sin-cos.svg"></object>

To display it above in this post, I entered:

``` html
<object type="image/svg+xml" data="images\sin-cos.svg"></object>
```