---
layout: post
title: Runtime speed for vectorized vs. devectorized code in Julia
permalink: de_vectorization-runtime
---

When dealing with arrays, we have two choices: apply a `for` loop or vectorize an array: apply the desired changes to all members of the array in a single statement. Vectorization famously speeds up R and Python code, which is why using `for` loops is discouraged for these languages.

From what I read, Julia is more optimized for running `for` loops. For example, according to John Myles White:

<blockquote class="twitter-tweet" lang="en"><p>I really like for loops. That&#39;s why I like that Julia makes it easy to write a loop that&#39;s 700x faster than Python: <a href="https://t.co/WjKikqyXTM">https://t.co/WjKikqyXTM</a></p>&mdash; John Myles White (@johnmyleswhite) <a href="https://twitter.com/johnmyleswhite/status/332920041626554369">May 10, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

In fact, in [The Relationship between Vectorized and Devectorized Code](http://www.johnmyleswhite.com/notebook/2013/12/22/the-relationship-between-vectorized-and-devectorized-code/), he points out that vectorized code seems to run slower in Julia:

| time(approach/language)   | R    | Julia  |
|--------------|------|--------|
| Vectorized   | 0.49 | 0.24   |
| Devectorized | 4.79 | 0.0035 |

I was curious to see if this still happens in the current stable release (`0.3.4` at the moment). I wrote three similar functions, each generating a set of random points <code>(x,y)</code> and calculating the distance from each of those points to another random point <code>(a,b)</code>.

Devectorized code:

{% gist 348e753db1e0c954f4d0 %}

``` julia
function devect()
    x = rand(500)
    y = rand(500)
    a = rand()
    b = rand()
    d = Array(Float64, 0)
    twins = Array(Float64, 0,2)
    
    for i in 1:500
        d = [d; sqrt((x[i] - a)^2 + (y[i] - b)^2)]
        if d[end] < 0.05
            twins = [twins; [x y][end,:]]
        end
    end
    
    return twins
end
```

{% highlight julia %}
function devect()
    x = rand(500)
    y = rand(500)
    a = rand()
    b = rand()
    d = Array(Float64, 0)
    twins = Array(Float64, 0,2)
    
    for i in 1:500
        d = [d; sqrt((x[i] - a)^2 + (y[i] - b)^2)]
        if d[end] < 0.05
            twins = [twins; [x y][end,:]]
        end
    end
    
    return twins
end
{% endhighlight %}


Vectorized code:

{% gist 79ce64c4117ea31f6221 %}

Vectorized code with <code>DataFrames:</code>

{% gist 3fdfdffe84c748e8056d %}

Finally, check each function's time:

{% gist 6b8fd8ebd67cb7b577c2 %}

Results:

<code>**devect():**<br />
elapsed time: 5.863229397 seconds. 1918832240 bytes allocated, 30.58% gc time<br />

**vect():**
elapsed time: 0.079824481 seconds. 45366656 bytes allocated, 55.56% gc time

**df_vect():**
elapsed time: 0.087366592 seconds. 38463360 bytes allocated, 51.15% gc time</code>

We can see that both of the vectorized functions perform faster than the devectorized function, with the function relying on `DataFrames` performing only a little slower than the one using arrays.

**Update:** Using `@inbounds` and `@simd` doesn't seem to help much.
