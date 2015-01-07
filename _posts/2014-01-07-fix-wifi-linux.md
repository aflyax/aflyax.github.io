---
layout: post
title: Fixing WiFi connectivity on Linux
---

After I switched to Linux (Mint 17.1), I've been experiencing horrible WiFi connectivity. At Starbucks or a local library, where the connection strength is (apparently) greater, I could connect to Internet easily, but at home my laptop could not connect at all, while my wife's Windows-running laptop (or my phone) had no problem connecting, streaming, etc.

Finally I decided to search for my WiFi card:
{% highlight %}
lspci <br />
[...] <br />
02:00.0 Network controller: Intel Corporation Centrino Advanced-N 6235 (rev 24)
{% end highlight %}


Some online search revealed a common problem with this controller that can be fixed with:
{% highlight}
$ echo options iwlwifi 11n_disable=1 | sudo tee /etc/modprobe.d/51-disable-6235-11n.conf
{% end highlight}

Which worked as a charm.