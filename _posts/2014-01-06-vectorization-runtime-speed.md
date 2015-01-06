---
layout: post
title: Runtime speed for vectorized vs. devectorized code in Julia
---

When dealing with arrays, we have two choices: apply a for loop or vectorize an array: apply the desired changes to all members of the array in a single statement. Vectorization famously speeds up R and Python code, which is why using for loops is discouraged for these languages.

From what I read, Julia is more optimized for running for loops:

<blockquote class="twitter-tweet" lang="en"><p>I really like for loops. That&#39;s why I like that Julia makes it easy to write a loop that&#39;s 700x faster than Python: <a href="https://t.co/WjKikqyXTM">https://t.co/WjKikqyXTM</a></p>&mdash; John Myles White (@johnmyleswhite) <a href="https://twitter.com/johnmyleswhite/status/332920041626554369">May 10, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

In fact, in [The Relationship between Vectorized and Devectorized Code](http://goo.gl/UJxkwq), John Myles White points out that vectorized code seems to run slower in Julia:

<table>
  <thead>
    <tr>
      <th>Approach</th>
      <th>Language</th>
      <th>Average Time</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Vectorized</td>
      <td>R</td>
      <td>0.49</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Devectorized</td>
      <td>R</td>
      <td>4.79</td>
    </tr>
    <tr>
      <td>Vectorized</td>
      <td>Julia</td>
      <td>0.24</td>
    </tr>
    <tr>
      <td>Devectorized</td>
      <td>Julia</td>
      <td>0.0035</td>
    </tr>
  </tbody>
</table>

I was curious to see if this still happens in the current stable release (<code>0.3.4</code> at the moment). I wrote three similar functions, each generating a set of random points <code>(x,y)</code> and calculating the distance from each of those points to another random point <code>(a,b)</code>.

Devectorized code:

{% gist 348e753db1e0c954f4d0 %}

Vectorized code:

{% gist 5555251 %}

Vectorized code with <code>DataFrames:</code>

{% gist 5555251 %}