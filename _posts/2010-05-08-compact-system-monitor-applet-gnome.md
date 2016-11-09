---
title: Compact System Monitor Applet for Gnome
tags: [system monitor, panel applet, gnome]
---

I've written a super-simple panel applet for Gnome which displays current CPU load and memory consumption. The applet is supposed to be a space-conserving replacement of the default (and ugly) system monitor applet shipped with Ubuntu. Take a look at the picture of the new applet in action.

![Compact System Monitor Appliet for Gnome](/images/compact-sysmon-screenshot-highlight.png)

Now compare the above applet with the default one.

![Default System Monitor Appliet for Gnome](/images/default-sysmon-screenshot-highlight.png)

I've highlighted both applets so that they're easier to spot. Notice the compactness of the new applet: it takes less than the third of the space the old applet took, while it still displays all the information I need: the CPU load per processor (the outer circle) and memory consumption (the inner one). There are actually two arcs in the inner circle, though unfortunately the separation space is only barely visible. The bars show the amount of physical memory used by applications and the physical memory dedicated to the cache.

The look and feel of the applet is currently hard-coded (notably the colors of arcs are set to white) so that they would match the color scheme used by the default theme of Ubuntu 10.04. It also fails to properly render its background, so it only works correctly for default panel sizes. I'll fix these problems when I have more time. In the mean time, if you like the applet and can live with the limitations, feel free to download and use it.

Get the [sources from the repository][1]. Note that on Ubuntu, you'll need libpanel-applet2-dev to build the applet.

  [1]: http://ratatanek.cz/hg/gnome-compact-sysmon-applet/

