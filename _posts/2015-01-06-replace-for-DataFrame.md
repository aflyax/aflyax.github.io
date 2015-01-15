---
layout: post
title: replace( ) for DataFrames
permalink: replace-for-DataFrames
tags: [julia, dataframes, analysis]
---
<img alt="The General Problem" src="http://imgs.xkcd.com/comics/the_general_problem.png" /><br />
In Python/Pandas we have:

<code data-gist-id="29fbce693d198040cd68" data-gist-hide-footer="true" data-gist-hide-line-numbers="true"></code>

Output:

```
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
```

Julia's equivalent (I don't know yet if this function is already implemented):
<br />
<code data-gist-id="23dfcfcedc53e44026f0" data-gist-hide-footer="true" data-gist-hide-line-numbers="true"></code>

The output is the same as above. (Note: apparently, in <code>0.4</code> release, the dictionary syntax will change.)

Unfortunately, if you feed the function an incomplete list, e.g:

``` julia
replace!(dating_df, :opinion, {1 => "bad", 2 => "OK"})
head(dating_df, 3)
```

`...`then you get `key not found: 3`. I can write a `for` loop, but I am trying to think of something more elegant.
