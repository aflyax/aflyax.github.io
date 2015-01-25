---
layout: post
title: Use unique( ) to drop duplicates
permalink: unique-Julia
comments: true
tags: [julia, arrays, sorting, duplicates, syntax]
---

In a [recent post](/drop-duplicates/), I reviewed how you can use `union()` to get unique members out of a set. A faster way to do that is to use `unique()` function:

```julia
println([a; b])
println(unique(a))
println(unique(b))
println(unique([a,b]))
println(sort(unique([a,b])))
```

Out:

```
[3 4 8 9 8 2 4 9 2 7 3 3 6 9 4
 5 3 0 0 10 7 6 9 9 2 10 0 5 9 5]
[3,4,8,9,2,7,6]
[5,3,0,10,7,6,9,2]
[3,5,4,8,0,9,10,2,7,6]
[0,2,3,4,5,6,7,8,9,10]
```
