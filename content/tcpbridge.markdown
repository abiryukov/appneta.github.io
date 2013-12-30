---
layout: content
title:  "tcpbridge"
categories: tcpreplay content
---

- [Overview](#overview)
- [Notes](#notes)

<h2><a name="overview"></a>Overview</h2>
*tcpbridge* allows you to connect two network segments and bridge them. *tcpbridge* emulates 
a learning bridge (it learns which MAC addresses are on each side in order to prevent 
broadcast storms) and allows a variety of packet editing features in the same way as [tcprewrite].

<h2><a name="notes"></a>Notes</h2>
1. *tcpbridge* works pretty well joining two ethernet segments.
2. I haven't had much luck spanning wireless segments, 
but this tends to be very OS/hardware/driver dependant
3. In general, don't expect great performance
4. Linux performance should be better then under Windows, OS X, etc
5. I really haven't done much testing of tcpbridge under Windows, so bug reports are welcome
6. Using the packet editing functionality with tcpbridge is possible, but lets face it, 
it isn't likely to work out very well because tcpbridge isn't stateful.

[tcprewrite]:          tcprewrite.html
