---
layout: post
title: Fixing WiFi connectivity on Linux
---
![](http://imgs.xkcd.com/comics/zealous_autoconfig.png "included in the latest Ubuntu release")

After I switched to Linux (Mint 17.1), I've been experiencing horrible WiFi connectivity. At Starbucks or a local library, where the connection strength is (apparently) better, I could connect easily, but at home my laptop could not connect at all, while my wife's Windows-running laptop (or my phone) had no problem, e.g., streaming.

Finally I decided to search for my WiFi card:
```bash
lspci
[...]
02:00.0 Network controller: Intel Corporation Centrino Advanced-N 6235 (rev 24)
  
```

Some online search revealed a common problem with this controller that can be fixed with:
> <code>
**$ echo** options iwlwifi 11n_disable=1 | **sudo** tee /etc/modprobe.d/51-disable-6235-11n.conf
</code>

Which worked as a charm. Now I can connect everywhere, even with a weak WiFi signal.