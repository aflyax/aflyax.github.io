---
layout: post
title: Create a heatmap of your Google Maps trips
permalink: google-geo
comments: true
tags: [javascript, google, plotting, maps, dataviz]
---

In this post, I am not writing code in Python or Julia (even though it was inspired by [these instructions](http://www.chrisalbon.com/map-your-google-location-history/) that use Python code). I will show how to use online JavaScript-based tool to plot your Google Maps locations in a heatmap form. You can use it even if you don't know how to code at all.

It is called [Location History Visualizer](https://theopolis.me/location-history-visualizer/); the instructions are on the page once you open it. You can either use:

* [Google Takeout](https://www.google.com/settings/takeout) to download your Google Maps location history (uncheck the other boxes), or:
* [Google Location History KML API endpoint](https://maps.google.com/locationhistory/kml?startTime=0&endTime=9000000000000) (much faster but still a bit experimental) — the file downloads automatically

After obtaining either file, just drag it onto the Visualizer page.

The heatmap of your travels (as recorded through Google Maps history) will be displayed. I have no spatial orientation at all, so I use GPS on my phone to go to a local grocery store. Therefore, I have a lot of Google Maps data.

Boston:

![](/images/boston_heatmap.png)

San Antonio:

![](/images/SA_heat_map.png)

It also shows my recent trip from Texas to Chicago:

![](/images/us_travel_heatmap.png)

The controls are on the left bottom. I think this example shows that JS is still the king of data visualization.