---
layout: post
title: Runtime speed for vectorized vs. devectorized code in Julia
comments: true
permalink: de_vectorization-runtime
tags: [julia, optimization, arrays, dataframes, loops]
---

When dealing with arrays, we have two choices: apply a `for` loop or vectorize an array: apply the desired changes to all members of the array in a single statement. Vectorization famously speeds up R and Python code, which is why using `for` loops is discouraged for these languages.

From what I read, Julia is more optimized for running `for` loops. For example, according to John Myles White, the author of `DataFrames` library:

<blockquote class="twitter-tweet" lang="en"><p>I really like for loops. That&#39;s why I like that Julia makes it easy to write a loop that&#39;s 700x faster than Python: <a href="https://t.co/WjKikqyXTM">https://t.co/WjKikqyXTM</a></p>&mdash; John Myles White (@johnmyleswhite) <a href="https://twitter.com/johnmyleswhite/status/332920041626554369">May 10, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<!-- more -->

In fact, in [The Relationship between Vectorized and Devectorized Code](http://www.johnmyleswhite.com/notebook/2013/12/22/the-relationship-between-vectorized-and-devectorized-code/), he points out that vectorized code seems to run slower in Julia:

| time(approach/language)   | R    | Julia  |
|--------------|------|--------|
| Vectorized   | 0.49 | 0.24   |
| Devectorized | 4.79 | 0.0035 |

I was curious to see if this still happens in the current stable release (`0.3.4` at the moment). I wrote three similar functions, each generating a set of random points <code>(x,y)</code> and calculating the distance from each of those points to another random point <code>(a,b)</code>.

<code data-gist-id="d90e477f4a371d53c395" data-gist-hide-footer="true"></code>

Results:

<code>**devect():**<br />
elapsed time: 5.863229397 seconds. 1918832240 bytes allocated, 30.58% gc time<br />

**vect():**
elapsed time: 0.079824481 seconds. 45366656 bytes allocated, 55.56% gc time

**df_vect():**
elapsed time: 0.087366592 seconds. 38463360 bytes allocated, 51.15% gc time</code>

We can see that both of the vectorized functions perform faster than the devectorized function, with the function relying on `DataFrames` performing only a little slower than the one using arrays.

**Update:** It was pointed out to me that I am growing my twins incorrectly: I am basically re-creating the variable `twins` every time anew (a bad practice I inherited from Matlab). I also realized I don't need to grow `dist` at all; I just need to compare it once to `0.05`. Rewriting the function:

``` julia
function devect(x::Array{Float64}, y::Array{Float64}, a::Float64, b::Float64)
    dist = Array(Float64, 0)
    twins_x = Array(Float64, 0)
    twins_y = Array(Float64, 0)
    
    for i in 1:500
        dist = sqrt((x[i] - a)^2 + (y[i] - b)^2)
        if dist < 0.05
            push!(twins_x, x[i])
            push!(twins_y, y[i])
        end
    end
    
    return [twins_x twins_y]
end
```

Result for three functions:

```
elapsed time: 0.015929117 seconds (8605024 bytes allocated)
elapsed time: 0.06146292 seconds (37263752 bytes allocated, 52.91% gc time)
elapsed time: 0.075607938 seconds (30415860 bytes allocated, 41.40% gc time)
```

Amazing! The devectorized version is now a few times quicker than the vectorized ones.

Something I heard at a talk today: from Python and R, we inherit the habit of searching through the documentation to find the "right" function for what we are trying to do, because its authors likely optimized it. In Julia, you don't have to do that: you can just write your own `for` loop, and it will be (almost) as fast as the "optimized version" (assuming you do it right, without silly mistakes like above).

This is great if you're just learning statistics and machine learning using Julia as a medium, because you can write your own functions and modules from scratch, instead of learning black-box packages (e.g., scikit-learn), in which you plug in your datasets. (For instance, one could listen to Andrew Ng's Coursera lectures on Machine Learning and translate them to Julia from Octave or go through *Machine Learning in Action* and translate it from Python.)
