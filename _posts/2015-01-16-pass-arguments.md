---
layout: post
title: Pass the arguments, please
permalink: pass-arguments
comments: true
tags: [julia, functions, optimization]
---
![](http://imgs.xkcd.com/comics/correlation.png "If correlation doesn't imply causation, then what does?")
According to [Julia documentation](http://julia.readthedocs.org/en/latest/manual/style-guide/)...

>It is [...] worth emphasizing that functions should take arguments, instead of operating directly on global variables (aside from constants like `pi`).

Let's see if there is a difference in runtime depending on whether a function takes arguments, takes globals, or uses internal variables (the latter may not be a very fair comparison in this case).

<!-- more -->

<code data-gist-id="26352b6402d604b7235d" data-gist-hide-footer="true"></code>

Output:

```
devect_global elapsed time: 2.326460502 seconds (808,400,000 bytes allocated, 11.88% gc time)
devect_internal elapsed time: 0.110363588 seconds (86,281,768 bytes allocated, 28.07% gc time)
devect_arg elapsed time: 0.034368194 seconds (5,954,048 bytes allocated)
```

Clear difference not just in the runtime speed, but also in the memory use.
