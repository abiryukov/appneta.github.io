---
layout: content
title:  "tcpreplay-edit"
categories: tcpreplay wiki
---

- [Overview](#overview)

<h2><a name="overview"></a>Overview</h2>
[tcpreplay] has evolved quite a bit over the years. In the 1.x days, it merely read packets and 
sent then back on the wire. In 2.x, *tcpreplay* was enhanced significantly to add various 
rewriting functionality but at the cost of complexity, performance and bloat. 
In 3.x, *tcpreplay* has returned to its roots to be a lean packet sending machine and
the editing functions have moved to [tcprewrite] and a powerful *tcpreplay-edit* which
combines the two. In 4.0 some editing capabilities where added back into *tcpreplay* but only
in the case where performance was not affected (`--unique-ip).

Since *tcpreplay-edit includes all the functionality of both *tcpreplay* and *tcprewrite*,
please see those wiki pages for how to use *tcpreplay-edit*.

Lastly, please remember that the packet editing code has some overhead- even when not in use.
Hence, for the highest performance I always recommend using *tcprewrite* and *tcpreplay*
separately.

[tcprewrite]:          tcprewrite.html
[tcpreplay]:           tcpreplay.html
[tcpprep]:             tcpprep.html
