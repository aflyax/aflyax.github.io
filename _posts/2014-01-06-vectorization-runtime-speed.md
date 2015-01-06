---
layout: post
title: Runtime speed for vectorized vs. devectorized code in Julia
---

When dealing with arrays, we have two choices: apply a for loop or vectorize an array: apply the desired changes to all members of the array in a single statement. Vectorization famously speeds up R and Python code, which is why using for loops is discouraged for these languages.

From what I read, Julia is more optimized for running for loops:

<blockquote class="twitter-tweet" lang="en"><p>I really like for loops. That&#39;s why I like that Julia makes it easy to write a loop that&#39;s 700x faster than Python: <a href="https://t.co/WjKikqyXTM">https://t.co/WjKikqyXTM</a></p>&mdash; John Myles White (@johnmyleswhite) <a href="https://twitter.com/johnmyleswhite/status/332920041626554369">May 10, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

