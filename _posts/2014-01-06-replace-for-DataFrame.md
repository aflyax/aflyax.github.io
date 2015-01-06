---
layout: post
title: replace() for DataFrame
---

<div dir="ltr" style="text-align: left;" trbidi="on">
<div dir="ltr" style="text-align: left;" trbidi="on">
<img alt="The General Problem" src="http://imgs.xkcd.com/comics/the_general_problem.png" /><br />
In Python/Pandas we have:

<script src="https://gist.github.com/aflyax/29fbce693d198040cd68.js"></script>
Output:<br />
<pre style="border-radius: 0px; border: 0px; font-size: inherit; line-height: inherit; padding: 0px; vertical-align: baseline; white-space: pre-wrap; word-break: break-all; word-wrap: break-word;">   miles     games  ice_cream  opinion
0  40920  8.326976   0.953952        3
1  14488  7.153469   1.673904        2
2  26052  1.441871   0.805124        1</pre>
<pre style="border-radius: 0px; border: 0px; font-size: inherit; line-height: inherit; padding: 0px; vertical-align: baseline; white-space: pre-wrap; word-break: break-all; word-wrap: break-word;"></pre>
<pre style="border-radius: 0px; border: 0px; font-size: inherit; line-height: inherit; padding: 0px; vertical-align: baseline; white-space: pre-wrap; word-break: break-all; word-wrap: break-word;">   miles     games  ice_cream   opinion
0  40920  8.326976   0.953952      good
1  14488  7.153469   1.673904        OK
2  26052  1.441871   0.805124       bad</pre>
<div style="color: #222222; font-family: arial, sans-serif; font-size: small;">
</div>
</div>
Julia's equivalent (I don't know yet if this function is already implemented):<br />
<br />
<script src="https://gist.github.com/aflyax/23dfcfcedc53e44026f0.js"></script><br />
The output is the same as above. (Note: apparently, in <code>0.4</code> release, the dictionary syntax will change.)</div>
