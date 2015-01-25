---
layout: post
title: Use unique( ) to drop duplicates
permalink: unique-Julia
comments: true
tags: [julia, arrays, sorting, duplicates, syntax]
---

In a [recent post](/drop-duplicates/), I reviewed how you can use `union()` to get unique members out of a set. A faster way to do that is to use `unique()` function:

<code data-gist-id="12c238f3da8d2875b3f2" data-gist-hide-footer="true" data-gist-hide-line-numbers="true"></code>

Out:

```
[3 4 8 9 8 2 4 9 2 7 3 3 6 9 4
 5 3 0 0 10 7 6 9 9 2 10 0 5 9 5]
[3,4,8,9,2,7,6]
[5,3,0,10,7,6,9,2]
[3,5,4,8,0,9,10,2,7,6]
[0,2,3,4,5,6,7,8,9,10]
```

Compare times:

``` julia
n = 3
@time [ union(a,b) for i in 1:10^n ]
@time [ unique([a,b]) for i in 1:10^n ];
```

Out:

```
elapsed time: 0.007959665 seconds (2335432 bytes allocated)
elapsed time: 0.002394242 seconds (1983432 bytes allocated)
```
