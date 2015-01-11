---
layout: post
title: replace( ) for DataFrame
permalink: replace-for-DataFrame
---

<div dir="ltr" style="text-align: left;" trbidi="on">
<div dir="ltr" style="text-align: left;" trbidi="on">
<img alt="The General Problem" src="http://imgs.xkcd.com/comics/the_general_problem.png" /><br />
In Python/Pandas we have:

<script src="https://gist.github.com/aflyax/29fbce693d198040cd68.js"></script>
Output:

before: 
|   | icecream | miles    | something | opinion |
|---|----------|----------|-----------|---------|
| 1 | 40920    | 8.326976 | 0.953952  | 3       |
| 2 | 14488    | 7.153469 | 1.673904  | 2       |
| 3 | 26052    | 1.441871 | 0.805124  | 1       |

after:
|   | icecream | miles    | something | opinion |
|---|----------|----------|-----------|---------|
| 1 | 40920    | 8.326976 | 0.953952  | good    |
| 2 | 14488    | 7.153469 | 1.673904  | OK      |
| 3 | 26052    | 1.441871 | 0.805124  | bad     |

Julia's equivalent (I don't know yet if this function is already implemented):
<br />
<script src="https://gist.github.com/aflyax/23dfcfcedc53e44026f0.js"></script>

The output is the same as above. (Note: apparently, in <code>0.4</code> release, the dictionary syntax will change.)

Unfortunately, if you feed the function an incomplete list, e.g:

<div class="highlight highlight-julia"><pre><span class="pl-s3">replace!</span>(dating_df, <span class="pl-c1">:opinion</span>, {<span class="pl-c1">1</span> <span class="pl-k">=</span><span class="pl-k">&gt;</span> <span class="pl-s1"><span class="pl-pds">"</span>bad<span class="pl-pds">"</span></span>, <span class="pl-c1">2</span> <span class="pl-k">=</span><span class="pl-k">&gt;</span> <span class="pl-s1"><span class="pl-pds">"</span>OK<span class="pl-pds">"</span></span>})</pre></div>

``` julia
replace!(dating_df, :opinion, {1 => "bad", 2 => "OK"})
```

`...`then you get `key not found: 3`. I can write a `for` loop, but I am trying to think of something more elegant`...`
